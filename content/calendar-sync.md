---
title: Calendar sync
date: 2026-11-15
url: /calendar-sync/
---

Calendar sync keeps a copy of my Google Calendar on my own computers, so that I can read and edit my appointments in calendar programs that run in a terminal and work offline. It is a personal tool operated by Sebastian Hoß, with exactly one user, me, and it is not offered to anyone else.

## Purpose

The tool synchronizes the calendars of my Google account with a local calendar store in both directions:

- Events created or changed in Google Calendar, for example on my phone, are copied to my computers.
- Events I create, change or delete on my computers, including meeting invitations I accept from my mail program, are written back to Google Calendar.
- Reminders for upcoming events are shown as desktop notifications on my computers.

## Google data used

The tool requests a single permission, the Google Calendar scope `https://www.googleapis.com/auth/calendar`. It uses it only to list the calendars of my account and to read, create, update and delete the events in them, which the two-way synchronization described above requires. It uses no other Google API and reads no other Google data.

## Software

The synchronization runs [neverest](https://github.com/pimalaya/neverest) on my computers, which talks to the Google Calendar API. [calendula](https://github.com/pimalaya/calendula) reads the local store to show events and save invitations, and [ortie](https://github.com/pimalaya/ortie) obtains the OAuth token when I sign in. All three are open-source programs that run locally; the tool has no server of its own.

## Scope of use

The tool only organizes my own appointments. It does not generate, edit or analyse images or any other media, involves no artificial intelligence of any kind, and does not use Google APIs for creating AI-generated non-consensual intimate imagery or any other generated content.

How the tool handles calendar data is described in its [privacy policy](/calendar-sync/privacy/).
