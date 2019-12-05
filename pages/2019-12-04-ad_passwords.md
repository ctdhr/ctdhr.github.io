---
layout: post
title: "Auditing AD user passwords"
categories: IT, security, active directory, passwords, audit, pentesting
date: "04-12-2019"
---

Say you're a blue teamer in an organisation of a few thousand people. Unless you're in a [kubernetized](https://kubernetes.io/), SaaSified and cloudified startup, you're likely to have an [Active Directory](https://en.wikipedia.org/wiki/Active_Directory) running. An AD and its accounts are a prime target for attackers, so how do you go about insuring your assets are a bit safer? Eliminating weak passwords is a start.

## AD accounts are a target

### Context

In a relatively modern enterprise setting, an AD serves as the backbone for any and all authentication. It has a central and powerful role in an organisation and it's easy to see why penetration testers and attackers nearly always reach for them.

They're often set up as the [Identity Provider](https://en.wikipedia.org/wiki/Identity_provider_(SAML)) in an [SSO](https://en.wikipedia.org/wiki/Single_sign-on) paradigm. As a result, gaining access to an AD account generally allows access to other services available on the enterprise network or on the internet. Getting your hands on a [Domain Admin](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-domainadmins) account gives you greater power as it has large privileges over other assets (accounts, workstations, [GPOs](https://docs.microsoft.com/en-us/windows-server/networking/branchcache/deploy/use-group-policy-to-configure-domain-member-client-computers) etc.).


### Weak passwords 

Securing an AD and its accounts is an entire field of expertise, and cannot be condensed in a blog post. One small part of this field is making sure your users don't get their accounts stolen.

To do that, you either:
 * use mutli-factor authentication (MFA)
 * try to enforce robust high-quality passwords

MFA greatly mitigates the risk of an account being stolen. Yet, large organisations are reluctant to adopt it. MFA requires changing the authentication process, and change is hard. Implementing organisation-wide MFA assumes that all members have access to some type of [token](https://en.wikipedia.org/wiki/Multi-factor_authentication#Possession_factors), which is not necessarily true and sometimes hard to have. As a result, your organisation might still rely on good old password based authentication for its members. It might even force password rotation every few months.

By default, AD provides admins with the possibility of selecting complexity rules for user passwords :
 * length rules (eg. password must be 10 characters or longer)
 * required character cases rules (eg. password must contain at least one number and one special character)
 * remembered passwords rules (eg. password must not be identical to one of the last 5 passwords)

This approach is less than ideal for the following reasons:
 * "my-password1" is valid
 * if password rotation is enabled, it's highly likely users will keep the same radical in their password and simply change a suffix

So your organisation probably has scores of weak passwords. What do you do now?

## Automated auditing for the win

The first step in eliminating weak passwords is knowing the weak passwords. To find the weakest passwords, you have to do some kind of password cracking attack.

Not everyone has access to a [cluster of 448 Nvidia RTX 2080 with hashcat installed](https://twitter.com/TerahashCorp/status/1155128018156892160) though. But that's not a problem since a modern AD stores passwords on disk in [NTLM hash format](http://www.adshotgyan.com/2012/02/lm-hash-and-nt-hash.html). For compatibility reasons, it also stores LM hashes by default. And both those hashes are rather quick to compute on a CPU.

So I've written a [PowerShell](https://en.wikipedia.org/wiki/PowerShell) script that launches a dictionary attack on all user passwords on an AD, and then outputs the results to a text file. [The code is available on Github](https://github.com/ctdhr/AD-Password-Auditor).

It uses the excellent [DSInternals](https://github.com/MichaelGrafnetter/DSInternals) module and can run multiple threads to accelerate the cracking process. It does require a dictionary however. I've used [Mentalist](https://github.com/sc0tfree/mentalist) to generate the dictionaries used in my attacks. You can use various wordlists as well.

### Recommendations

I highly recommend running this tool on a machine separate from the actual AD server, as it consumes a lot of CPU horsepower. It also makes sense to run this on a machine only accessible to trustworthy persons since the results will be written directly to disk.

## Takeaway

Running an automated password audit regularly allows me to track the weakest passwords in my organisation and identify the points I need to spend time on during my next security awareness trainings.

Keeping tabs on the weakest passwords in your organisation can also be invaluable during incident response investigations.
