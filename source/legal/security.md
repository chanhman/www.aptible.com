---
title: Security Policy
tracked_title: Security
description: "Aptible's public security policy"
posted: 2017-08-14
section: Legal
sub_section: Policies
---
<!-- Reference Links -->
[Aptible Terms of Service]:/legal/terms-of-service
["Amazon Web Services: Overview of Security Processes - May 2017"]:https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf

Version 3.7 - August 2017

This policy outlines: 1) Aptible's security practices and resources, and 2)  your security obligations. 

Obligations under this policy (both ours and yours) are are incorporated by reference into the [Aptible Terms of Service].

### Your Obligations
Our documentation may specify restrictions on how the Services may be  configured, or specifications for Enclave Container Services such as apps. You agree to comply with any such restrictions or specifications.

You are responsible for properly configuring and using the Services and taking your own steps to maintain appropriate security, protection and backup of Your Content, which may include the use of encryption technology to protect Your Content from unauthorized access and routinely archiving Your Content. Aptible provides many built-in controls for you, as discussed herein. Where configurable or optional security controls (such as encryption) are offered as part of the Services, you are responsible for configuring or enabling those controls. You are ultimately responsible for determining whether the security controls applied to your Applications and data are sufficient for your requirements.

Aptible access credentials and private keys generated by the Services are for your internal use only. You may not sell, transfer or sublicense them to any other entity or person, except that you may disclose your private key to your agents and subcontractors performing work on your behalf.

Pursuant to Section 2 of the Aptible Terms of Service, you will not use the Services to create, receive, maintain, or transmit electronic protected health information, as defined in 45 C.F.R. § 160.103, without a Business Associate Agreement in place between you and Aptible.

### Reporting Security Vulnerabilities
If you discover a potential security vulnerability, please see our policy on [Responsible Disclosure](http://www.aptible.com/legal/responsible-disclosure). We strongly prefer that you notify us in private. Publicly disclosing a security vulnerability without informing us first puts the community at risk. When you notify us of a potential problem, we will work with you to make sure we understand the scope and cause of the issue. Thank you!

### Our Obligations
Without limiting any provision of the Aptible Terms of Service, we will implement reasonable and appropriate measures designed to help you secure Your Content against accidental or unlawful loss, access or disclosure.

### Our Security Practices
Aptible manages information security using the ISO/IEC 27001:2013 framework, which specifies the requirements for establishing, implementing, maintaining and continually improving a comprehensive information security management system and risk management capabilities.

#### **1. Data Center Security**
Aptible runs on the Amazon Web Services global infrastructure platform.

AWS publishes an ["Overview of Security Processes" whitepaper](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf) that serves as the reference material for this section. SOC 2 reports are available directly from AWS upon request.

##### **1.A - Compliance**
> AWS computing environments are continuously audited, with certifications from accreditation bodies across geographies and verticals, including ISO 27001, FedRAMP, DoD CSM, and PCI DSS. Additionally AWS also has assurance programs that provide templates and control mappings to help customers establish the compliance of their environments running on AWS against 20+ standards, including the HIPAA, CESG (UK), and Singapore Multi-tier Cloud Security (MTCS) standards.

p. 6 - ["Introduction to AWS Security - July 2015"](https://d0.awsstatic.com/whitepapers/Security/Intro_to_AWS_Security.pdf)

##### **1.B - Physical Security**
> AWS data centers are housed in nondescript facilities. Physical access is strictly controlled both at the perimeter and at building ingress points by professional security staff utilizing video surveillance, intrusion detection systems, and other electronic means. Authorized staff must pass two-factor authentication a minimum of two times to access data center floors. All visitors and contractors are required to present identification and are signed in and continually escorted by authorized staff.

p. 5 - ["Amazon Web Services: Overview of Security Processes - May 2017"]

##### **1.C - Environmental Security**
AWS data center environmental controls include:
- Fire detection and suppression systems
- Redundant power systems, backed by Uninterruptible Power Supply units and generators
- Climate and temperature controls
- Active system monitoring

pp. 5-8 - ["Amazon Web Services: Overview of Security Processes - May 2017"]

#### **2. Enclave Network Security**
Please see our [Reference Architecture Diagram](/resources/enclave-reference-architecture-and-division-of-responsibilities) and [FAQ](/faq) for an explanation of the terms in this section.

##### **2.A - Secure Architecture**
Aptible Enclave stacks run in separate AWS Virtual Private Clouds. Each stack is an isolated network. Most services run in a private subnet. Only SSL/TLS endpoints and a bastion host are exposed to the Internet. Backend users connect to the stack through the bastion host, which restricts access to stack components and logs activity for review.

##### **2.B - Firewalls**
All public-facing EC2 instances use inbound Security Group rules configured in deny-all mode. Ports are opened as necessary for: administrative SSH access, Enclave SSH Portal Access, and Redis. Public-facing Enclave Endpoints (which consist in part of an AWS load balancer) are configured to allow traffic on all ports, but only listen on the specific ports required for functionality (e.g., 80 and 443 for an HTTPS Endpoint).

##### **2.C - DDoS Protection and Mitigation**
Enclave's VPC-based approach means that most stack components are not accessible from the Internet, and cannot be targeted directly by a DDoS attack.

Enclave SSL/TLS endpoints include an AWS Elastic Load Balancer, which only supports valid TCP requests, meaning DDoS attacks such as UDP and SYN floods will not reach your app layer.

Should you need to add capacity to deal with a potential attack, you can instantly scale your stack using the Aptible dashboard or command line tool.

##### **2.D - Port Scanning**
AWS monitors and stops unauthorized port scanning. Because most of an Enclave stack is private, and all hosts run strict firewalls, port scanning is generally ineffective.

##### **2.E - Spoofing & Sniffing**
The AWS network prohibits a host from sending traffic with a source IP or MAC address other than its own. The AWS hypervisor will also not deliver any traffic to a host the traffic is not addressed to, meaning even an instance running in promiscuous mode will not receive or be able to "sniff" traffic intended for other hosts.

p. 13 - ["Amazon Web Services: Overview of Security Processes - May 2017"]

##### **2.F - Network and Host Vulnerability Scanning**
Aptible scans both the Internet-facing network and private network of a master reference stack each month. Aptible is responsible for network and host security, and remediates adverse findings without customer intervention, however you may request a scan of your dedicated VPC and its hosts as needed for your own security assessments and audits.

#### **3. Enclave Platform Security**

##### **3.A - Configuration and Change Management**
For app services that have an SSL/TLS endpoint attached, Enclave performs a health check on the container set before promoting it to the current release. If the health check fails, the container set is not promoted. Either way, the deploy is zero-downtime.

For any deploy, you can roll back to a previous codebase by pushing a different ref to your app's Git endpoint.

##### **3.B - Isolation**
Dedicated Enclave environments are deployed on AWS VPC-based dedicated stacks, isolated at the customer level. The VPC, network, underlying instances, and AWS virtual infrastructure for your dedicated stack are not shared with any other tenant.

##### **3.C - Logging and Monitoring**
Aptible logs AWS and Aptible API activity, and host activity within your stack. Enclave monitors performance indicators such as disk, memory, compute, and logging issues, and automatically resolves them on your behalf.

##### **3.D - Intrusion Detection & Prevention**
You may choose to run a host-based intrusion detection or prevention system that can be managed externally, such as Threat Stack, as an add-on. Aptible will ensure the host agents run and can connect to your external management service. You are responsible for procuring a license and operating the system.

##### **3.E - Host Hardening**
Enclave host operating systems are hardened based on the Center for Internet Security's Security Configuration Benchmark for the OS and version in use. For all operating systems:

- Operating systems are installed on hosts only from bare images, and only via automated configuration management. Services installed can be enumerated upon request.
- Host password logins are disabled. SSH root keys are not permitted.
- No user SSH keys are permitted on hosts by default. Aptible internal workforce user access is configured only on a per-user basis, and only when necessary to provide customer support.
- Swap is disabled to avoid writing in-memory secrets to unencrypted volumes.
- Command history for shell sessions is disabled.
- Non-default SSH ports are used.
- No password-based services are installed automatically. Password-based services (such as PostgreSQL) are provisioned only with unique, per-resource, Aptible-generated passphrases. No default passwords are permitted.
- Host security updates are automated.
- All host ports are opened only via whitelist.

##### **3.F - Your Code**
SSH public key authentication is used to limit access to your authorized backend users during git-based deploys. Following a successful push to an Aptible git endpoint, code is copied down to your stack's build layer. The resulting images are pushed to a private stack registry, backed by AWS S3, which provides redundant, access-controlled storage.

##### **3.G - Databases**
Databases run in the database layer of your stack, on a private subnet accessible only from app or bastion layer. SSL/TLS is required if the database protocol supports it. Disk volumes backing databases are encrypted at the filesystem level using Aptible-managed AES encryption. You can check whether your database uses AES-192 or AES-256 in the Enclave dashboard. You can rekey the database by dumping/restoring it at any time. You may implement additional controls, such as database security policies or row-/column-level encryption with keys you manage.

#### **4. Enclave Business Continuity**
##### **4.A - Backups**
Aptible automatically backs up several different types of data:

- Customer Enclave app code and the container images built from that code are stored in private, redundant, access-controlled registries. Aptible recommends that you maintain the canonical version of your codebase in a distributed version control system, such as GitHub. In the event of an app-level outage, Enclave automatically restores services from registry backups.

- Customer metadata is stored in the Aptible APIs, backed by the Amazon Relational Database Service. This metadata includes customer account data (passwords, permissions, SSH keys), and Enclave configuration data, such as environmental variables. Backups are taken nightly and retained for one week.

- Enclave customer database disks are automatically backed up nightly and retained daily for 90 days, and monthly for 6 years. No customer action is required. Two backup copies are kept: One in the region where the database runs, to facilitate fast disaster recovery; One in a separate geographic region to protect against loss of the original region. Customers may also take on-demand backups.

- For Enclave databases like PostgreSQL that support intermediate backups (e.g., write-ahead logs), Aptible configures these intermediate backups to span at least the time between daily backups, to enable fine-grained, point-in-time disaster recovery.

##### **4.B - Fault Tolerance**
AWS data centers are clustered into regions, and sub-clustered into availability zones, each of which is designed as an independent failure zone, meaning they are:

- Physically separated
- Located in lower-risk flood plains
- Equipped with independent uninterruptable power supplies and onsite backup generators
- Fed via different grids from independent utilities, and
- Redundantly connected to multiple tier-1 transit providers

For dedicated environments, Enclave automatically distributes app containers across availability zones when a service is scaled to more than one container.

##### **4.C - High Availability**
Enclave allows you to set up high-availability clustering for databases that support it.

App services on v2 stacks are automatically distributed across AWS availability zones as soon as they are scaled to more than one container.

##### **4.D - Disaster Prevention and Recovery**
Aptible monitors the stability and availability of customer infrastructure and automatically recovers from disruptions, including app and database failures. In the event of a disaster, Aptible restores apps from the last healthy build image and restores data from the last backup. In the event of a database outage, Enclave will automatically recover the underlying database instance and disk. If the disk is unavailable, Enclave will restore from a backup. Raw database snapshots and restored database clones are available upon request for testing and recovery.

##### **5 - Aptible Internal Security**

##### **5.A - Aptible Access**
We do not access or use Your Content for any purpose other than for developing and operating the Services and as required by law. As a routine matter, Aptible workforce members do not require access to data processed by your Enclave Containerized Services, such as data stored in your databases. Aptible workforce members are granted least-privilege access to customer environments only when a specific business need arises. Workforce members undergo criminal background screening before hire. In some cases, such as Enclave databases, you may encrypt Your Content using keys you manage.

##### **5.B - Security Management**
Aptible manages information security consistent with ISO 27001 and applicable legal and regulatory requirements.

Aptible conducts regular security and vulnerability assessments of stack hosts and our applications. Code undergoes automated testing and manual code review prior to being deployed to production. Our security team receives regular notifications of vulnerabilities and patches on a continuous basis.

We also run a [responsible disclosure program](https://www.aptible.com/legal/responsible-disclosure) for security vulnerabilities, with cash bounties.
