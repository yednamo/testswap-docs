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

The app dashboard shows every tester: days 1–14, check-in source (SDK or manual) and feedback.
In SDK mode it also shows whether Google Play Integrity confirmed the installation, and for the app, the time of the
last successful verification and the reason of the last rejection.

- **Feedback**: accept it, or reject it with an explanation. Without a decision within 3 days, it is accepted automatically.
  A tester can rewrite rejected feedback once. If you reject the rewritten feedback too, a TestSwap moderator reviews it:
  the test counts, and the moderator can revoke it and refund your points if the feedback really is empty or copied.
- **Remove from test**: e.g. if a tester did not install the app. Possible only until day 3 of the test inclusive.
  The reason is saved as a report.
- **Pause**: no new testers join, current tests continue.

If you delete your Yednamo account, your apps are closed and the running tests end without awarding points;
already completed tests and awarded points are kept.

## 5. Google’s questionnaire

The “Draft answers for Google” page turns your test statistics and feedback into draft answers
(in the interface language or in English). Review them and make sure to add what you changed after the feedback.
