# Workhop preparation

This directory includes Cloudformation template for preparing the workshop

## Prerequisites

- Be IAM Administrator

## Modules

1. Deploy the "iam.template" cloudformation template file of this directory.

### Devops
if
```
git clone https://git-codecommit.eu-west-1.amazonaws.com/v1/repos/uni-api
Cloning into 'uni-api'...
fatal: repository 'https://git-codecommit.eu-west-1.amazonaws.com/v1/repos/uni-api/' not found
```
Verify `cat ~/.gitconfig`

if

```
git clone https://git-codecommit.eu-west-1.amazonaws.com/v1/repos/uni-api
Cloning into 'uni-api'...
fatal: unable to access 'https://git-codecommit.eu-west-1.amazonaws.com/v1/repos/uni-api/': The requested URL returned error: 403
```
Solution 

```
git config --global credential.helper '!aws --profile workshop1 codecommit credential-helper $@'
```
