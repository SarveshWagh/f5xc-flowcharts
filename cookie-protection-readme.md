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
