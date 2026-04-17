# 🏁 picoCTF Writeup: Old_Sessions

## 📌 Challenge Overview

Author: David Gaviria

Description
Proper session timeout controls are critical for securing user accounts. If a user logs in on a public or shared computer but doesn’t explicitly log out (instead simply closing the browser tab), and session expiration dates are misconfigured, the session may remain active indefinitely.
This then allows an attacker using the same browser later to access the user’s account without needing credentials, exploiting the fact that sessions never expire and remain authenticated.
Additional details will be available after launching your challenge instance.

💡 **Hint:** *Where are cookies stored?*

## 🔎 Step 1: Register with test username and password 

A comment reveals i found something at /sessions

## 🔎 Step 2: Vivit that /sessions page 

Reveals admin session value and also the test session value

## 🔎 Step 3: Replace the current session value in the Web Inspector with the admin session value and reload

🚩 Final Flag
picoCTF{s3t_s3ss10n_3xp1rat10n5_53a328ed}
