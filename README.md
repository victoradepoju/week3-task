# **Task: User Authentication & Organisation (Backend)**

## Overview

Using Spring (boot) framework, implement user authentication and organisation management based on the specifications below.

---

## **Acceptance Criteria**

### **Database**

* Connect your application to a PostgreSQL database.
* You may use an ORM of your choice (optional).

---

## **User Model**

The user model must include the following fields:

```json
{
  "userId": "string",       // must be unique
  "firstName": "string",    // must not be null
  "lastName": "string",     // must not be null
  "email": "string",        // must be unique and must not be null
  "password": "string",     // must not be null
  "phone": "string"
}
```

* Provide validation for all fields.
* On validation error, return status code **422** with this payload:

```json
{
  "errors": [
    {
      "field": "string",
      "message": "string"
    }
  ]
}
```

---

## **Authentication**

### **User Registration**

* Implement endpoint for registering users.
* Hash user passwords before storing them.
* On success, return **201 Created** with:

```json
{
  "status": "success",
  "message": "Registration successful",
  "data": {
    "accessToken": "eyJh...",
    "user": {
      "userId": "string",
      "firstName": "string",
      "lastName": "string",
      "email": "string",
      "phone": "string"
    }
  }
}
```

* On failure:

```json
{
  "status": "Bad request",
  "message": "Registration unsuccessful",
  "statusCode": 400
}
```

### **User Login**

* Implement login endpoint using email and password.
* On success, return **200 OK**:

```json
{
  "status": "success",
  "message": "Login successful",
  "data": {
    "accessToken": "eyJh...",
    "user": {
      "userId": "string",
      "firstName": "string",
      "lastName": "string",
      "email": "string",
      "phone": "string"
    }
  }
}
```

* On failure:

```json
{
  "status": "Bad request",
  "message": "Authentication failed",
  "statusCode": 401
}
```

---

## **Organisation Model**

* A user can belong to one or more organisations.
* An organisation can contain multiple users.
* On registration, create a default organisation.

  * Name format: `"<firstName>'s Organisation"`

### **Organisation Fields**

```json
{
  "orgId": "string",         // unique
  "name": "string",          // required, not null
  "description": "string"
}
```

---

## **Endpoints**

### **\[POST] /auth/register**

Registers a user and creates a default organisation.

**Request Body:**

```json
{
  "firstName": "string",
  "lastName": "string",
  "email": "string",
  "password": "string",
  "phone": "string"
}
```

**Response:** See user registration success/failure above.

---

### **\[POST] /auth/login**

Authenticates a user.

**Request Body:**

```json
{
  "email": "string",
  "password": "string"
}
```

**Response:** See login success/failure above.

---

### **\[GET] /api/users/\:id**       [PROTECTED]  

Get user details. Accessible only to logged-in users and limited to their organisations.

**Success Response:**

```json
{
  "status": "success",
  "message": "<message>",
  "data": {
    "userId": "string",
    "firstName": "string",
    "lastName": "string",
    "email": "string",
    "phone": "string"
  }
}
```

---

### **\[GET] /api/organisations**      [PROTECTED]

Get all organisations a user belongs to.

**Success Response:**

```json
{
  "status": "success",
  "message": "<message>",
  "data": {
    "organisations": [
      {
        "orgId": "string",
        "name": "string",
        "description": "string"
      }
    ]
  }
}
```

---

### **\[GET] /api/organisations/\:orgId**        [PROTECTED]

Get a specific organisation record (must belong to the user).

**Success Response:**

```json
{
  "status": "success",
  "message": "<message>",
  "data": {
    "orgId": "string",
    "name": "string",
    "description": "string"
  }
}
```

---

### **\[POST] /api/organisations**    [PROTECTED]

Create a new organisation.

**Request Body:**

```json
{
  "name": "string",
  "description": "string"
}
```

**Success Response:**

```json
{
  "status": "success",
  "message": "Organisation created successfully",
  "data": {
    "orgId": "string",
    "name": "string",
    "description": "string"
  }
}
```

**Failure Response:**

```json
{
  "status": "Bad Request",
  "message": "Client error",
  "statusCode": 400
}
```

---

### **\[POST] /api/organisations/\:orgId/users**        [PROTECTED]

Add user to an organisation.

**Request Body:**

```json
{
  "userId": "string"
}
```

**Success Response:**

```json
{
  "status": "success",
  "message": "User added to organisation successfully"
}
```

---

## **Unit Testing Requirements**

* **Token Generation**

  * Ensure correct expiry.
  * Verify user details in token.

* **Organisation Access**

  * Ensure users cannot access organisations they don’t belong to.

---

## **End-to-End Test: Register Endpoint**

Create the file: `tests/auth.spec.ext` (e.g., `tests/auth.spec.ts` for TypeScript)

### **Test Cases**

* ✅ Registers user successfully and creates default organisation.
* ✅ Validates the default organisation name (`"John's Organisation"`).
* ✅ Confirms expected user details and token in response.
* ❌ Fails registration when required fields are missing.
* ❌ Fails when duplicate email or userId is used.

---

## **Submission Instructions**

* Host your API (e.g., on Render, Vercel, Railway, etc.).
* Submit only the **base URL** of your endpoint, e.g., `https://example.com`
* Use the provided Google form for submission.

### **Deadline**

**Friday, 13th June 2025 at 11:59 PM GMT**
*Late submissions will not be accepted.*
