# Microsoft Entra ID — External User Access Governance Lab (B2B + Access Review + MFA)

<img width="1536" height="1024" alt="ChatGPT Image Feb 21, 2026 at 11_09_24 AM" src="https://github.com/user-attachments/assets/b0d2190a-55b3-4224-9c38-9267750dcd16" />

## Overview

This lab focuses on implementing and validating secure external collaboration using Microsoft Entra External Identities.

The objective was to simulate a real-world partner access scenario — inviting an external user, enforcing authentication controls, validating access through MyApps, and performing governance through an Access Review.

The exercise demonstrates how organizations can safely onboard external identities while maintaining visibility, strong authentication, and lifecycle control.

---

## Scenario

A vendor/partner requires temporary access to organizational resources.

To model a realistic workflow, I implemented:

- A B2B guest invitation to an external **Gmail identity**
- Invitation redemption (acceptance) by the guest user
- A **Conditional Access policy** that enforces MFA for guest access
- A guest sign-in that triggered MFA and generated sign-in telemetry
- MyApps access validation to confirm what the guest can see and access
- An **Access Review** to simulate periodic access certification and lifecycle governance

---

## Environment

- Microsoft Entra ID tenant
- External Identities (B2B collaboration)
- Conditional Access
- MyApps portal
- Identity Governance — Access Reviews
- Test guest identity: **Gmail account**

---

## Implementation Summary

### 1) Guest Invitation (B2B)
An external user was invited using a Gmail address through Entra External Identities.  
The invitation was delivered to Gmail and redeemed by accepting the invite, creating a guest user object in the tenant.

![Image 2-21-26 at 10 31](https://github.com/user-attachments/assets/53ca4f79-ad93-4953-abfa-d391eaa33058)

![Image 2-21-26 at 10 35](https://github.com/user-attachments/assets/1955d711-ff4d-4776-b143-939e4077fcfb)

![Image 2-21-26 at 10 36](https://github.com/user-attachments/assets/68da6ab4-8a70-4225-839f-5ef1fd7fc103)

![Image 2-21-26 at 10 37](https://github.com/user-attachments/assets/08a7a5e5-64e3-45b8-b1ca-6213aa64a109)

---

### 2) Conditional Access Policy (MFA for Guests)

To ensure external users authenticate securely, I created a Conditional Access policy to require MFA for guest sign-ins.

**Policy intent:** enforce strong authentication for external identities without relying on manual enforcement.

**Key policy design:**
- **Users:** Guest or external users (B2B guests)
- **Target resources:** All cloud apps (or selected apps depending on scope)
- **Grant controls:** Require multifactor authentication
- **State:** Enabled

This policy is what intentionally triggered the MFA prompt during the guest sign-in test.

![Image 2-21-26 at 10 40](https://github.com/user-attachments/assets/db543bf2-dee1-4aec-9b76-4206dc1f5389)

![Image 2-21-26 at 10 42](https://github.com/user-attachments/assets/64786fb2-d484-4631-b099-98b6147d3cac)

---

### 3) Access Review (Identity Governance)
To simulate ongoing governance, I created an **Access Review** for the guest’s access to ensure external access is periodically revalidated.

The review supports:
- Access certification decisions (keep/remove)
- Preventing stale external access
- Ongoing least-privilege enforcement
![Image 2-21-26 at 10 45](https://github.com/user-attachments/assets/8d22edb9-8d54-4648-9bb2-38cdb806a5e9)

![Image 2-21-26 at 10 46](https://github.com/user-attachments/assets/cf0dc3c4-1bed-49ed-9e2b-195d7f00ed21)

![Image 2-21-26 at 10 47](https://github.com/user-attachments/assets/1355d8d0-c6ea-4638-8954-959b5e3b45a8)

--- 

### 3) External User Sign-In + MFA Validation (Gmail)
Using the invited Gmail identity, the guest completed sign-in and MFA.

During validation, I confirmed:
- The sign-in was recorded as a **Guest** authentication event
- MFA was successfully required and satisfied
- Conditional Access evaluation showed the policy being applied to the sign-in

This verified that external identities are governed by the organization’s authentication controls.

![Image 2-21-26 at 10 52](https://github.com/user-attachments/assets/303f2515-4b2c-4032-80c1-e1a88d1a0eab)

![Image 2-21-26 at 10 52](https://github.com/user-attachments/assets/a075e4a4-ed99-473f-acab-bde10a6f9e2a)

![Image 2-21-26 at 10 52 (1)](https://github.com/user-attachments/assets/810a032a-0d8a-450e-ac2d-e4dbb4592023)

![Image 2-21-26 at 10 54](https://github.com/user-attachments/assets/7aae7e20-62ad-4a60-b10f-d8dd2742c5cd)

![Image 2-21-26 at 10 54 (1)](https://github.com/user-attachments/assets/bf696b0b-2a33-4d35-a9f4-ed04f376ee87)

![Image 2-21-26 at 10 55](https://github.com/user-attachments/assets/3820d7db-ea0d-4849-9612-342d3474e732)

![Image 2-21-26 at 10 56](https://github.com/user-attachments/assets/d4bfc5dc-0382-498f-9e90-417aed2494e0)

---

### 4) MyApps Access Validation (Guest Experience + Reviewer Mindset)

After MFA, I signed into **MyApps** using the Gmail guest identity to validate the external user experience and confirm:

- What applications/resources the guest can see
- Whether access is scoped as intended
- That access is not broader than required

This mirrors how IAM teams validate real-world partner access from the user’s perspective.

![Image 2-21-26 at 11 00](https://github.com/user-attachments/assets/18209e4f-25a7-46d8-b011-0e620f48a26e)

![Image 2-21-26 at 11 01](https://github.com/user-attachments/assets/e891f719-d6d8-4316-89a3-25cf37812730)

---

## Logging and Evidence (What I Verified)

I reviewed Microsoft Entra logs to confirm:
- Guest invitation redemption/acceptance
- Conditional Access policy evaluation during the guest sign-in
- MFA requirement and completion
- Successful sign-in session creation
- Governance activity tied to the access review workflow

![Image 2-21-26 at 11 02](https://github.com/user-attachments/assets/9c64493d-5d61-4ddf-9c46-11a21e312e05)

![Image 2-21-26 at 11 14](https://github.com/user-attachments/assets/4f035406-652a-42b0-9116-2c4ecbe4bf1c)

![Image 2-21-26 at 11 15](https://github.com/user-attachments/assets/f719506c-c288-49f8-bcd3-4992381ac785)

