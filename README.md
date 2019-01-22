[![Follow on Twitter](https://img.shields.io/twitter/follow/opendevsecops.svg?logo=twitter)](https://twitter.com/opendevsecops)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/8d095fd957fe404da20ab90ef0db8770)](https://www.codacy.com/app/OpenDevSecOps/guide-aws-hacking?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=opendevsecops/guide-aws-hacking&amp;utm_campaign=Badge_Grade)

# AWS Hacking

This guide provides some basic instructions how to compromise AWS. The hope is that by knowing how to take advantage of various types of AWS weaknesses you will be verse enough to provide the correct countermeasures.

> **WARNING**: There will be spelling errors and incorret grammer all over the place because we are not checking for such things at this point in time. It will get better. At this stage we are looking to build the content.

## Introduction

When you hear about AWS security vulnerabilities we often think of misconfigured S3 buckets. No wonder. For some uknown reason s3 buckets happen to be particularly tricky to get right in the majority of organisations and every bucket ever created is a one click away from complete security disaster.

Although s3 buckets are the pinnacle of AWS security, there is so much more one can do. And as it happens, attackers do!

The purpose of this project is to provide some guidelines of the various tips, tricks and techniques that take to compromise an AWS account.

## Fingerprinting

The more you know about the target organisation the better you can do when you try to find security vulnerabilities. This principle broadly applies to anything and AWS is no exception. Generally, we try to find AWS account ids, ARNs, IP addresses, Role Names and other related AWS information.

### Public Code Repositories

Public code repositories belonging to the target organisation could contain artifacts related to AWS accounts such as ARNs (Amazon Resource Names). [Gitleaks](https://github.com/zricethezav/gitleaks) is a tool which comes extremely useful in such situations. In fact, OpenDevSecops official [Gitleaks Docker Build](https://github.com/opendevsecops/docker-gitleaks) contains prebuilt configuration files that can be used to enumerate AWS resources. Here is an example:

```sh
docker run opendevsecops/gitleaks --config=/run/configs/aws-enum.toml --github-org=target --exclude-forks -v
```

> **NOTE**: Gitleaks is great but it is a memory hog. Sounds like something that could be automated with pownjs in the future.

You can use [recon](https://github.com/pownjs/pown-recon) from the [pown](https://pownjs.com/) toolkit to quickly find github account you would like to target. Here is an example how this could work:

```sh
pown recon t githublistmembers <target>
pown recon t githublistrepos <target|member>
```

Use the method above to dump the leaks once you identify all repositories.

Pay particular attention to personal dotfile repositories. It is very common to see such things discloused online.

### Docker Hub

It is worth checkout the target docker hub for target related images. If the image is not built with a public Dockerfile there is a chance it may container some interesting artifacts.

There are no known tools to automate this process at present.

> **NOTE**: Sounds like something that can be done with pownjs.
> **TODO**: There is a tool to discover each change in the image. What was the name?

### Package Manager (NPM, Gem, Maven, Etc.)

Companies who use AWS tend to have very diverse software stacks. It is not too uncommon to find references to AWS artifacts in package repositories (especially old ones).

There are no known tools to automate this process at present. You need to download each individual package version and check for useful information.

> NOTE: Sounds like something that can be done with pownjs or perhaps build custom solutions with the language that is supported by the package manager.

### Mobile Apps (iOS, Android)

Mobile apps often contain more than they should like header files, readmes, configuration files and more. Some developers do not know how to slice (conditionally compile) their soure code such as it does not contain sensitive debug/testing information which often could lead to revealing some interesting aspects about the target including AWS account information and secrets.

### Web Search Engines

Needless to say, the target organisation may already disclouse this information somewhere such as GitHub, Stackoverflow, etc. Some basic searches could produce fruitful results:

```
"arn:aws:" target
".amazonaws.com" target
```

Mix the above a little bit for more targeted results. For example, the following search query will try to find AWS-related information in trello boards:

```
"arn:aws:" target site:trello.com
```

It is also possible to discover other types of resources in AWS that could lead to fruitful results. For example, elastic search instances can be discovered with simple searches like this:

```

```

### Shodan, ZoomEye, Censys

These sites can be used to find potentially interesting information that relates to the target. Try all of them and see what you can get. Again, the aim to find things that are related, not just some random open targets on the internet, which plenty can be found with ease.

### Web Sites

Websites belonging to the target organisation may contain various types of leaks including AWS ARNs. There is no easy way to go about this. You need to download the site content and look for strings. You can look at tools such as `httrack` to do this for you. There is also a large collection of tools that can be used in this domain over [here](https://github.com/BruceDone/awesome-crawler).

> **NOTE**: SecApps Cohesion might be able to help with this task.

> **NOTE**: SecApps Unfold might be able to help with this task as well.

### AWS Bugs

From time to time you may encounter a bug which you can use to map emails to account ids and so on. Be on a constant lookout for these bugs. AWS appears tight in this regard but it like any other software is has its own challanges.

### General Rule

Basically the better job you do at anumerating the target the higher the chance of finding something that is useful. There is no single tool that will do this for you. You need to use what you know and be creative.

## Methodology

The key to success is having a well defined system. While this rule applies to life as a whole it is particularly useful when performing security assessments. The following table can be used to kickstart any AWS security assessment project:

| Category           | Question                                         | Complience (Yes/No/Partial) |
|--------------------|--------------------------------------------------|-----------------------------|
| **Fingerprinting** | Are AWS resources leaked on GitHub repositories? |                             |
|                    | Are AWS resources leaked on in search engines?   |                             |
|                    | Are AWS resources leaked on target sites?        |                             |

## Defaults

You will find AWS credentials in the following locations:

| OS          | Path                               |
|-------------|------------------------------------|
| Window      | C:\Users\USERNAME\.aws\credentials |
| Linux/Unix  | /home/USERNAME/.aws/credentials    |

Additionally the following URLs can be queried for sensitive information if you already have access to AWS (EC2).

| URL                                                                               | Description                             |
|-----------------------------------------------------------------------------------|-----------------------------------------|
| http://169.254.169.254/latest/meta-data/iam/security-credentials/                 | Name of the attached instance profile   |
| http://169.254.169.254/latest/meta-data/iam/security-credentials/the_profile_name | The actual credentials from the profile |
| http://169.254.169.254/latest/meta-data/iam/security-credentials/dummy            | Same as above but works with AWS Glue   |

## Tools

* [Bucket Search By Grayhatwarfare](https://buckets.grayhatwarfare.com)
* [Pacu AWS Exploitation Framework](https://github.com/RhinoSecurityLabs/pacu)

## Links

Rhino Security Labs has a tone of tutorials on how to hack into AWS.

* [Using AWS Account ID’s for IAM User Enumeration](https://rhinosecuritylabs.com/aws/aws-iam-user-enumeration/)
* [Assume the Worst: Enumerating AWS Roles through AssumeRole](https://rhinosecuritylabs.com/aws/assume-worst-aws-assume-role-enumeration/)
* [Cloud Breach: Compromising AWS IAM Credentials](https://rhinosecuritylabs.com/aws/aws-iam-credentials-get-compromised/)
* [AWS IAM Enumeration 2.0: Bypassing CloudTrail Logging](https://rhinosecuritylabs.com/aws/aws-iam-enumeration-2-0-bypassing-cloudtrail-logging/)
* [AWS Privilege Escalation – Methods and Mitigation
](https://rhinosecuritylabs.com/aws/aws-privilege-escalation-methods-mitigation/)
* [Cloud Security Risks (Part 1): Azure CSV Injection Vulnerability](https://rhinosecuritylabs.com/azure/cloud-security-risks-part-1-azure-csv-injection-vulnerability/)
* [Cloud Security Risks (P2): CSV Injection in AWS CloudTrail](https://rhinosecuritylabs.com/aws/cloud-security-csv-injection-aws-cloudtrail/)
* [Penetration Testing AWS Storage: Kicking the S3 Bucket
](https://rhinosecuritylabs.com/penetration-testing/penetration-testing-aws-storage/)

## Labs

* [Terraform AWS Goat](https://github.com/opendevsecops/terraform-aws-goat)
* [Cloud Goat](https://github.com/RhinoSecurityLabs/cloudgoat)

## Authors

* [@pdp](https://twitter.com/pdp)

## Sponsors

* [Websecurify](https://websecurify.com)
* [SecApps](https://secapps.com)
* [GNUCITIZEN](https://gnucitizen.org)
