# Brute Force / Credential Stuffing

## 1. Summary
- **What it is:** Repeated authentification attempts against one or more accounts to guess passwords or test leaked credentials.
- **Common indicators:** Many failed logins, high-frequency attempts, attempts across many users from one IP, unusual geolocation, repeated attempts against privelged accounts.
- **Primary risk (CIA):** Confidentiality (account takeover), potentially Integrity (unauthorized changes).

## 2. Severity Guidance
- **Low:** Signle user, low volume, known IP, normal location, short duration.
- **Medium:** Multiple users or high-frequency attempts; unusual IP/geolocations; repeated attempts over time.
- **High/Critical:** Privelged accounts targeted; signs of successful login; MFA bypass attempts; activity across multiple systems.

## 3. Initial Triage Questions (5 minutes)
- Which accounts are targeted (standard vs admin)?
- Is there any successful login among the failures?
- Is MFA enabled on targeted accounts?
- Is the Source IP known/expected? (VPN, corporate IP, employee ISP)
- Are multiple services affected? (email + VPN + SSO)

## 4. Validation Steps (Evidence-Based)
- **Logs to check:**
  - Auth logs (Linux: `/var/log/auth.log` or `/var/log/secure`)
  - SSO/IdP logs (Azure AD/Okta/etc if available)
  - VPN access logs
  - SIEM correlated authentication alerts
- **Confirms true positive:**
  - Many failures in short time window (automation-like)
  - Attempts across many usernames from same IP
  - Geolocation inconsistent with user profile
  - Subsequent successful login followed by unusual activity
- **Suggests false positive:**
  - One user, low volume, long gaps between attempts
  - Known corporate IP / known device
  - Password reset attempts by user

## 5. Scope & Impact
Identify and document:
- Total failed attempts and time window
- Targeted accounts list
- Source IP(s), ISP/ASN/org if known
- Any successful authentication and what followed (new device, unusual location, impossible travel)

## 6. Containment Actions (If Authorized)
- **Short-term options:**
  - Force password reset for targeted account(s)
  - Enable/require MFA
  - Temporarily block source IP (if not shared corporate/VPN)
  - Disable account if confirmed compromise
- **Do-not-do (risk of disruption/evidence loss):**
  - Don’t block broad IP ranges without validation (risk: business disruption)
  - Don’t reset passwords without documenting evidence first (audit trail matters)

## 7. Escalation Triggers
Escalate to SOC lead / IR lead if:
- Any successful login is observed after repeated failures
- Privileged accounts are targeted
- Multiple users/systems are affected
- Evidence suggests credential stuffing (many usernames) or automation at scale

## 8. Documentation Requirements
Include in the ticket:
- Alert source and rule name (if SIEM)
- Timeline: first seen / last seen
- Affected accounts and count
- Source IP(s) + geolocation/ASN if available
- Evidence snippet (log lines or SIEM event IDs)
- Actions taken and approvals (if blocking/resetting)

## 9. Post-Incident
- Detection improvements: thresholds, geo-velocity rules, impossible travel
- Preventative controls: MFA enforcement, conditional access, lockout policies
