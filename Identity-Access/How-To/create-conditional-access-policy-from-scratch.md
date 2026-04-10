# How Do You Create a Conditional Access Policy From Scratch?

## Metadata

| Field | Value |
|---|---|
| Category | Identity & Access / How-To |
| Difficulty | Intermediate |
| Time to Complete | 15–30 minutes (policy creation) + testing |
| Last Verified | 2026-04-10 |
| Sources | *(Add your LinkedIn saves / company KB source IDs here)* |
| Prerequisites | [What Is Conditional Access and How Does It Work?](../Concepts/what-is-conditional-access-and-how-it-works.md) |

## The Question

> You need to create a new Conditional Access policy that requires MFA for all users
> when accessing any cloud app from outside the corporate network. How do you do it
> from scratch, including testing and rollout?

## Quick Answer (TL;DR)

1. Define the policy logic: WHO + WHAT app + WHAT condition → WHAT control
2. Create in **Report-Only** mode first (never enforce immediately)
3. Exclude break-glass accounts
4. Test with sign-in logs
5. Move to **On** after validation period

## Prerequisites Checklist

- [ ] At least **Entra ID P1** licenses assigned to target users
- [ ] **Break-glass accounts** exist and are documented
- [ ] Users have **MFA methods registered** (check Authentication Methods → Registration report)
- [ ] **Named Locations** configured (if using location-based conditions)
- [ ] You have **Security Administrator** or **Conditional Access Administrator** role

---

## Step-by-Step Procedure

### Phase 1: Plan the Policy Logic

Before touching the Entra portal, write out the policy in plain English:

```
IF:
  - User is: All users EXCEPT break-glass accounts and service accounts
  - Accessing: All cloud apps
  - From: Outside the corporate network (any untrusted location)
THEN:
  - Require: Multi-factor authentication
```

> [!IMPORTANT]
> **Always write the logic out before creating the policy.** This prevents misconfigurations, especially the accidental "block everyone from everything" scenario.

### Phase 2: Create the Policy in Report-Only Mode

| Step | Action | Setting |
|---|---|---|
| 2.1 | Navigate to **Entra Admin Centre** → **Protection** → **Conditional Access** → **Policies** | |
| 2.2 | Click **+ New Policy** | |
| 2.3 | **Name**: Use a clear naming convention, e.g., `CA-003: Require MFA from untrusted locations` | Descriptive, numbered |
| 2.4 | **Assignments → Users**: | |
|  | Include: **All users** | |
|  | Exclude: Break-glass accounts, service accounts group | ⚠️ CRITICAL: Never skip exclusions |
| 2.5 | **Assignments → Target resources**: | |
|  | Cloud apps: **All cloud apps** | |
| 2.6 | **Conditions → Locations**: | |
|  | Include: **Any location** | |
|  | Exclude: **Trusted locations** (your named corporate locations) | |
| 2.7 | **Grant controls**: | |
|  | Select: **Require multifactor authentication** | |
| 2.8 | **Session controls**: Leave default (or set sign-in frequency if required) | |
| 2.9 | **Enable policy**: Set to **Report-only** | ⚠️ NOT "On" — test first |
| 2.10 | Click **Create** | |

### Phase 3: Test in Report-Only Mode

| Step | Action | What to Check |
|---|---|---|
| 3.1 | Wait 24–48 hours for sign-in data to accumulate | |
| 3.2 | Go to **Sign-in logs** → filter by the policy name | |
| 3.3 | Review the **"Report-only"** column: | |
|  | ✅ **Success** = user would have passed with MFA | No issue |
|  | ❌ **Failure** = user would have been blocked | Investigate — do they have MFA registered? |
|  | ⚠️ **Not applied** = policy conditions didn't match | Check if the user/app/location conditions are correct |
| 3.4 | Check for false positives: legitimate users/apps that would be blocked | Adjust exclusions if needed |
| 3.5 | Check for false negatives: users who should be challenged but aren't | Verify condition logic |

### Phase 4: Enforce the Policy

| Step | Action |
|---|---|
| 4.1 | Once satisfied with Report-Only results, edit the policy |
| 4.2 | Change **Enable policy** from **Report-only** to **On** |
| 4.3 | Save |
| 4.4 | Monitor sign-in logs for the first 24 hours after enforcement |
| 4.5 | Have the service desk ready for MFA-related support calls |

---

## Verification

| Check | How to Verify | Expected Result |
|---|---|---|
| Policy is active | Entra → CA → Policies → check state = "On" | State shows "On" |
| MFA prompt outside office | Sign in from mobile data / VPN-off | MFA challenge appears |
| No MFA from office | Sign in from corporate network | No MFA challenge (location excluded) |
| Break-glass works | Sign in with break-glass account | No MFA challenge (excluded) |
| Sign-in logs show policy | Sign-in logs → Conditional Access tab | Policy listed as "Success" |

## What Can Go Wrong

| Problem | Cause | Fix |
|---|---|---|
| ALL users blocked from everything | Forgot to exclude break-glass accounts OR accidentally set grant to "Block" | Use break-glass to disable the policy immediately |
| MFA loops forever | User hasn't registered MFA methods | Check Auth Methods registration; enable self-service registration |
| Service accounts break | Service accounts not excluded | Add service accounts to exclusion group |
| Legacy apps stop working | Legacy apps can't do MFA (Basic auth) | Create separate policy to block legacy auth; migrate apps to modern auth |
| VPN users get challenged | VPN exit IP not in Named Locations | Add VPN egress IPs to trusted named locations |

## Related Questions

- [How do you set up and protect break-glass accounts?](./setup-break-glass-accounts.md)
- [How do you block legacy authentication?](./block-legacy-authentication.md)
- [How do you require a compliant device for Office 365?](./require-compliant-device-for-office365.md)
- [Troubleshoot: User unexpectedly blocked by CA](../Troubleshoot/user-blocked-by-ca-policy.md)
- [Concept: What is Conditional Access and how does it work?](../Concepts/what-is-conditional-access-and-how-it-works.md)

## Notes & Lessons Learned

<!-- Add your real-world notes here -->
<!-- Examples:
- "We had to add 15 service accounts to the exclusion group — maintain a dynamic 
   group or use a naming convention filter"
- "Report-Only for 2 weeks minimum before enforcement — we caught 3 legacy apps 
   that would have broken"
- "Always communicate to users BEFORE enforcing MFA — reduces service desk tickets by 80%"
-->
