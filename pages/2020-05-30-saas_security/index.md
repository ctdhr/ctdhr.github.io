---
layout: post
title: "Build me a secure SaaS app"
categories: IT, security, SaaS, AWS, roadmap, interview
date: "07-06-2020"
---

When applying for an IT engineering job, meeting a few people at the target company and answering some technical questions is standard practice. Things get a bit more interesting when you have to present a security roadmap to all the company's technical leads.

## Interviews

A few months ago, I applied to a security engineering role at a French scale-up, let's call it ACME. I went through the first couple of interviews and everything was going fine. For the third one, I was sent a case study to complete beforehand.

### The case

> Your company has multiple front-end applications that expose private customer's data.
>
> Those applications are served by multiple back-end micro-services hosted on an AWS
infrastructure using Docker and ECS, in a single region.
>
> The applications make use of static assets served by an S3 bucket.
>
> There's no strong certification to be compliant with, but the main objective is to be as close as
possible to industry standards (SaaS B2B, web application, distributed worldwide).
>
> Going from scratch, propose and apply an organization's data privacy principles on:
> * Application level
>    - Authentication mechanism
>    - Configuration and secrets management strategy
>    - Logging strategy
> * Service to service communication
>    - Apply end to end encryption
> * Customer data management
>    - Encryption at-rest and how to implement it on all layers
> * Network infrastructure
>    - Cloud network architecture
>    - Offices network architecture
> * IT acceptable use policy
> * Physical facilities access policy
> * Incident response management on data breach
>
> One possible output would be a presentation (15 minutes max) with a few slides on the aforementioned topics to explain the choices you made (diagrams are useful too). The goal is to share guidelines with ACME's team in order to prepare for implementation next year! Giving context and arguments will also help improve the company's security culture.
>
> Feel free to ask as many questions as you want to understand the problem and let's schedule a call in the next few days for you to answer these questions.

<span style="font-size:60%;color:#a5a5a5">Text was modified to remove company name and grammar errors.</span>

This "case" covers a very wide range of topics _and_ goes into the technical nitty gritty of building a cloud based SaaS app's security paradigm. Preparing an answer takes much more than the 2 hours budgeted by ACME, and the answer definitely won't fit in a 15 minute presentation.

<style>
.title-rectangle{
    width: 93%;
    height: 28px;
    padding: 18px;
    background-color: #82b1ff;
    vertical-align: top;
    border-top: 1px #2979ff;
    border-left: 1px #2979ff;
    border-right: 1px #2979ff;
    border-bottom: 8px #2979ff;
    border-style: solid;
}
.title-rectangle:before{
    content: "";
    display: block;
}

.flex-rectangle{
    width: 93%;
    min-height: 360px;  
    padding: 18px;
    border-top: 0px #2979ff;
    border-left: 1px #2979ff;
    border-right: 1px #2979ff;
    border-bottom: 1px #2979ff;
    border-style: solid;
}
.flex-rectangle:before{
    content: "";
    display: block;
}
</style>

## The answer

It took me 2 days to prepare the slide deck, so I'm sharing my work with you.

### Disclaimer

Each organization has specific requirements and resources, a one-size-fits-all approach will not work.

The presentation below is a collection of best practices that are broadly applicable to a software service delivery infrastructure running in a cloud environment.
This is **NOT** a blueprint of what should be implemented in your organization.
It does not take any resource constraints into consideration and tries to cover as many topics as possible.


If you are looking to replicate the suggestions made in this post in your organization, remember that security is a team sport: one person cannot cover all of these issues.

And finally, this was written in May 2020, if new products, tools, or best practices are released, or if you're an expert in a particular subdomain and have ideas on improving a company's security, do share them with me so I can update this post.

Now, without further ado, let's dive right in.

<div class="title-rectangle" markdown="1">
### Overview
</div>
<div class="flex-rectangle" markdown="1">

Improving an [SMB](https://en.wikipedia.org/wiki/Small_and_medium-sized_enterprises)’s security stance requires sustained efforts on multiple fronts

[GRC](https://en.wikipedia.org/wiki/Governance,_risk_management,_and_compliance), keeping on top of
* Legal obligations
* Compliance obligations
* Contractual obligations

Tech-wise
* Keeping up with quickly changing technologies
* Implementing GRC requirements
* Reducing overhead due to security measures and improving
workflow integration

> IT security is not a state, it’s a discipline
</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Starting with GRC
</div>
<div class="flex-rectangle" markdown="1">
1\. Identify regulatory, contractual and compliance requirements. You might need to comply with
  * [GDPR](https://en.wikipedia.org/wiki/General_Data_Protection_Regulation)
  * Legislation applicable to ACME's business environment
  * Security standards ([ISO27001/17/18](https://en.wikipedia.org/wiki/ISO/IEC_27000-series), [SOC1/2/3](https://en.wikipedia.org/wiki/System_and_Organization_Controls), [CSA Star](https://cloudsecurityalliance.org/star/), [HDS](https://esante.gouv.fr/labels-certifications/hebergement-des-donnees-de-sante) etc.)
  * Contractual requirements are highly variable depending on the
client, but usually have a common set or rules

2\. Risk assessment
  * Given the GRC context, identify risks weighing on your business
and data
  * Your clients might ask you for this
</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Starting with GRC
</div>
<div class="flex-rectangle" markdown="1">

3\. Write everything down (non-exhaustive list)
  * **Information Systems Security Policy** sets security requirements (availability,
integrity, confidentiality, proof) and how to implement them
  * **Business Continuity Plan**
  * **Personal Data Security Policy** sets personal data handling rules within ACME
  * **Privacy Statement** clarifies ACME’s handling of personal data to clients and their respective clients if necessary
  * **IT Acceptable Use Policy**


</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### GRC / IT Acceptable Use Policy
</div>
<div class="flex-rectangle" markdown="1">
#### IT Acceptable Use Policy
  * Signed by employees as they join ACME
  * Separate from the Information Systems Security Policy
  * Sets IT resource usage rules for ACME employees
  * Sets simple rules, including some simple security rules applying to all employees
    - "Don’t use company resources for illegal/immoral activities"
    - "Don’t disclose company data"
    - "Don’t store company/client data on the USB you give to your son/daughter"
    - "Don’t hack us"

</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### GRC / Incident response
</div>
<div class="flex-rectangle" markdown="1">

#### Incident management procedure
 * Assess
    - Understand what happened, extent of damage
    - If necessary, set up crisis management unit (CISO, [DPO](https://en.wikipedia.org/wiki/Data_Protection_Officer), Account Manager, IT experts, management, etc.)
    - Determine next steps
      * Call clients?
      * (Have clients) call Data Protection authority?
      * External help? (legal, IT forensics, etc.)
      * Save/dump data for legal proof/later forensics
 * Contain
    - Limit/stop damage within business requirements (availability, data loss etc.)
 * Remediate
    - Identify root causes and possible solutions
    - Implement solutions and monitor effectiveness
 * Improve
</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### GRC / Physical spaces
</div>
<div class="flex-rectangle" markdown="1">
#### Offices

* All services (internal and client facing) are hosted on the cloud
* No data, source code, or infrastructure hosted on premises
* [BeyondCorp](https://en.wikipedia.org/wiki/BeyondCorp) zero-trust approach, office LAN is a mere internet access
* Visitors allowed only when identified and accompanied
* 24/7 surveillance, alarm system, and on-site security for employees’ physical safety

</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### GRC / Miscellaneous
</div>
<div class="flex-rectangle" markdown="1">

#### Ensure

* The DPO is involved in the development of new features that impact data privacy
* Personal data is labelled as such, wherever it may be stored on the IT infrastructure
* Personal data has an expiration date beyond which it’s automatically deleted
* Security audits are carried out regularly
  - Annually
  - Clients *will* ask for the latest audit/pentest results
* Employees’ access accounts are adequately managed (and revoked)
* Slack’s free tier is not used, it’s a major compliance risk

</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / Networks
</div>
<div class="flex-rectangle" markdown="1">
#### AWS Account architecture

* Production account
    - Production [VPC](https://aws.amazon.com/vpc/) and assets
* Development account
    - Staging VPC and assets
    - Separate demo environment
    - Dev sandbox VPC
* Security/logs account
    - Used to store logs from all accounts
* Master account
    - Billing
    - [Cloudtrail](https://aws.amazon.com/cloudtrail/) master

</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / Networks
</div>
<div class="flex-rectangle" markdown="1">
#### Static content

* Publicly accessible S3 bucket serves assets and the front-end framework
* CloudFront [CDN](https://en.wikipedia.org/wiki/Content_delivery_network) to reduce worldwide latency for static assets
* Careful not to store any important information here

#### Production VPC

* Every subnet has its own default [SG](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) and [NACL](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)
* Add endpoints depending on subnet use (S3, STS, SQS, RDS etc.)
* Accessing AWS console, APIs and resources requires VPN + MFA
* Staging VPC in separate account deployed identically thanks to [Terraform](https://www.terraform.io/)

</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / Networks
</div>
<div class="flex-rectangle" markdown="1">
#### Production VPC simple diagram

```
 +---------Internet GW-----------VPN TGW-------------------------------+
 |              |                    |                                 |
 |         LB / API GW               |                                 |
 |              +--------------------)-----------------+               |
 |              |                    |                 |               |
 |  +----------------------+  +-----------+  +----------------------+  |
 |  |                      |  |           |  |                      |  |
 |  |   Public subnet      |  |           |  |    Public subnet     |  |
 |  |                      |  |           |  |                      |  |
 |  +----------------------+  |           |  +----------------------+  |
 |                            |  Bastion  |                            |
 |  +----------------------+  |   EC2     |  +----------------------+  |
 |  |                      |  |           |  |                      |  |
 |  |   Private (apps)     |  | Admin     |  |    Private (apps)    |  |
 |  |                      |  | Subnet    |  |                      |  |
 |  +----------------------+  +-----------+  +----------------------+  |
 |                                                                     |
 |  +----------------------+  +-----------+  +----------------------+  |
 |  |                      |  |           |  |                      |  |
 |  |   Private (data)     |  |           |  |    Private (data)    |  |
 |  |                      |  |           |  |                      |  |
 |  +----------------------+  |           |  +----------------------+  |
 |                            |           |                            |
 |  +----------------------+  |           |  +----------------------+  |
 |  |                      |  |           |  |                      |  |
 |  |   Protected (data)   |  |Monitoring |  |    Protected (data)  |  |
 |  |   Optional           |  | Subnet    |  |    Optional          |  |
 |  +----------------------+  +-----------+  +----------------------+  |
 |        AZ 1                                       AZ 2              |
 |                              AWS Region                             |
 +---------------------------------------------------------------------+
```
</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / Networks
</div>
<div class="flex-rectangle" markdown="1">
#### DDoS
* Necessary to avoid service outage from a small attack
* AWS, CloudFlare, OVH, [6Cure](https://www.6cure.com/en/)
* Different technologies for different use cases

#### WAF
* [Blocks suspicious API requests](https://en.wikipedia.org/wiki/Web_application_firewall)
* Requires maintenance and monitoring
* Papers over the cracks that the apps might have
* Plugs into the [SIEM](https://en.wikipedia.org/wiki/Security_information_and_event_management)

</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / Networks
</div>
<div class="flex-rectangle" markdown="1">
#### Offices

* No LAN resources, just an internet access
* AWS service delivery infrastructure accessible via VPN
* Business applications, IM & file sharing -> Accessible online with ACME [SSO](https://en.wikipedia.org/wiki/Single_sign-on) account
* On-site network security necessary
    - Preventing [arpspoof](https://en.wikipedia.org/wiki/ARP_spoofing), [rogue DHCPs](https://en.wikipedia.org/wiki/Rogue_DHCP), [VLAN hopping](https://en.wikipedia.org/wiki/VLAN_hopping) and other network attacks with modern network equipment
    - On-site recursive DNS server and monitoring outbound DNS requests
    - Monitor FW
    - On-site NIDS [Suricata](https://suricata-ids.org/) (on a dedicated VLAN)
    - Everything routes back to corporate SIEM via a VPN
    - Going one step further : Per user VLAN & ACME LDAP-based 802.1x

</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / Authentication
</div>
<div class="flex-rectangle" markdown="1">
#### Admins
* LDAP+SSO of some sorts (GSuite LDAP, Okta etc)
* Long passwords
* VPN + MFA to access production with STS
* Passwords management solution for all ACME employees, eg
  - Self hosted [bitwarden](https://bitwarden.com/) instance
  - [Keepass](https://keepass.info/)

#### Users
* ACME apps must provide logins through
  - built-in accounts
    * Long passwords
    * MFA if the client wants it
  - SSO integration with clients’ Directories (client handles authentication)

</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / Securing data end to end
</div>
<div class="flex-rectangle" markdown="1">
* Users connect to the internet facing API Gateway (reverse proxy) with TLS (with strong cipher suites)
* Internal [PKI](https://en.wikipedia.org/wiki/Public_key_infrastructure) allows inter-service TLS encrypted communication with mutual auth for synchronous HTTP calls
* Asynchronous calls (RabbitMQ, SQS etc) can also be encrypted in transit and at rest
* Encryption certs/keys management must be automated
* Databases (RDS) can also encrypt data (storage encryption)
* Database encryption (non-storage) may carry a performance penalty
* S3 stored objects can be encrypted with [SSE](https://docs.aws.amazon.com/AmazonS3/latest/dev/serv-side-encryption.html) or [KMS](https://aws.amazon.com/kms/) [CMK](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#master_keys)
* EBS & EFS volumes (for EC2s or ECS) must be encrypted (eg. with SSE)
</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / App logs
</div>
<div class="flex-rectangle" markdown="1">
#### ACME's apps' logs
* Consistent structure across all apps and logs
  - Use precise timestamps (ntp sync’d)
  - Error IDs
  - JSON format
* Log levels (INFO, DEBUG, WARN, ERROR)
* Human (and non dev) readable description is a must
* No personal data in logs thanks to developer awareness
</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / Infrastructure Logs
</div>
<div class="flex-rectangle" markdown="1">
* Log all the things to S3 in the log account. Use SSE & lifecycles
* Main AWS services:
  - Cloudtrail
  - Cloudwatch Logs
  - Config
  - ELBs
  - Lambdas
  - etc.
* Use a syslog server (eg. fluentd) for
  - EC2
  - Containers
* Channel all the logs to a SIEM (eg. [Splunk](https://www.splunk.com/), [ELK SIEM](https://www.elastic.co/siem)) (see [previous blog post](/pages/2019-08-19-aws_logs))
  - Configure SIEM alerts & dashboards
</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / SIEM architecture
</div>
<div class="flex-rectangle" markdown="1">
* Collection / Indexing / Viewing
* Collectors close to the resources
* Indexers are clustered, replicated and fault tolerant
  - Data stored persistently, replicated across AZ
  - Indexing machines part of immutable architecture
  - Indexer location must comply with GRC requirements
* Viewers (eg. Kibana, or Splunk Search Heads) replicated and
fault tolerant
  - Alerts are vital for a SOC to function properly
</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / Backups & BCP
</div>
<div class="flex-rectangle" markdown="1">
#### As part of the Business Continuity Plan / Disaster Recovery Plan

* RDS
  - RDS backed-up to multiple different AZ, or regions
  - Test RDS backup restoration in the staging environment
* S3
  - S3 is highly fault tolerant, can be backed up to Glacier if really necessary (data [lifecycles](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-lifecycle-mgmt.html) still apply)
* EBS / EFS
  - Backup data using [rsnapshot](https://rsnapshot.org/) / [duplicity](http://duplicity.nongnu.org/) to separate regions / AZ
  - Volume snapshots to separate regions / AZ ([AWS Backup](https://aws.amazon.com/backup/) could be useful)
</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / Backups & BCP
</div>
<div class="flex-rectangle" markdown="1">

* Code and machines
  - Use [immutable infrastructure](https://www.hashicorp.com/resources/what-is-mutable-vs-immutable-infrastructure/) with Terraform/Cloudformation, Ansible/Saltstack, Packer etc.
    * Machine images ([AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html), docker registry images) can be easily redeployed
  - Save code and config files on a version control service
    * If self hosted, back it up to external location
    * If SaaS, ensure contract is acceptable, and still back it up to an external location
</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / DevOps security
</div>
<div class="flex-rectangle" markdown="1">
#### Security tools in software production chain

* Use a reputable security framework to handle security sensitive workloads like [RBAC](https://en.wikipedia.org/wiki/Role-based_access_control), authentication/2FA etc (eg. [Spring Security](https://spring.io/projects/spring-security), [Yosai](https://github.com/YosaiProject/yosai) etc.)
* Use [SonarQube](https://www.sonarqube.org/) and select security rules/plugins to check code security, dependencies and configurations
* Include security libraries/dependencies if applicable (eg. [Sqreen](https://www.sqreen.com/))
* Use [Anchore](https://anchore.com/) / [Clair](https://github.com/quay/clair) for container image static analysis
* Run [Nessus](https://www.tenable.com/products/nessus) and additional fuzzers on staging environment
* Sign Docker images ([DCT](https://docs.docker.com/engine/security/trust/content_trust/)) if untrusted registry
* Plug all security tooling reports into the SIEM
</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / DevOps security
</div>
<div class="flex-rectangle" markdown="1">
#### Tight RBAC

* Use least-privilege RBAC everywhere, eg:
  - AWS IAM for admins, services
  - Bucket policies, KMS policies etc
  - On the network level
</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / DevOps security
</div>
<div class="flex-rectangle" markdown="1">
#### Configuration management

* Use Git
* Use Terraform to deploy AWS resources
* Use Ansible and Packer to apply configurations to machines and prepare AMIs
* Use a CI (eg. Gitlab CI) to build docker images
* Use AWS Config to check what’s deployed against what’s architected
* Use AWS Nuke to destroy resources in the dev sandbox and reduce cost / exposure

</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / DevOps security
</div>
<div class="flex-rectangle" markdown="1">
#### Secrets management

* Mozilla Sops to encrypt secrets in git files with KMS or GPG
  - Set RBAC with IAM and KMS
  - Use Terraform Sops provider to automatically decrypt secrets on TF apply
* Ansible Vault to encrypt secrets in git files
  - Ansible automatically unvaults its secrets on playbook run
* Generic/service accounts are necessary
  - For admins, use central secret management solution (eg. bitwarden, dashlane)
  - For machines, use [Vault](https://www.vaultproject.io/)
  - Use automatic secret rotation when possible
* People share secrets in instant messages
  - Don’t use Slack’s free tier, prefer a self-hosted [Mattermost](https://mattermost.com/) instance
</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / DevOps security
</div>
<div class="flex-rectangle" markdown="1">
#### Bastion
* Set up a bastion host for all access to production and staging environments
  - No resource (EC2, RDS etc) can be SSH’d into without going through the bastion
* Log all admin actions to the SIEM
* Automatically blocks suspicious actions
</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / Security watch
</div>
<div class="flex-rectangle" markdown="1">
#### OSINT
* Continuously scan the open web for misuse of ACME information and identity
* Detect “shadow” IT assets
* Detect potential scammers, phishers using ACME-looking traps
* [Spiderfoot](https://www.spiderfoot.net/) is a useful example

#### Security newsfeeds
* Information about the latest attacks and vulnerabilities
  - Useful for keeping on top of [CVEs](https://cve.mitre.org/cve/) and updates
  - Useful for keeping up-to-date with changing threat landscape
</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / Security watch
</div>
<div class="flex-rectangle" markdown="1">
#### Threat Intelligence

* Use open TI feeds to supply information to in in-house TI platform
  - Open source platforms ([MISP](https://www.misp-project.org/), [OpenCTI](https://www.opencti.io/en/))
  - Get and share IoCs from public feeds ([Abuse.ch](https://urlhaus.abuse.ch/), [USA Homeland Security](https://www.dhs.gov/cisa/automated-indicator-sharing-ais) etc.)
  - Plug with the SIEM to detect intrusions quicker
</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Tech / Intrusion Prevention Systems
</div>
<div class="flex-rectangle" markdown="1">
#### Going one step further

* Include [OSSEC](https://www.ossec.net/) (an IPS) on all EC2s and run an OSSEC master
  - Useful for file integrity checking, alerting, and automated response
* Include [netsniff-ng](http://netsniff-ng.org/) on all EC2, and route the traffic through an encrypted connection to a Suricata (NIDS) server
* HIDS for docker containers
  - Monitor the host with OSSEC or [Falco](https://falco.org/) (doesn’t work with [Fargate](https://aws.amazon.com/fargate/))
  - Can run in a side car docker (eg. Falco)
* Must route back to the SIEM
</div>

&nbsp;

<div class="title-rectangle" markdown="1">
### Final thoughts
</div>
<div class="flex-rectangle" markdown="1">
* IT Security is the discipline of understanding your weak points and shielding them
* These tools, if implemented, need to be used for the security stance to actually improve
* It’s necessarily a company wide effort, requiring awareness and implication from
  - Management
  - Engineering
  - Business operations
</div>
