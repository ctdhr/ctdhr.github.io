---
#
# By default, content added below the "---" mark will appear in the home page
# between the top bar and the list of recent posts.
# To change the home page layout, edit the _layouts/home.html file.
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
#
layout: home
title: "Blog posts"
---

<a href="https://dilbert.com/strip/2017-12-05" style="margin:auto;width:50%">
![Dilbert vs. Elbonian hackers](https://assets.amuniversal.com/5af325a0b04a0135ff38005056a9545d "Dilbert vs. Elbonian hackers")
</a>

&nbsp;

## [Selecting a Mac EDR agent](pages/2021-06-30-selecting_mac_edr/index.md) <span style="font-size:60%;color:#a0a0a0">30th June 2021</span>

My organisation recently migrated all its workstations from Windows boxes to MacOS devices.
The Windows fleet was equipped with a remotely managed anti-virus software.
We needed to maintain a similar level of protection, so I had to select the security agent that would be deployed on the new Mac fleet.

## [Putting out non-existent fires](pages/2021-05-12-putting_out_fires/index.md) <span style="font-size:60%;color:#a0a0a0">12th May 2021</span>

On a random September afternoon, Daniel, a colleague, rings me up.
He tells me that a privileged user account on our B2B SaaS website had been hacked, and that we had received an email from the affected user, Zoe, as well as the CISO of the company she works at.

## [Laying log-pipes with Lambda](pages/2020-12-19-logging_lambda/index.md) <span style="font-size:60%;color:#a0a0a0">19th December 2020</span>

If you ask the devops team running a SaaS service about the biggest weakness in their current production infrastructure, chances are they'll tell you "monitoring and logging".
When the pressure to deliver is high on a devops team, as with any team, projects that do not directly contribute to serving stakeholders are put on hold, ergo the previous answer.

## [Build me a secure SaaS app](pages/2020-05-30-saas_security/index.md) <span style="font-size:60%;color:#a0a0a0">7th June 2020</span>

When applying for an IT engineering job, meeting a few people at the target company and answering some technical questions is standard practice. Things get a bit more interesting when you have to present a security roadmap to all the company's technical leads.

## [Auditing AD user passwords](pages/2019-12-04-ad_passwords/index.md) <span style="font-size:60%;color:#a0a0a0">4th December 2019</span>

Say you're a blue teamer in an organisation of a few thousand people. Unless you're in a [kubernetized](https://kubernetes.io/), SaaSified and cloudified startup, you're likely to have an [Active Directory](https://en.wikipedia.org/wiki/Active_Directory) running. ADs and their accounts are a prime target for attackers, so how do you go about insuring your assets are a bit safer? Eliminating weak passwords is a start.


## [Handling logs on AWS](pages/2019-08-19-aws_logs/index.md) <span style="font-size:60%;color:#a0a0a0">19th August 2019</span>

As we migrate our IT systems to the AWS cloud, it is imperative for us to be able to monitor their health and security. AWS does provide a range of tools for logging, however they feel fragmented and balky at times. Looking through logs in the dedicated AWS services was awkward at best, frustrating at worst, and definitely time-consuming. We needed a centralised, robust log storage and management infrastructure that we could control. So I built one!
