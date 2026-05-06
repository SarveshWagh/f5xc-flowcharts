# F5XC Security Feature Enablement Guidelines

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
