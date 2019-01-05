# AWS Hacking

This guide provides some basic instructions how to compromise AWS accounts. The hope is that by knowing how to take advantage of various types of AWS weaknesses you will be verse enough to provide the correct countermeasures.

> WARNING: There will be spelling errors and incorret grammer all over the place. It will get better over time. At this stage we are looking to build the content.

## Introduction

When you hear about AWS security vulnerabilities we often think of misconfigured S3 buckets. No wonder. For some uknown reason s3 buckets happen to be really hard to get right in the majority of organisations and every bucket ever created is a one click away from complete security disaster.

Although s3 buckets are the pinnacle of AWS security, there is so much more one can do. And as it happens, attackers do!

## Fingerprinting

### AWS Acount ID Fingerprinting

Information about related account ids that belongs to the target organisation is not directly available. There are however, various techniques that can be used to obtain such information. This information can be subsequently used to find AWS account ids.

#### Public Code Repositories

Public code repositories belonging to the target organisation could contain artifacts related to AWS accounts such as ARNs (Amazon Resource Names). [Gitleaks](https://github.com/zricethezav/gitleaks) is a tool which comes extremely useful in such situations. In fact, OpenDevSecops official [Gitleaks Docker Build](https://github.com/opendevsecops/docker-gitleaks) contains prebuilt configuration files that can be used to enumerate AWS resources. Here is an example:

```sh
docker run opendevsecops/gitleaks --config=/run/configs/aws-enum.toml --github-org=target --exclude-forks -v
```

> NOTE: Gitleaks is great but it is a memory hog. Sounds like something that could be automated with pownjs.

#### Docker Hub

It is worth checkout the target docker hub. If the image is not built with a public Dockerfile there is a chance it may container some interesting artifacts. You will be surprised how many time this actually happens.

There are no known tools to automate this process at present.

> NOTE: Sounds like something that can be done with pownjs.

#### NPM, Gem, Maven, Etc.

Companies who use AWS tend to have very diverse software stacks. It is not too uncommon to find references to AWS artifacts in package repositories (especially old ones).

There are no known tools to automate this process at present.

> NOTE: Sounds like something that can be done with pownjs or perhaps build custom solutions with the language that is supported by the package manager.

#### Mobile Apps

Mobile apps often contain more than they should like header files, readmes, configuration files and more. Some developers do not know how to slice (conditionally compile) their soure code such as it does not contain sensitive debug/testing information which often could lead to revealing some interesting aspects about the target including AWS account information and secrets.

#### Search Engines

Needless to say, the target organisation may already disclouse this information somewhere such as GitHub, Stackoverflow, etc. Some basic searches could produce fruitful results:

```
"arn:aws:" target
".amazonaws.com" target
```

#### Web Sites

Websites belonging to the target organisation may contain various types of leaks including AWS ARNs. There is no easy way to go about this. You need to download the site content and look for strings.

> NOTE: SecApps Cohesion might be able to help with this task.

> NOTE: SecApps Unfold might be able to help with this task as well.

## Methodology

The key to success is having a well defined system. While this rule applies to life as a whole it is particularly useful when performing security assessments. The following table can be used to kickstart any AWS security assessment project:

| Category           | Question                                    | Complience (Yes/No/Partial) |
|--------------------|---------------------------------------------|-----------------------------|
| **Fingerprinting** | Are AWS ARNs leaked on GitHub repositories? |                             |
|                    | Are AWS ARNs leaked on in search engines?   |                             |
|                    | Are AWS ARNs leaked on target sites?        |                             |

## Links

* [Using AWS Account ID’s for IAM User Enumeration](https://rhinosecuritylabs.com/aws/aws-iam-user-enumeration/)
* [Assume the Worst: Enumerating AWS Roles through AssumeRole](https://rhinosecuritylabs.com/aws/assume-worst-aws-assume-role-enumeration/)
* [Cloud Breach: Compromising AWS IAM Credentials](https://rhinosecuritylabs.com/aws/aws-iam-credentials-get-compromised/)

## Labs

* [Terraform AWS Goat](https://github.com/opendevsecops/terraform-aws-goat)

## Authors

* [@pdp](https://twitter.com/pdp)

## Sponsors

* [Websecurify](https://websecurify.com)
* [SecApps](https://secapps.com)
* [GNUCITIZEN](https://gnucitizen.org)
