---
layout: post
title: "Selecting a Mac EDR agent"
categories: saas, security, malware, edr, endpoints
date: "30-06-2021"
---

My organisation recently migrated all its workstations from Windows boxes to MacOS devices.
The Windows fleet was equipped with a remotely managed anti-virus software.
We needed to maintain a similar level of protection, so I had to select the security agent that would be deployed on the new Mac fleet.

I spent a few days testing security software for Mac, and I'll be sharing with your my thought process on the matter.

{:refdef: style="text-align: center;"}
![Icons made by Freepik, from www.flaticon.com](images/malware_v_mac.png "Icons made by Freepik, from www.flaticon.com"){:height="300px"}
{: refdef}


## [Context](#context)

### [Why move to Mac?](#why-move-to-mac)
The move from Windows to Mac boxes was driven by the IT department for various reasons that will not be discussed in this blog post.
We will be focusing solely on the security side of things.

### [AV or EDR?](#av-or-edr)
A regular AV coupled with regular network and proxy monitoring might be good enough for an organisation where everybody works from the office, or joins the corporate network with a VPN.
That was not our case.
Our workforce is highly distributed, and our organisation has taken many steps down the path of the [BeyondCorp paradigm](https://www.beyondcorp.com/).
As a result, we decided to go with an EDR agent instead of a regular AV solution, because it allowed us greater visibility as to what was going on in our fleet, as well as better response capabilities in the case of an incident.

### [Is this a paid product review?](#is-this-a-paid-product-review)
No. This is not a paid product ad, nor is it a thorough, scientific review of the security products I'll be presenting.
Each organisation has its own requirements and resources.
Those that apply to mine, might not necessarily apply to yours.

Also, this work was done in March 2021.
The findings presented reflect my experience with the tested products at that specific moment in time.

## [Selection process](#selection-process)

We followed a simple 3 step process.

### [Step :one:](#Step-one)

> <h4><b>Decide what was important to us, and how important it was.</b></h4>


So I prepared a comparison table with all our requirements, and set a priority for each item using the [MoSCoW method](https://en.wikipedia.org/wiki/MoSCoW_method).

| Category | Criteria                                 | Priority  | Comment                                                                                    |
|:---------|:-----------------------------------------|:----------|:-------------------------------------------------------------------------------------------|
| Detection| Adequate protection against common known malware | must | <span style="color:#ffcc33">Needs testing</span> |
| GRC      | Data processing location                 | must      | EU only |
| GRC      | Data storage duration                    | must      | 6 months at most |
| MOC      | Ease of installation                     | should    | Using our Mac [MDM](https://en.wikipedia.org/wiki/Mobile_device_management). <span style="color:#ffcc33">Needs testing</span>. |
| MOC      | Agent auto-update                        | must      | |
| MOC      | Fast releases in case of OS updates      | could     | In case Apple rolled out a new OS, we wanted to be able to update our fleet ASAP |
| MOC      | Low workstation resource usage (CPU, RAM)| should    | We don't want to cripple our fleet and annoy our colleagues. <span style="color:#ffcc33">Needs testing</span> |
| Logs     | Syslog integration                       | must      | The bare minimum required to plug this data into a SIEM |
| Logs     | S3 Bucket integration                    | should    | Our preferred method of SIEM data ingestion |
| Logs     | ElasticSearch integration                | could     | |
| RBAC     | SSO integration for admins               | should    | Allows admins to log in using our usual 2FA method |
| RBAC     | RBAC for EDR admins                      | must      | |
| RBAC     | Role-based EDR agent configuration       | must      | We need different EDR configurations for different populations (engineers & non-engineers) |
| DFIR     | Workstation quarantine                   | must      | Allows us to isolate a machine using its local firewall |
| DFIR     | Workstation shell access                 | could     | Nice to have feature in case of a big incident, but otherwise a rather risky feature |
| DFIR     | Malicious file quarantine                | must      | |
| DFIR     | IP and/or URL blocking                   | could     | |
| DFIR     | On-demand scans                          | should    | |
| Usability| User friendly admin interface            | should    | <span style="color:#ffcc33">Needs testing</span> |
| Usability| Easily identifiable workstation user     | must      | |

### [Step :two:](#step-two)

> <h4><b>Find EDR providers and, using public information, narrow the list down.</b></h4>

I looked up various EDR solution providers, and found many of the answers to the previously listed criteria. Quite a few did not meet all the "must have" requirements.

This allowed me to narrow the list down to 3 potential candidates:
* [Jamf Protect](https://www.jamf.com/fr/produits/jamf-protect/)
* [CarbonBlack](https://www.carbonblack.com/)
* [SentinelOne](https://www.sentinelone.com/)

However, Jamf Protect's list price was quite a bit higher than that of its competitors. More than double in fact.
So it dropped out of the race, leaving CarbonBlack & SentinelOne as the finalists.

### [Step :three:](#step-three)

> <h4><b>Test the shortlisted solutions and choose one.</b></h4>

To find out which provider was best for us, I needed to answer the questions that could not be answered in step 2.
I specifically focused on addressing the following issues:

#### :point_right: <span style="color:#ffcc33">Adequate protection against common known malware</span>

The solution must detect and stop common malware, with MacOS' [GateKeepter](https://support.apple.com/en-gb/HT202491) deactivated.

I used [Objective-See](https://www.objective-see.org)'s excellent expertise on Mac malware to get a diverse selection of recent, highly dangerous malware samples to throw at the EDRs:

  * [EICAR](https://www.eicar.org) - Dummy payload that was developed to test the response of computer antivirus programs
  * [FinSpy](https://objective-see.com/blog/blog_0x4F.html) - Spyware
  * [ElectroRAT](https://www.intezer.com/blog/research/operation-ElectroRAT-attacker-creates-fake-companies-to-drain-your-crypto-wallets/) - Backdoor
  * [EvilQuest](https://objective-see.com/blog/blog_0x59.html) - Ransomware
  * [XCSSET](https://documents.trendmicro.com/assets/pdf/XCSSET_Technical_Brief.pdf) - Virus
  * [IPStorm](https://objective-see.com/blog/blog_0x5F.html#-ipstorm) - Botnet agent & backdoor

#### :point_right: <span style="color:#ffcc33">Ease of installation</span>
  
Agent installation, uninstallation & updates should be straightforward using our MDM system.

#### :point_right: <span style="color:#ffcc33">Resource utilization</span>

The solution should have the lowest CPU & RAM footprints possible, over a typical workday's workload. That included:
  * Heavy internet browsing
  * Light spreadsheet work
  * Light terminal work
  * A few file downloads in the browser

#### :point_right: <span style="color:#ffcc33">User friendly admin interface</span>

The solution should have an easy to understand and use UI.

## [Results](#results)

I won't go into a tedious description of how each product fared during our benchmarking, and will instead provide a quick overview for each product, as well as a comparison table.

### [CarbonBlack](#carbonblack)

CarbonBlack blocked all the malware samples I used.
It had low resource utilization, a good enough UI, and a reasonable price compared to all the other candidates.

Getting the installation to work well was a little tricky on Mac OS Big Sur, but once we figured it out, it was all plain sailing.

### [SentinelOne](#sentinelone)

Things started out well for SentinelOne. Installation was a breeze.

It turned sour quickly though. It didn't block all the malware I threw at it. It even let ElectroRAT & FinSpy install and run for a while, before it shut them down. I presume it waited until a lot of malicious syscalls were being made before it terminated the offending processes. This would be okay in the case of a new, unknown malware strain, but these were well known samples.

SentinelOne then proceeded to delete all the malware's files without any human intervention. I found this to be quite counter productive, as it would make any investigation a tiny bit more involved.

To add insult to injury, it also had a larger resource footprint than CarbonBlack.

On a brighter note, when I tried to relaunch ElectroRAT & FinSpy, it blocked them immediately! Delightful! It had learned that these binaries were malicious. So I tried another sample ([GravityRAT](https://objective-see.com/blog/blog_0x5B.html)), hoping for more success. Lo and behold, GravityRAT ran happily before eventually getting terminated.

### [Comparison](#comparison)

| Category | Criteria | Priority  | CarbonBlack | SentinelOne |
|:---------|:---------|:----------|:------------|:------------|
| Detection| Adequate protection against common known malware | must   | :heavy_check_mark: | :x: |
| GRC      | Data processing location                         | must   | :heavy_check_mark: | :heavy_check_mark: |
| GRC      | Data storage duration                            | must   | :heavy_check_mark: | :heavy_check_mark: |
| MOC      | Ease of installation                             | should | :heavy_check_mark: | :heavy_check_mark: |
| MOC      | Agent auto-update                                | must   | :heavy_check_mark: | :heavy_check_mark: |
| MOC      | Fast releases in case of OS updates              | could  | :x: | :x: |
| MOC      | Low workstation resource usage (CPU, RAM)        | should | :heavy_check_mark: | :x: |
| Logs     | Syslog integration                               | must   | :heavy_check_mark: | :heavy_check_mark: |
| Logs     | S3 Bucket integration                            | should | :x: | :x: |
| Logs     | ElasticSearch integration                        | could  | :x: | :x: |
| RBAC     | SSO integration for admins                       | should | :heavy_check_mark: | :x: |
| RBAC     | RBAC for EDR admins                              | must   | :heavy_check_mark: | :heavy_check_mark: |
| RBAC     | Role-based EDR agent configuration               | must   | :heavy_check_mark: | :heavy_check_mark: |
| DFIR     | Workstation quarantine                           | must   | :heavy_check_mark: | :heavy_check_mark: |
| DFIR     | Workstation shell access                         | could  | :heavy_check_mark: | :heavy_check_mark: |
| DFIR     | Malicious file quarantine                        | must   | :heavy_check_mark: | :heavy_check_mark: |
| DFIR     | IP and/or URL blocking                           | could  | :x: | :x: |
| DFIR     | On-demand scans                                  | should | :heavy_check_mark: | :heavy_check_mark: |
| Usability| User friendly admin interface                    | should | :heavy_check_mark: | :heavy_check_mark: |
| Usability| Easily identifiable workstation user             | must   | :heavy_check_mark: | :heavy_check_mark: |


## [The winner](#the-winner)

{:refdef: style="text-align: center;"}
![CarbonBlack](images/cb.jpeg "CarbonBlack"){:height="150px"}
{: refdef}


To absolutely nobody's surprise, we decided to go with CarbonBlack.

Once licensing was sorted out, we rolled it out progressively on all workstations.
Configuration went smoothly enough. We grouped machines depending on their users, giving a bit more leeway to our software engineers. We also set up notifications on Slack for faster alert processing.

The EDR has been running in production for a few weeks now, and it's been quite painless. There are a few false positives every now and then, but far from unmanageable.
