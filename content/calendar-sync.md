---
title: Calendar sync
date: 2026-11-15
url: /calendar-sync/
---

Calendar sync is the Google OAuth client that lets my own computers synchronize my Google Calendar with a local calendar store. It is a personal setup with exactly one user, me, and it is not offered to anyone else.

The sync runs [neverest](https://github.com/pimalaya/neverest) on my computers, which reads and writes the calendars of my Google account through the Google Calendar API. [calendula](https://github.com/pimalaya/calendula) reads that local store to show events and save invitations, and [ortie](https://github.com/pimalaya/ortie) obtains the OAuth token.

How the client handles calendar data is described in its [privacy policy](/calendar-sync/privacy/).
