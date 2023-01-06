---
title: awsenv
date: 2022-02-14
categories:
- snippet
tags:
- aws
- azure
- fzf
---

In order to quickly log into and switch between AWS accounts in a terminal, I wrote the following script. It sets the `AWS_PROFILE` environment variable which is used by many tools that interact with the AWS API, like `awscli` or `terraform`. My current environment uses AzureAD as a single-sign-on provider, thus this script uses `aws sso login` to perform an MFA login into AWS. The AWS profiles must be set up in such a way that `aws configure list-profiles` can detect them, e.g. specify them in `${AWS_CONFIG_FILE:-$HOME/.aws/config}`. 

```shell
#!/usr/bin/env sh

###############################################################################
# This script performs an AWS SSO login for the user-selected AWS profile
# and sets the AWS_PROFILE environment variable afterwards. In order to use
# this, create an alias that sources this script like this:
#
#     alias awsenv='source path/to/this/script.sh'
#
# Required software that is not in GNU coreutils:
#   - 'aws' to list profiles & get current caller identity
#   - 'fzf' to list all available AWS profiles
###############################################################################

# prompt user to select one AWS profile
profile=$(aws configure list-profiles | \
  fzf --cycle --layout=reverse --tiebreak=index)

# user can cancel switching profiles by pressing ESC
if [ -n "${profile}" ]; then
  # check is access token exists and is valid for selected profile
  if ! aws --profile "${profile}" sts get-caller-identity >/dev/null 2>&1; then
    # perform login into profile in case access token is invalid
    if ! aws sso login --profile "${profile}"; then
      # short circuit in case login failed
      return
    fi
  fi
  # AWS_PROFILE is used by many AWS-related tools
  echo "Setting AWS_PROFILE to [${profile}]"
  export AWS_PROFILE="${profile}"
  # do not expose internal variables
  unset profile
fi
```
