# AWS Hacking

This guide provides some basic instructions how to compromise AWS accounts.

## Fingerprinting

### AWS Acount ID Fingerprinting

Information about related account ids that belongs to the target organisation is not directly available. There are however, various techniques that can be used to obtain such information. This information can be subsequently used to find AWS account ids.

#### Public Code Repositories

Public code repositories belonging to the target organisation could contain artifacts related to AWS accounts such as ARNs (Amazon Resource Names). [Gitleaks](https://github.com/zricethezav/gitleaks) is a tool which comes extremely useful in such situations. In fact, OpenDevSecops official [Gitleaks Docker Build](https://github.com/opendevsecops/docker-gitleaks) contains prebuilt configuration files that can be used to enumerate AWS resources. Here is an example:

```sh
docker run opendevsecops/gitleaks --config=/run/configs/aws-enum.toml --github-org=target --exclude-forks -v
```

#### Search Engines

Needless to say, the target organisation may already disclouse this information somewhere such as GitHub, Stackoverflow, etc. Some basic searches could produce fruitful results:

```
"arn:aws:" target
```

#### Web Sites

Websites belonging to the target organisation may contain various types of leaks including AWS ARNs. There is no easy way to go about this. You need to download the site content and look for strings.

```
NOTE: SecApps Cohesion might be able to help with this task.
NOTE: SecApps Unfold might be able to help with this task as well.
```

## Methodology

The key to success is having a well defined system to follow. While this rule applies to life as a whole it is particularly useful when performing security assessments. The following table can be used to kickstart any AWS security assessment project:

| Category       | Question                                    | Complience (Yes/No/Partial) |
|----------------|---------------------------------------------|-----------------------------|
| Fingerprinting | Are AWS ARNs leaked on GitHub repositories? |                             |
|                | Are AWS ARNs leaked on in search engines?   |                             |
|                | Are AWS ARNs leaked on target sites?        |                             |

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
