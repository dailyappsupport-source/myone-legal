# MYONE Privacy Policy

**Effective date:** 16 May 2026
**Last updated:** 16 May 2026

This Privacy Policy describes how the MYONE Android application ("MYONE", the "App", "we", "us", "our") collects, uses, stores, shares, and protects your information when you install or use the App.

MYONE is published by **DailyApp Labs** ("the Publisher"). The Publisher is the data controller for personal data processed under this policy.

If you do not agree with this Policy, please do not install or use MYONE.

---

## 1. Summary

- MYONE is a personal finance app. Most of your financial data (transactions, accounts, balances, budgets, goals, receipts) is stored **on your device** in an encrypted-at-rest local database managed by Android.
- If you sign in, your personal data is also synced to **Google Cloud Firestore** under your Firebase user ID so you can use it on other devices.
- If you opt in, MYONE can back up your data to **your own Google Drive** in an app-specific folder. We never read or write any other files in your Drive.
- We do **not** run advertising, tracking, or analytics SDKs. We do **not** sell or rent your personal data.
- You can delete your data at any time from inside the App or by contacting us.

---

## 2. Information we collect

### 2.1 Information you provide directly

When you use the App you create financial records yourself. These records may include:

- Transactions (date, amount, type, category, account, optional note, optional receipt photo).
- Accounts you set up (name, icon, type such as cash, debt, asset, deposit, mutual fund, stock, crypto), opening balance, currency, and optional metadata such as billing date, interest rate, tenor, broker or bank name.
- Budgets, savings goals, recurring transactions, installments, debt plans, scenarios, and notification reminders.
- Holdings or trade entries (ticker, quantity, purchase price, fees, dates) for investment accounts.
- Shared wallet membership and transactions you choose to record inside a shared wallet.
- Settings such as preferred currency, theme, language, reminders, and onboarding state.
- Optional 4 to 6 digit app PIN. The PIN is **never stored in plain text**. We store only a SHA-256 hash of the PIN, on your device.

You decide what to enter. You do not need a real name to use the App.

### 2.2 Information from your Google Account (only if you sign in)

If you sign in with Google through Android Credential Manager or Google Sign-In, we receive a limited profile from Google:

- Your Google account email address.
- Your Google account display name and profile picture, if available.
- A unique Firebase user ID (UID) that identifies your account inside MYONE.

### 2.3 Information from your device

- **App PIN hash and PIN-enabled flag** (stored locally only).
- **Firebase Cloud Messaging (FCM) registration token**, used to deliver push notifications to your device. The token is stored under your user record and deleted when you sign out or uninstall.
- **Notification permission state** (Android 13+).
- **Camera output** if you choose to attach a receipt photo to a transaction. The photo is stored on your device and only synced to your private Firestore folder if you have enabled cloud sync.
- **Locale and basic device locale settings** used to format currency and dates. We do not log advertising IDs, IMEI, MAC, or any other persistent device identifier.

### 2.4 Information from third-party services

- **CoinGecko**: when crypto pricing is enabled, the App fetches public market prices from CoinGecko. These requests do not include your account, name, email, holdings, or any personal identifier.
- **Google Play Billing**: if you purchase a subscription or one-off product, Google handles the payment. The App receives a purchase token used only to verify your entitlement. We never see your full credit card or banking details.
- **MYONE backend** (`api.myone.dailyapp.com`): used by the App for product configuration and entitlement checks. Requests include your Firebase UID and the relevant purchase token; they do not include the contents of your transactions.

### 2.5 Information we do NOT collect

We do not collect or use:

- Advertising identifiers (`ADID`, `IDFA`).
- Precise device location.
- Contacts, SMS, call logs, calendar.
- Health or fitness data.
- Biometric data (fingerprint or face). Biometric checks, if added later, are performed by the Android system on the device and we never receive the biometric value.
- Crash analytics, product analytics, or tracking SDKs of any kind. The current build of MYONE has no analytics, advertising, attribution, or remote logging libraries integrated.

---

## 3. Permissions we request

| Android permission | Why MYONE asks for it |
|---|---|
| `INTERNET` | Cloud sync, push notifications, price feed, entitlement checks. |
| `CAMERA` | Take photos of receipts for transactions. Photos are only saved when you tap save. |
| `POST_NOTIFICATIONS` | Show local reminders (bills, savings) and Firebase push notifications. |
| `RECEIVE_BOOT_COMPLETED` | Re-schedule reminders after the device restarts. |
| `READ_EXTERNAL_STORAGE` (Android 12 and below) | Pick existing receipt images from your gallery. Not requested on Android 13+. |
| `WRITE_EXTERNAL_STORAGE` (Android 9 and below) | Legacy export of receipts on older devices. Not requested on Android 10+. |
| `com.android.vending.BILLING` | Process subscription purchases through Google Play Billing. |

You can revoke any permission at any time in Android Settings; some features may stop working.

---

## 4. How your data flows

MYONE follows a "local first, sync optional" model.

### 4.1 On your device

- Financial data lives in a Room SQLite database called `myone.db` inside the App's private storage. Other apps cannot read this file.
- Receipt images are stored under the App's private files directory.
- Settings (theme, currency, PIN flag, PIN hash, etc.) are stored in Android DataStore.
- Background workers (WorkManager) run reminders, recurring transactions, and sync.

Android Auto Backup is enabled. The App's database, receipts, and most preferences are eligible to be backed up to your own Google Drive **by Android itself** (not by MYONE) when Auto Backup is enabled by the operating system. Sensitive keys are excluded from Auto Backup, including the PIN hash and any subscription entitlement token. The exact rules live in `app/src/main/res/xml/backup_rules.xml` and `data_extraction_rules.xml`.

### 4.2 Cloud sync (optional)

If you sign in, your **personal** data is mirrored under `users/{your_uid}` in Cloud Firestore. Only an authenticated user can read or write their own document tree. Firestore rules are enforced server-side and reviewed in `firestore.rules`.

If you create or join a **shared wallet**, the shared transactions and member list live under `sharedWallets/{walletId}`. Each member can read and contribute to that wallet. Invite codes are created and accepted through Firebase Cloud Functions; clients cannot write directly to invite documents.

### 4.3 Google Drive backup (optional)

If you connect Google Drive from Settings, MYONE asks for the **`drive.file`** scope. This scope only lets MYONE see and manage files that MYONE itself creates in Drive. We **cannot** see your other Drive files, photos, documents, or folders. We can:

- Create a backup file containing your MYONE database and receipts.
- Restore from a backup file you created in MYONE.
- Delete a backup file you ask us to remove.

You can revoke the Drive permission at any time at <https://myaccount.google.com/permissions>. Doing so does not delete data already in your Drive; you can delete those files manually from Drive.

### 4.4 Push notifications

If you allow notifications, your FCM token is stored under your user record so we can send reminders and shared-wallet activity alerts. The token can change when you reinstall the App or clear data. We can send notifications to that token; we cannot read messages or notifications from other apps.

### 4.5 Subscriptions

If you purchase MYONE Pro, the purchase is processed by **Google Play Billing**. The App receives a purchase token that we forward to our backend (or, in some flows, to a server-side verifier) to confirm the purchase is valid and unlock Pro features. We retain the entitlement state, the purchase token, the SKU, and the expiry timestamp. We never receive your card number, CVV, billing address, or banking credentials.

### 4.6 Public market data

If you enable crypto pricing, the App requests prices from CoinGecko. Requests contain no personal data.

---

## 5. Service providers and third parties

We do not sell or rent your personal data. We share data only with the providers necessary to run MYONE:

| Provider | Role | What is shared | Where to learn more |
|---|---|---|---|
| Google Firebase (Authentication, Firestore, Cloud Messaging, Cloud Functions) | Identity, cloud sync, push, server logic | Email, UID, FCM token, your synced records | <https://firebase.google.com/support/privacy> |
| Google Drive | Optional user-controlled backups | Backup file you generate | <https://policies.google.com/privacy> |
| Google Play Billing | Subscription processing | Purchase token, SKU | <https://policies.google.com/privacy> |
| Google Identity Services (Credential Manager, Google Sign-In) | Sign-in | OAuth tokens | <https://policies.google.com/privacy> |
| CoinGecko | Crypto market prices | Anonymous price queries only | <https://www.coingecko.com/en/privacy> |
| Cloud hosting for `api.myone.dailyapp.com` | Entitlement verification, configuration | UID, purchase token | Firebase Hosting |

These providers process data on our behalf under their own security and privacy commitments.

---

## 6. Data location and retention

- On-device data lives on your device until you delete it from inside MYONE or uninstall the App. Uninstalling normally removes the local database.
- Cloud Firestore data is hosted by Google. The default region for the MYONE Firebase project is `asia-southeast2`.
- Cloud Functions execute in `asia-southeast2`.
- Drive backup files are stored in **your** Google account, in the region Google assigns to your account.
- We retain Firestore data for as long as your account exists. When you delete your account or specific records, the corresponding Firestore documents are deleted within 30 days.
- Operational logs (Cloud Functions logs, billing verification logs) are retained for up to 90 days for security and debugging, then rotated.
- Backups created by Android Auto Backup follow Google's own retention rules.

---

## 7. Security

- All network traffic between the App and Firebase, our backend, and CoinGecko uses HTTPS/TLS.
- Firebase Auth tokens are short-lived and rotated by the Google Identity SDK.
- The optional app PIN is hashed with SHA-256 before being saved on the device.
- Firestore security rules restrict each user to their own `users/{uid}` tree and to shared wallets they belong to.
- Shared-wallet invite acceptance runs only inside trusted Cloud Functions, not directly from the client.
- Receipt images and the local database live in the App's private storage, which Android isolates from other apps.

No system is perfectly secure. If you believe your account or data has been compromised, contact us at the address in section 12.

---

## 8. Your choices and rights

Depending on where you live, you may have rights under laws such as the EU/UK GDPR, the California Consumer Privacy Act, and Indonesia's UU PDP. These include:

- **Access** the personal data we hold about you.
- **Correct** inaccurate data, mostly by editing it in the App.
- **Delete** your data, by:
  - Removing individual records inside the App, or
  - Uninstalling the App to remove on-device data, or
  - Emailing the contact in section 12 with the email tied to your account so we can delete cloud data tied to that account.
- **Export** your data from inside the App, where export features are available.
- **Object** or **restrict** certain processing.
- **Withdraw consent** for cloud sync, Drive backup, push notifications, or subscriptions at any time. The on-device app will keep working in offline mode.
- **File a complaint** with your local data protection authority.

We will respond to verifiable requests within 30 days. We may need to verify the request comes from the account owner.

---

## 9. Children

MYONE is **not directed to children under 13** (or under the equivalent age in your jurisdiction). We do not knowingly collect personal data from children. If you believe a child has provided us personal data, contact us and we will delete it.

---

## 10. International data transfers

If you are outside the country where MYONE is operated, your data may be transferred to and processed in countries with different data protection rules. Where we use Google services, transfers rely on Google's standard contractual clauses and other safeguards described in their privacy policy.

---

## 11. Changes to this policy

We may update this Policy as MYONE evolves. When we make material changes we will:

- Update the **Last updated** date at the top.
- Show a notice inside the App or on the listing page before the change takes effect.

Continued use of MYONE after a change means you accept the updated Policy.

---

## 12. Contact

For privacy questions, requests, or complaints:

- Email: dailyapp.support@gmail.com
- Postal address: Available upon verified request by email.
- Governing law: Laws of the Republic of Indonesia.

We aim to respond within 7 business days.

---

## Appendix A — Google Play Data safety mapping

This appendix mirrors the answers you should provide on the Play Console **App content → Data safety** form. It is informational, not part of the legal policy.

### Data collected

| Category | Data type | Collected? | Optional? | Shared with third parties? | Processed in transit only? | Used for |
|---|---|---|---|---|---|---|
| Personal info | Email address | Yes (if user signs in) | Yes (you can use the App offline) | No | No | Account management, sync |
| Personal info | Name | Yes (display name from Google profile, if provided) | Yes | No | No | Account display |
| Personal info | User IDs (Firebase UID) | Yes (if user signs in) | Yes | No | No | Account management |
| Financial info | Other financial info (transactions, balances, debts, goals) | Yes (if user signs in) | Yes | No | No | App functionality |
| Financial info | Purchase history | Yes (if user subscribes) | Yes | No | No | Entitlement |
| Photos and videos | Photos | Yes (only if user attaches receipts) | Yes | No | No | App functionality |
| Files and docs | Files and docs | Yes (only if user enables Drive backup) | Yes | No | No | App functionality |
| App activity | App interactions | Yes (only data the user creates: categories, accounts, etc.) | Yes | No | No | App functionality |
| Messages | None | No | – | – | – | – |
| Audio | None | No | – | – | – | – |
| Calendar | None | No | – | – | – | – |
| Contacts | None | No | – | – | – | – |
| Location | None | No | – | – | – | – |
| Web browsing | None | No | – | – | – | – |
| Device or other IDs | FCM token | Yes (if notifications enabled) | Yes | No | No | App functionality (notifications) |

### Security practices

- Data is encrypted in transit (TLS).
- Users can request deletion of their data.
- App follows Google Play Families Policy (if applicable to your final audience selection).

### Disclosures

- The App does **not** include any third-party advertising SDKs.
- The App does **not** include any third-party analytics SDKs.
- The App does process subscription purchases through Google Play Billing.

---

## Appendix B — Sources of truth in the codebase

For maintainers, the assertions in this Policy reflect the following code paths. Update both this Policy and the corresponding files together when behavior changes.

- Permissions: `app/src/main/AndroidManifest.xml`
- Backup and data extraction rules: `app/src/main/res/xml/backup_rules.xml`, `data_extraction_rules.xml`
- Drive scope: `core-auth/DriveBackupManager.kt`, `core-auth/GoogleAuthManager.kt` (uses `DriveScopes.DRIVE_FILE`)
- PIN hashing: `core-data/storage/delegates/SettingsStorageDelegator.kt` (SHA-256)
- Personal sync path: `users/{uid}` (`README.md`, `firestore.rules`)
- Shared-wallet invite path: Cloud Functions in `functions/`
- Crypto price source: CoinGecko (`BuildConfig.COINGECKO_CRYPTO_ENABLED`)
- Backend host: `BuildConfig.API_BASE_URL = https://api.myone.dailyapp.com`
- Push notifications: `MyoneFirebaseMessagingService` registered in `AndroidManifest.xml`
