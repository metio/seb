---
title: Calendar sync privacy policy
date: 2026-11-15
url: /calendar-sync/privacy/
---

[Calendar sync](/calendar-sync/) accesses the calendars and events of my own Google account and keeps a copy of them on my own computers. No data is shared with anyone, no other person's account is ever accessed, and no data leaves my computers except to Google Calendar itself.

This policy is effective from 15 November 2026.

## Operator

Calendar sync is operated by Sebastian Hoß for personal use. Questions about this policy go to [seb@xn--ho-hia.de](mailto:seb@xn--ho-hia.de).

## Data accessed

When I sign in with my Google account, the tool requests the scope `https://www.googleapis.com/auth/calendar` and, through the Google Calendar API, accesses:

- the list of calendars in the account, with their names, colours and time zones;
- the events in those calendars, with their titles, descriptions, locations, start and end times, recurrence rules, reminders, attendees and the organizer's and attendees' names and email addresses, as far as an event contains them;
- the OAuth access and refresh tokens that Google issues for the sign-in.

The tool accesses no other Google data, such as contacts, mail, files or profile information beyond what a calendar event itself contains.

## Use of the data

The data is used only to provide the tool's single function, a two-way synchronization of my calendars:

- events are copied from Google Calendar into a local calendar store, where my calendar programs display them and show reminders;
- events I create, change or delete locally are written back to Google Calendar;
- the OAuth tokens authorize these requests to the Google Calendar API.

The data is not used for advertising, profiling, analytics, or to train or improve any machine-learning or artificial-intelligence model, and no artificial intelligence processes it.

## Storage, retention and security

The calendar data is stored as iCalendar files in a local calendar store on my own computers, readable only by my user account. The OAuth tokens are stored in an encrypted secret store on the same computers. The tool runs no server, and nothing is sent to any server other than Google's.

The local copy is kept for as long as I use the tool and mirrors my Google Calendar: an event deleted in Google Calendar is deleted from the local store at the next synchronization. Deleting the local store removes every copy the tool made, and revoking access, as described below, invalidates the tokens.

## Sharing

Calendar data is never sold, shared with, disclosed to or transferred to any third party. Nobody other than me reads it.

## Limited Use

The use and transfer of information received from Google APIs to any other app adheres to the [Google API Services User Data Policy](https://developers.google.com/terms/api-services-user-data-policy), including the Limited Use requirements. In particular, data obtained through Google APIs is:

- used only to provide and improve the user-facing synchronization described above;
- not transferred to others except as needed to provide that function, to comply with applicable law, or as part of a merger, acquisition or sale of assets after notice to the user;
- not used to serve advertisements, including retargeting, personalized or interest-based advertising;
- not read by humans, except with my own consent, which as the only user I give, or where needed for security purposes or to comply with applicable law.

## Revocation and deletion

Access can be revoked at any time under [Third-party apps & services](https://myaccount.google.com/connections) of the Google account, which stops all further access. Deleting the local calendar store and the stored tokens removes every copy of the data the tool holds.

## Children

The tool is used by a single adult and is not directed at children.

## Changes

Changes to this policy are published on this page, together with a new effective date.
