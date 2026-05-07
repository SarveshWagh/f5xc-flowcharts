# F5XC Security Feature Enablement Guidelines
## ⚠️ Important Note

Please refer to the official F5XC technical documentation for the latest and updated details.  
This README is intended for learning and understanding purposes only, these are juist guidelines to get started with and in no way best practices.

## CSRF Protection

### Step 1: Application Type Assessment

* If the application is an **API**:

  * End. No CSRF protection is required.
* If the application is a **Web Application**:

  * Move to Step 2.

---

### Step 2: Existing Application-Level Protection Check

* If CSRF protection is **already implemented at the application level using csrf tokens or referrer/origin validation**:

  * End. No additional CSRF protection is required on the F5XC platform.
* If CSRF protection is **not implemented**:

  * Move to Step 3.

---

### Step 3: F5XC Platform Action Item

Apply CSRF protection directly on the F5XC platform.

#### Prerequisite

Gather and configure the list of trusted domains/referrers that are legitimately allowed to send requests to the onboarded application.

Example:

* If `bank.com` accepts requests from `bank1.com`, then `bank1.com` must be added to the allowed source origin list.

---

# Cookie Protection

### Step 1: Cookie Type Assessment

* If the cookie is a **session/authentication cookie**:

  * Move to Step 2.
* If the cookie is a **non-session cookie** (analytics/tracking/etc.):

  * End. No cookie protection is required on the F5XC platform.

---

### Step 2: Existing Application-Level Protection Check

* If cookie protection is **already implemented at the application level**:

  * End. No additional cookie protection is required on the F5XC platform.
* If cookie protection is **not implemented**:

  * Move to Step 3.

---

### Step 3: F5XC Platform Action Item

Apply cookie protection directly on the F5XC platform.

#### Prerequisites

Gather the following details:

* Session cookie name(s)
* Cookie attributes currently used or required:

  * `HttpOnly`
  * `Secure`
  * `SameSite`

---

# API Discovery

### Step 1: Application Identification

Identify the API applications that require API Discovery.

---

### Step 2: Load Balancer Configuration

* Navigate to the HTTP Load Balancer (LB) configuration for the application.
* Go to the **API Protection** section.

---

### Step 3: Enable API Discovery

Enable the **API Discovery** setting.

---

### Step 4: Define Learning Parameters

#### Response Filter

Choose whether API Discovery should learn from:

* `200 OK` responses only
* Redirect responses as well

#### Retention Policy

Define how long the platform should retain discovered API endpoint information.

---

# API Protection / Validation

### Step 1: Schema Acquisition

Request the latest OpenAPI/Swagger specification files from the application development team.

---

### Step 2: Platform Integration

* Upload the Swagger/OpenAPI files to the F5XC platform.
* Create an **API Definition** object using the uploaded schema files.

---

### Step 3: Logical Grouping

Create **API Groups** to apply different validation and security policies to different API endpoint sets.

---

### Step 4: Load Balancer Enforcement

Under:

```text
HTTP Load Balancer → API Protection
```

Perform the following:

* Select the API Definition object
* Set Validation Mode to:

  * `Custom List`

---

### Step 5: Validation Rule Configuration

Create validation rules for each API group.

Validation can be applied for:

* Requests
* Responses
* Both

Typical validation checks include:

* Parameters
* Data types
* Schema compliance

---

### Step 6: Policy Staging (Recommended Safe Transition)

#### Initial State

Set policy action to:

```text
Report
```

---

#### Analysis Phase

Perform false positive (FP) analysis using logs and observed traffic behavior.

---

#### Final State

Once traffic is confirmed legitimate, change the action to:

```text
Block
```

---

### Step 7: Fall-Through Logic

Configure a **Fall-Through Rule List** for traffic that does not match any explicit API group.

#### Recommended Approach

* Start with:

  * `Report`
* Then move to:

  * `Skip`
  * or `Block`

based on observed traffic behavior and validation confidence.

# Cookies, Cookie Protection and CSRF - Notes

# Cookies

Cookies are sent by backend applications to the user's browser, for example session cookies used for login/authentication.

Each domain maintains its own cookie storage ("cookie jar" for understanding purposes).

Example:

* `bank.com` has its own cookie jar
* `evil.com` has its own cookie jar

When the user makes a request to `bank.com`, the browser automatically sends cookies associated with `bank.com`.

When the user makes a request to `evil.com`, the browser automatically sends cookies associated with `evil.com`.

Cookie behavior depends on the cookie protection settings configured on the cookies.

---

# Cookie Protection - SameSite Attribute

Cookie protection settings commonly use the `SameSite` attribute.

The three possible values are:

* `Strict`
* `Lax`
* `None`

---

# Request Categories

Cookie behavior can be understood by dividing requests into two categories:

## 1. Background Triggered Requests (Silent Requests)

The user does not intentionally navigate to the site.

Examples:

* `<img>`
* `<iframe>`
* `<script>`
* Auto-submitted `<form>`
* `fetch()` / AJAX

---

## 2. User-Initiated Navigation

The user intentionally navigates to the site.

Examples:

* Clicking a link
* Typing a URL
* Bookmark click
* Redirects after login

---

# SameSite Behavior Matrix

## User-Initiated Navigation

| SameSite | Cookie Behavior                                       |
| -------- | ----------------------------------------------------- |
| Strict   | Cookies not sent                                      |
| Lax      | Cookies sent (typically for top-level GET navigation) |
| None     | Cookies sent                                          |

---

## Background Triggered Requests

| SameSite | Cookie Behavior  |
| -------- | ---------------- |
| Strict   | Cookies not sent |
| Lax      | Cookies not sent |
| None     | Cookies sent     |

---

# Simplified SameSite View

| SameSite | Silent Attack (`img/form`) | User Click       |
| -------- | -------------------------- | ---------------- |
| Strict   | Cookies not sent           | Cookies not sent |
| Lax      | Cookies not sent           | Cookies sent     |
| None     | Cookies sent               | Cookies sent     |

---

# CSRF - Cross Site Request Forgery

## What is CSRF?

Imagine a user has already authenticated into `bank.com`.

The server sends session cookies, which are stored in the browser under the `bank.com` domain.

Now the user visits another site, for example `evil.com`.

If `evil.com` contains a malicious element like an `<img>` or `<a>` tag that triggers a request to `bank.com` (for example, a money transfer), this is considered a CSRF attack.

---

# Why is CSRF Possible?

Even though the request originated from `evil.com`, the browser automatically attaches cookies belonging to `bank.com` when making the request.

The backend server sees valid session cookies and assumes the request is legitimate.

## Key Insight

> Cookies prove identity, but not user intent.

---

# CSRF Protection Mechanisms

CSRF protection can be implemented in multiple ways.

---

# 1. CSRF Tokens (Primary Defense)

The application generates a unique CSRF token and includes it in the page (HTML, JavaScript, or meta tags).

When a legitimate request is made, the browser sends:

* Session cookie (automatically)
* CSRF token (explicitly included by the application)

The server validates both.

If the token is missing or invalid:

* The request is rejected.

---

# Why the Attacker Fails

An attacker on `evil.com`:

* Can trigger a request
* Cannot read `bank.com` content
* Cannot access the CSRF token

Therefore:

* The attacker cannot send a valid token
* The request is rejected

---

# 2. SameSite Cookies

The `SameSite` attribute controls when cookies are sent in cross-site requests.

| SameSite | Behavior                                                                |
| -------- | ----------------------------------------------------------------------- |
| Strict   | Cookies never sent cross-site                                           |
| Lax      | Cookies sent only on user-initiated navigation (typically GET requests) |
| None     | Cookies sent in all requests                                            |

`SameSite=Lax` helps reduce CSRF risk by blocking cookies during silent/background requests.

---

# 3. Origin / Referer Validation

The server (or F5XC platform) checks:

* Origin header
* Referer header
* Destination/target host

If the source origin/referrer and target host match (or belong to the allowed list):

* Request is allowed

If they do not match:

* Request is rejected

---

# F5XC CSRF Protection Note

CSRF protection on F5XC is optimized for properly designed applications where state-changing operations use:

* `POST`
* `PUT`
* `DELETE`

F5XC primarily protects actions, not page views.

Applications should avoid using `GET` requests for sensitive state-changing operations.
