# Privacy Policy

Effective Date: October 1, 2025

This Privacy Policy describes how we collect, use, and protect your personal data in compliance with the GDPR and other applicable laws. As an EU-based project available worldwide, we prioritize GDPR standards for all users. This Policy forms part of our Terms of Service.

## 1. Data Controller

Apostolos Zlatinis aka Remi is the data controller responsible for your personal data. Contact: remizlatinis@gmail.com.

## 2. Personal Data We Collect

We collect minimal personal data necessary for the Service:

- **From Google SSO (via Firebase):** Public profile name, email address, and profile picture (if available). This is used for authentication.
- **Device Identifiers:** ExpoPushTokens generated for push notifications (via Firebase).
- **From Agent Requests:** IP addresses from your Linux machine where the Agent is installed.
- **Service Logs:** Status logs of user-defined services reported by the Agent for monitoring purposes.

We do not collect data for analytics. The Agent may use BugSink for error logging in production, but this can be disabled by you.

No cookies are set for tracking or analytics in any component.

## 3. How We Use Your Data

Your data is processed based on legitimate interests (for Service functionality) or consent (where applicable, e.g., SSO):

- SSO data: Solely for authentication.
- ExpoPushTokens: Solely for sending push notifications about service status changes.
- IP addresses and service logs: To facilitate real-time monitoring, visible only to you (via Clients) and the Backend administrator for support. Not used elsewhere.

Data is not used for marketing, profiling, or automated decision-making.

## 4. Sharing Your Data

We share data only with Firebase (Google) for SSO and push notifications. No other third parties receive your data, except as required by law (e.g., legal requests). Data shared is limited and protected under data processing agreements compliant with GDPR.

## 5. Data Retention

Data is retained as long as your account is active. Upon account deletion, data is erased. Due to the project's early stage, database migrations may occur, potentially erasing data—but you can re-sign up, and the Agent on your machine can recreate monitoring data. We do not retain backups beyond what's necessary for Service continuity.

## 6. Security Measures

All communications between Clients, Backend, and Agent use HTTPS encryption. Authentication credentials are stored securely in Expo Secure Storage on mobile Clients. We implement reasonable measures to protect data, though no system is infallible.

## 7. Your Rights Under GDPR

As a data subject, you have rights including:

- Access: Request a copy of your data.
- Rectification: Correct inaccurate data.
- Erasure ("Right to be Forgotten"): Delete your data, subject to legal obligations.
- Restriction: Limit processing in certain cases.
- Portability: Export your data in a structured format.
- Objection: Object to processing based on legitimate interests.
- Withdraw Consent: Where processing relies on consent.

To exercise these rights, email remizlatinis@gmail.com. We will respond within one month (extendable under GDPR). If unsatisfied, you may complain to your local data protection authority (e.g., Hellenic Data Protection Authority in Greece).

## 8. International Transfers

If data is transferred outside the EU/EEA (e.g., via Firebase), we ensure adequate safeguards like Standard Contractual Clauses approved by the European Commission, in compliance with GDPR Chapter V.

## 9. Changes to This Privacy Policy

Changes will be notified via email to active users. The effective date is noted at the top.

For any privacy concerns, contact us as above.
