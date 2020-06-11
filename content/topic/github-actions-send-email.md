---
title: Send emails with GitHub Actions
date: 2020-11-02
menu: topic
categories:
- devops
- snippet
tags:
- github
- github actions
- email
---

The [dawidd6/action-send-mail](https://github.com/dawidd6/action-send-mail) action allows to send an email in your [GitHub Action](https://github.com/features/actions).

```yaml
name: <PIPELINE>
jobs:
  build:
    runs-on: <RUN_ON>
    steps:
      - name: Send mail
        uses: dawidd6/action-send-mail@v2
        with:
          server_address: ${{ secrets.MAIL_SERVER }}
          server_port: ${{ secrets.MAIL_PORT }}
          username: ${{ secrets.MAIL_USERNAME }}
          password: ${{ secrets.MAIL_PASSWORD }}
          subject: <SUBJECT>
          body: <BODY>
          to: ${{ secrets.MAIL_RECIPIENT }}
          from: ${{ secrets.MAIL_SENDER }}
```

- `<PIPELINE>`: The name of your pipeline.
- `<RUN_ON>`: The runner to use, see GitHub's own [documentation](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on) for possible values.
- `<SUBJECT>`: Subject for the email.
- `<BODY>`: Body for the email.

Create appropriate secrets in your organization or project. In case you are using an organization, but different mailing lists, define `MAIL_RECIPIENT` for each project.
