---
title: awsenv
date: 2022-02-14
menu: topic
categories:
- snippet
tags:
- aws
- azure
- fzf
---

In order to quickly log into and switch between AWS accounts in a terminal, I wrote the following script:

```shell
#!/usr/bin/env sh

###############################################################################
# This script performs an aws-azure-login for the user-selected AWS profile
# and sets the AWS_PROFILE environment variable afterwards. In order to use
# this, create an alias that sources this script like this:
#
#     alias awsenv='source path/to/this/script.sh'
#
# Required software that is not in GNU coreutils:
#   - 'aws' to list profiles & get current caller identity
#   - 'aws-azure-login' to login via AzureAD
#   - 'fzf' to list all available AWS profiles
###############################################################################

# prompt user to select one AWS profile
selected_profile=$(aws configure list-profiles | fzf --cycle --layout=reverse --tiebreak=index)

# user can cancel switching profiles by pressing ESC
if [ -n "${selected_profile}" ]; then
    # check is access token exists and is valid for selected profile
    if ! aws --profile "${selected_profile}" sts get-caller-identity >/dev/null 2>&1; then
        # perform login into profile in case access token is invalid
        if ! aws-azure-login --no-prompt --profile "${selected_profile}"; then
          # short circuit in case login failed
          return
        fi
    fi
    # AWS_PROFILE is used by many AWS-related tools
    echo "Setting AWS_PROFILE to [${selected_profile}]"
    export AWS_PROFILE="${selected_profile}"
    # do not expose internal variables
    unset selected_profile
fi
```
