---
layout: post
title: "Putting out non-existent fires"
categories: saas, security, authentication, hacker
date: "12-05-2021"
---

###### :information_source: *Disclaimer: These events happened in 2020. All names, dates, company names and technical details have been changed. Some events have been omitted for clarity.*

Today, I'll tell you all about a security "incident" that I had to deal with a while ago at work.

On a random September afternoon, Daniel, a colleague, rings me up.
He tells me that a privileged user account on our B2B SaaS website had been hacked, and that we had received an email from the affected user, Zoe, as well as the CISO of the company she works at.

He sends the emails over.

## [The first few emails](#the-first-few-emails)

> <h5 style="color:#2a7ae2">:incoming_envelope:  22/09/20@06:01pm;&emsp;<u>From:</u>zoe@some-company.com;&emsp;<u>To:</u>support@my-saas.com;</h5>
> Hi,
>
>We're having a bit of trouble with your website today.
My colleague Lola's user account has disappeared and I had to create a new one for her. Also my post from last week has been edited!
>
>Do you have any idea why this is happening?
>
>Zoe

Then I read this email.

> <h5 style="color:#2a7ae2"> :incoming_envelope: 23/09/20@12:17pm;&emsp;<u>From:</u>ciso@some-company.com;&emsp;<u>To:</u>support@my-saas.com;</h5>
>Hi,
>
>I am Some-Company's CISO. I'm taking Zoe's report extremely seriously because the unauthorized access to a Some-Company account can have a major impact on our group's reputation.
>
>To understand what's going on, could you please send me all logs relating to Zoe's and Lola's accounts? That includes source IP addresses and timestamps, as well as any audit logs that you might have.
>
>Can I also have the email and phone number of some engineer I can talk to?
> 
>Thanks,
>
>Vince, Some-Company's CISO

Daniel, the support engineer handling this, replies back saying that the issue is under investigation. It doesn't take long for Vince to reply.

> <h5 style="color:#2a7ae2"> :incoming_envelope: 23/09/20@02:50pm;&emsp;<u>From:</u>ciso@some-company.com;&emsp;<u>To:</u>support@my-saas.com;</h5>
>Hi Daniel,
>
>I understand you're investigating, but in the meantime, can you send all logs relating to Chloe's and Zoe's accounts?
>
>Thanks,
>Vince

Vince seems quite determined to say the least. As a matter of fact, he called Daniel a few minutes after that email to insist on getting what he wanted.

Three hours later, Daniel had finished his investigation, and he sent Vince and Zoe the following email.

> <h5 style="color:#2a7ae2"> :incoming_envelope: 23/09/20@06:15pm;&emsp;<u>From:</u>support@my-saas.com;&emsp;<u>To:</u>ciso@some-company.com,zoe@some-company.com;</h5>
>Hello,
>
>Please find below the results of our investigation regarding the incidents you've reported.
>
>**Modified post**
>
>The post with ID n°450292102 had the following actions.
>
```
| Action  | User | Timestamp                 | IP          |
|---------|------|---------------------------|-------------|
| Edit    | Zoe  | Sept 22nd 2020 @ 16:47:07 | 92.XX.YY.ZZ |
| Edit    | Zoe  | Sept 22nd 2020 @ 16:50:12 | 92.XX.YY.ZZ |
| Hide    | Zoe  | Sept 22nd 2020 @ 16:57:34 | 92.XX.YY.ZZ |
| Show    | Zoe  | Sept 22nd 2020 @ 17:10:46 | 92.XX.YY.ZZ |
| Edit    | Zoe  | Sept 22nd 2020 @ 17:11:37 | 92.XX.YY.ZZ |
```
>
>**User account deletion**
>
>The account linked to Lola@some-company.com was deleted by Zoe@some-company.com, at 16:41:58 the same day, from the address 92.XX.YY.ZZ.
>
>Kind regards,
>
>Daniel, My-SaaS support team

Then, Vince sends 3 emails in quickfire succession.

> <h5 style="color:#2a7ae2"> :incoming_envelope: 23/09/20@06:18pm;&emsp;<u>From:</u>ciso@some-company.com;&emsp;<u>To:</u>support@my-saas.com;</h5>
> Thanks, Can I have the raw logs for both accounts?

> <h5 style="color:#2a7ae2"> :incoming_envelope: 23/09/20@06:46pm;&emsp;<u>From:</u>ciso@some-company.com;&emsp;<u>To:</u>support@my-saas.com;</h5>
> Since you haven't given me the the raw logs yet, I'd like to have your legal counsel's and your DPO's email addresses.

> <h5 style="color:#2a7ae2"> :incoming_envelope: 23/09/20@06:57pm;&emsp;<u>From:</u>ciso@some-company.com;&emsp;<u>To:</u>support@my-saas.com;</h5>
> I'm not asking you for all your users' logs, only Zoe's and Lola's, who already know I need them.
> The data you've provided doesn't show when and whence Zoe & Lola have accessed your website or whether they had their passwords changed. The data is nowhere near complete enough for me to know what Lola & Zoe's accounts did before and after they were hacked.

Daniel has had it with him. He asks me to deal with this.

## [Trying to deal with Vince](#trying-to-deal-with-vince)

I can already hear you say "just give him what he wants and be done with it".
A few of my colleagues did.

As a security engineer at My-Saas, I'm responsible for all our users' security, as well as that of my organization.
I obviously want to help Vince and Zoe figure out what's going on, but I need to ensure that doing so does not put other users or my company at risk.
Subsequently, there are a few things to consider before attempting to deal with the tantrum.

### :point_right: Can raw logs be shared with clients?

I'd argue that system logs are a piece of intellectual property.
A company creating closed source software, such as My-Saas, wouldn't go around sending people its source code, so why would it share logs?

Also, and this is really obvious, logs contain information about the software & systems producing it.
So sharing "raw logs", as requested by Vince, puts My-Saas' systems at risk.

Logs also contain [PII](https://en.wikipedia.org/wiki/Personal_data).
Even the cleanest logs have to include some reference to an IP address, a user ID, etc.
As such, sharing logs generated by a person's activity equates to disclosing information about their actions.
In this case, our app logs contained IP addresses which could be used to track a person's geographical location.

Last but not least, anyone who has ever had a hands-on experience with a large scale SaaS system knows that going through logs can be a nightmare.
It takes time and knowledge of the systems to filter out the logs to get the results you want. You always risk either missing something because you've filtered out too much, or including unwanted data (potentially PII) because you haven't narrowed the scope enough.

You can already guess I'm not overly enthusiastic about sharing "raw logs" with a third party.

### :point_right: Was the user's account really hacked?

Daniel has done a great job so far, but as Vince rightly pointed out, he hasn't explored all possible scenarios.
So, for the sake of completeness, I called two colleagues who had an extensive knowledge of our systems and we did a deep dive in our logs to find any clue pointing to a potential compromise.

There were no changed passwords, no logons from foreign IP addresses, no bruteforce attempts, nothing out of the ordinary really.

The user, Zoe, had 3 sessions open on our SaaS:
* 92.XX.YY.ZZ, geolocated in Troyes, France, registered to a residential internet connection
* 84.XX.YY.ZZ, geolocated in Paris, registered to a residential internet connection
* 107.XX.YY.ZZ, registered to a telecom operator's mobile network

The suspicious activity comes from IP address 92.XX.YY.ZZ, which is not geolocated in Paris, where Some-Company's headquarters are, and where Zoe probably lives.
So there is a possibility that a malicious actor from Troyes could have accessed Zoe's account.

If it really was the case though, why would the attacker "just" edit a post and delete another user's account?
The attacker could do anything: spam contacts, launch phishing campaigns, publish reputation damaging posts; yet they did none of that.
They could, theoretically, sleep on this for a while, to prepare for a more sophisticated assault. But they messed around and got detected.
What kind of rubbish attacker would do that?

### :point_right: Has My-Saas' security been compromised?

Let's imagine for a second that Zoe's account was indeed hacked. Could it be because My-Saas itself was compromised? Did we have any suspicious activities on our apps, or platform?

Well, simply put, no. No particular IoCs to be found. All systems were operational and I had no alerts in my inbox. 

### :point_right: What are our legal & contractual obligations?

At this point, even if I wanted to send Vince the "raw logs" he so desperately wants, I did not know what to send because there wasn't anything interesting to look at.
And I obviously wasn't going to send over hundreds of GBs worth of system logs just to prove there wasn't anything to find there.

Given that he made a thinly veiled attempt at threatening us with legal action, I got in touch with our legal department to find out more about what the applicable laws and our contract required us to do.

My friends over at the legal department and I went over the existing contract and applicable laws and we couldn't find anything that compels us to provide any logs.
The contract had clauses regarding information security, but nothing that could be applied to the situation at hand.
To be honest, I would have been surprised if there was a law forcing us to disclose intellectual property to anyone who asks.

[GDPR's article 15](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32016R0679&from=EN#d1e2513-1-1) guarantees individuals' right to access their data, but it says nothing about raw logs.
Also, GDPR emphasizes the importance of using "accessible, plain and clear" language whenever communicating with individuals.
Here, Vince is not the concerned data subject, nor are system & app logs "accessible, plain and clear".

The only way he could coerce us into providing raw logs was through a court warrant, and he had no legal case.

### :point_right: Why is he being so relentless?

I'll probably never know why Vince, who had apparently spent 20 years working on IT security governance for the French military, was hell-bent on getting us to obey his commands. ¯\\_(ツ)_/¯

### :point_right: Bonus: renegotiating the deal with Some-Company 

I'm knee deep into this whole shebang, when a colleague from the sales team, Sara, sends me a message on Slack.
> :woman: Hi :wave:
>
> :woman: I just wanted to let you know that we're currently renegotiating our contract with Some-Company, so it would be great if we could solve this quickly and move on.

I was not expecting that. Now we have to figure out a way of handling this without jeopardizing the negotiations. Tough ask.

### :point_right: Bonus combo: Poor opsec

> :man: Hi Sara, thanks for the info!
>
> :man: To be honest, we're in a bind here. Vince is pushing us hard, but we cannot give him what he wants without putting other users' security at risk. And I really don't think Zoe's account was hacked. I mean, why would a supposed hacker from Troyes mess around with her posts.
>
> :woman: Troyes? Wait a second, Zoe was in Troyes a few days ago!
>
> :man: Come again?
>
> :woman: Well yes, I tried calling her for an unrelated subject on Monday. I needed to set up a meeting with her, but she told me she was in Troyes, and would only be available when she got back to Paris on Tuesday.

It can be surprising how quick chats between colleagues can help sometimes.
Thanks to this information, I could finally articulate a legitimate working hypothesis:

:arrow_right: **The user left her session open on a shared computer, and another user messed around with the session.**

It wouldn't be outlandish to imagine she went to visit family over the weekend, and used someone else's computer for remote work on Monday 21st September.
All of this could simply be a case of poor [opsec](https://en.wikipedia.org/wiki/Operations_security).

## [Actually dealing with Vince](#actually-dealing-with-vince)

While I was investigating and researching the issue. Vince was getting all worked up. He followed up on his 3 previous emails sent after office hours with another one the next day.

> <h5 style="color:#2a7ae2"> :incoming_envelope: 24/09/20@11:06pm;&emsp;<u>From:</u>ciso@some-company.com;&emsp;<u>To:</u>support@my-saas.com;</h5>
> Hi,
>
> I would like to ask you again to share the raw logs relating to the two accounts affected by the hack. 
> In addition, could I have the emails and phone numbers of your DPO and your CISO?
>
> Vince

I replied with all the information I had, explained he wouldn't be getting any "raw logs", and thanked him for his patience while we were investigating.

He seemed quite disappointed that we wouldn't satisfy his every whim. He threatened with reporting us to the GDPR regulation authority, asked a whole lot of questions about our systems' security, and demanded the "raw logs".

The next morning, our DPO received from Zoe a data access request under GDPR's article 15. I was happy to let the DPO deal with that.

## [What we learned](#what-we-learned)

### Traceability is key

My organization could not have handled this incident if we did not have the right traceability tools at our disposal.
Ensuring systems logs are complete, centralized, automatically analyzed and available for investigation in a SIEM is a must for any SaaS.

User oriented [audit trails](https://en.wikipedia.org/wiki/Audit_trail) are also a great tool to help users manage their own security.

### Security professionals are funny

IT Security professionals usually have to work with other people to improve an organization's security.
So I don't really understand why get on their high horses.
Nobody gets an endorphin rush when they do something they were *ordered* to do.

If you're in this field, you'll get much greater cooperation when people are happy to help you.
You'll also get better results, or at least won't be disappointed, when you know what you are, or are not, entitled to ask for. 

### Team-work saves the day

Go have a coffee with teams you've never spoken to before, get to know them better. It will eventually come in handy.

## [Final words](#final-words)

Security incidents are trying times.
They stress your organization, its tools & processes.
Having user friendly tools and trained incident response engineers means the organization can swiftly respond to the threat by assessing it, containing it, and remediating any ill effects.

In this instance, the real risk for my organization was losing a client, but I did take this opportunity to improve our internal security practices for next time.
