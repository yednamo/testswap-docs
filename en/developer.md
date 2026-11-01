# For developers

## Why

Google Play requires new personal developer accounts to run a closed test with at least 12 testers for 14 days
in a row. After that, Play Console shows a questionnaire about the test and you can apply for production access.

## 1. Prepare the closed test in Play Console

1. Upload a build to the **Closed testing** track (Testing → Closed testing).
2. Choose how testers get access:
   - **Google Group (recommended)**. Create a group at https://groups.google.com that anyone can join.
     In Play Console: Closed testing → Testers → add the group address (`name@googlegroups.com`).
     Testers join the group themselves.
   - **Email list**. Create a tester list in Play Console. When someone joins on TestSwap, you get an email and
     see the address in your dashboard: add it to the list and press “Access granted”.
     If access is not granted within 7 days, the participation is closed automatically with no penalty for either side,
     and the tester can join another app.
3. Publish the release to the closed test and wait for Google’s review.
4. **If the app has Google sign-in** (Credential Manager, Sign in with Google, Firebase Auth), check the signature.
   Google Play re-signs the build with its app signing key, whose SHA-1 differs from your upload key. If only
   the upload key is registered in Google Cloud, testers see the account picker but the sign-in never completes:
   to them “the app is broken”, and they drop the test.
   - Take the SHA-1 of the **app signing key**: Play Console → “Protected with Play” → “App signing”.
   - Add it in Google Cloud → Google Auth Platform → Clients as a separate Android client with the same package
     (or in Firebase → Project settings → SHA fingerprints). Keep the upload key for builds you install directly.
     No re-release is needed; the change takes from 5 minutes to a few hours.
   - Make sure the OAuth consent screen is “In production”: in “Testing” mode only listed test users can sign in.

## 2. List the app on TestSwap

[List an app](https://testswap.yednamo.com/en/dev/new):

- name, package (`applicationId`, cannot be changed later), short description;
- **what to test**: specific scenarios, otherwise feedback will be generic;
- access mode (Google Group or email list);
- participation check: **with SDK** or **without SDK** (see [SDK](sdk.md)). SDK mode requires Android 8.0+
  and Play Integrity keys: without them the app does not appear in the catalog;
- number of testers: 12–30, 15 recommended as a buffer.

You can have up to 3 open apps at a time.

## 3. Points

One tester costs **12 points**: 10 are charged when they complete the test, 2 when you accept their feedback.
Your balance can go down to **−180** (15 testers), so you can list your first app right away and pay back by testing others.
While the reserve for current testers does not fit the limit, new testers cannot join.

## 4. During the test

The app dashboard starts with three numbers: joined on the site, joined the group (as reported by testers; next to it
a link to the group’s member list in Google Groups, visible only to the group owner), opened the app (in SDK mode
confirmed by Play Integrity, without SDK the first check-in). Below, every tester: their current step and when they
first opened the app, days 1–14, check-in source (SDK or manual) and feedback. For the app, the time of the last
successful Integrity verification and the reason of the last rejection.

A tester who has not opened the app within 3 days is removed automatically (with a warning email on day 2),
and the spot frees up for someone else. You can turn this off in the app settings (“Free the spot
automatically…”): the site then only shows “not opened yet”, and you remove testers manually, at any time.
Useful when testers use the app but have not linked it to the site: Google counts them anyway.

- **Feedback**: accept it, or reject it with an explanation. Without a decision within 3 days, it is accepted automatically.
  A tester can rewrite rejected feedback once. If you reject the rewritten feedback too, a TestSwap moderator reviews it:
  the test counts, and the moderator can revoke it and refund your points if the feedback really is empty or copied.
- **Messages**: a private chat with each tester. The “Messages” page collects conversations across all your apps,
  unread first, each with the app, the tester’s step and test day. The list shows only conversations that already
  have a message; start a conversation with a tester from the app dashboard, where every tester has a
  “Messages” button. Messages are delivered instantly; if the recipient has not read one within 10 minutes, they get
  an email (at most once an hour per conversation). You can write to a tester while their participation exists; a
  tester can write while the test is running and for 7 more days after. Up to 2000 characters, at most 30 messages
  per hour. The “Report” button sends the conversation to a moderator.
- **Remove from test**: e.g. if a tester did not install the app. A tester who has already checked in can be removed
  only until day 3 of the test inclusive; one who never opened the app, at any time. The reason is saved as a report.
- **Pause**: no new testers join, current tests continue.
- **Close app**: do this only after you have applied in Play Console. Until then testers are asked not to leave the
  Google group (Google only counts testers opted in for the last 14 days). After closing, everyone who tested gets
  an email saying they can leave the group.

If you delete your Yednamo account, your apps are closed and the running tests end without awarding points;
already completed tests and awarded points are kept.

A tester who left on their own, did not start within 3 days or did not get access within 7 days can join again once:
the test starts over for them, and the dashboard shows both participations. A tester you removed cannot return.

## 5. Showcase and public feedback

- Once the app is live on Google Play, press “Published on Google Play” in the “Showcase” panel of the app
  dashboard. The app appears on the [“Launched”](https://testswap.yednamo.com/en/launched) page and on the home
  page: name, category, description, your name with a profile link, how many testers completed the test, the date
  and a Google Play button. It is a free mention for you and social proof for the service. The mark can be removed.
- “Show accepted feedback on the app page” (in the app form or in the “Showcase” panel): the public app page then
  shows up to 10 latest accepted feedback entries with the tester’s name and date, and the showcase shows one quote
  from the newest. Off by default. Rejected and undecided feedback is never shown.

## 6. Google’s questionnaire

The “Draft answers for Google” page turns your test statistics and feedback into draft answers
(in the interface language or in English). Review them and make sure to add what you changed after the feedback.
