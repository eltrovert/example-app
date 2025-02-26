Below is a **simple and professional API documentation** for your Flask application. This documentation is designed to be clear and concise, making it easy for the mobile app team to integrate with your API.

---

# **API Documentation**

## **Base URL**
All API endpoints are relative to the following base URL:
```
https://mobile-api.kourai.com/api
```

---

## **Authentication**
- **Authorization Header**: All requests require a valid Firebase ID token in the `Authorization` header.
  - Format: `Bearer <Firebase-Token>`
  - Example:
    ```bash
    Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6ImRjNjI2MmYzZTk3NzIzOWMwMDUzY2ViODY0Yjc3NDBmZjMxZmNkY2MiLCJ0eXAiOiJKV1QifQ...
    ```

---

## **Endpoints**

### **1. Health Check**
- **Endpoint**: `/users/ping`
- **Method**: `GET`
- **Description**: A simple health check endpoint to verify the API is up and running.
- **Response**:
  ```json
  {
    "message": "pong"
  }
  ```

---

### **2. Create a User Profile**
- **Endpoint**: `/users`
- **Method**: `POST`
- **Description**: Creates a new user profile.
- **Headers**:
  - `Authorization`: Bearer `<Firebase-Token>`
  - `Content-Type`: `application/json`
- **Request Body**:
  ```json
  {
    "Identifier": "unique_identifier_456",
    "UserName": "Alice",
    "LastName": "Johnson",
    "DateOfBirth": "1985-08-15",
    "PhoneNumber": "5555555555",
    "EmailAddress": "alice@example.com",
    "EmailPassword": "supersecurepassword",
    "Address": "789 Oak St",
    "Country": "UK",
    "State": "London",
    "ZIP": "SW1A 1AA",
    "Provider": "Firebase"
  }
  ```
- **Response**:
  - Success (HTTP 201):
    ```json
    {
      "message": "User profile created successfully"
    }
    ```
  - Error (HTTP 400 or 409):
    ```json
    {
      "error": "Missing required fields",
      "missing_fields": ["FieldName1", "FieldName2"]
    }
    ```

---

### **3. Retrieve a User Profile**
- **Endpoint**: `/users/<user_uid>`
- **Method**: `GET`
- **Description**: Retrieves a user profile by `UserUID`. Optionally, you can specify which fields to retrieve using the `fields` query parameter.
- **Headers**:
  - `Authorization`: Bearer `<Firebase-Token>`
- **Query Parameters**:
  - `fields` (optional): Comma-separated list of fields to retrieve.
    - Example: `?fields=UserName,LastName,Email`
- **Response**:
  - Default Response (all fields):
    ```json
    {
      "UserUID": "<user_uid>",
      "Identifier": "unique_identifier_456",
      "UserName": "Alice",
      "LastName": "Johnson",
      "DateOfBirth": "1985-08-15",
      "PhoneNumber": "5555555555",
      "EmailAddress": "alice@example.com",
      "Address": "789 Oak St",
      "Country": "UK",
      "State": "London",
      "ZIP": "SW1A 1AA",
      "Provider": "Firebase",
      "created_at": "2023-10-01T12:34:56Z",
      "updated_at": "2023-10-01T12:34:56Z"
    }
    ```
  - Custom Fields Response:
    ```json
    {
      "UserName": "Alice",
      "LastName": "Johnson",
      "EmailAddress": "alice@example.com"
    }
    ```

---

### **4. Update a User Profile**
- **Endpoint**: `/users/<user_uid>`
- **Method**: `PUT`
- **Description**: Updates an existing user profile.
- **Headers**:
  - `Authorization`: Bearer `<Firebase-Token>`
  - `Content-Type`: `application/json`
- **Request Body**:
  - Only include the fields you want to update.
  ```json
  {
    "UserName": "Jane",
    "LastName": "Doe",
    "EmailPassword": "newsecurepassword"
  }
  ```
- **Response**:
  - Success (HTTP 200):
    ```json
    {
      "message": "User profile updated successfully"
    }
    ```
  - Error (HTTP 400 or 500):
    ```json
    {
      "error": "Unexpected error",
      "details": "Error details here"
    }
    ```

---

### **5. Delete a User Profile**
- **Endpoint**: `/users/<user_uid>`
- **Method**: `DELETE`
- **Description**: Deletes a user profile by `UserUID`.
- **Headers**:
  - `Authorization`: Bearer `<Firebase-Token>`
- **Response**:
  - Success (HTTP 200):
    ```json
    {
      "message": "User profile deleted successfully"
    }
    ```
  - Error (HTTP 404 or 500):
    ```json
    {
      "error": "User not found"
    }
    ```

---

## **Error Handling**
The API uses standard HTTP status codes and provides detailed error messages in JSON format.

| HTTP Status Code | Description                     | Example Response                                                                 |
|-------------------|---------------------------------|----------------------------------------------------------------------------------|
| 200 OK            | Successful request             | `{ "message": "User profile updated successfully" }`                             |
| 201 Created       | Resource created successfully  | `{ "message": "User profile created successfully" }`                             |
| 400 Bad Request   | Missing or invalid parameters  | `{ "error": "Missing required fields", "missing_fields": ["FieldName1"] }`       |
| 401 Unauthorized  | Invalid or missing token       | `{ "error": "Token is missing" }`                                                |
| 404 Not Found     | Resource not found             | `{ "error": "User not found" }`                                                  |
| 409 Conflict      | Duplicate entry                | `{ "error": "Identifier already exists", "details": "The provided Identifier..." }` |
| 500 Internal Server Error | Unexpected server error | `{ "error": "Unexpected error", "details": "Error details here" }`               |

---

## **Notes**
1. **Sensitive Data**: The `EmailPassword` field is encrypted and never returned in API responses.
2. **Validation**: Ensure all required fields are provided when creating or updating a user profile.
3. **Field Selection**: Use the `fields` query parameter to retrieve only the data you need.

---

## **Example Requests**

### **Create a User Profile**
```bash
curl -X POST https://mobile-api.kourai.com/api/users \
     -H "Authorization: Bearer <Firebase-Token>" \
     -H "Content-Type: application/json" \
     -d '{
           "Identifier": "unique_identifier_456",
           "UserName": "Alice",
           "LastName": "Johnson",
           "DateOfBirth": "1985-08-15",
           "PhoneNumber": "5555555555",
           "EmailAddress": "alice@example.com",
           "EmailPassword": "supersecurepassword",
           "Address": "789 Oak St",
           "Country": "UK",
           "State": "London",
           "ZIP": "SW1A 1AA",
           "Provider": "Firebase"
         }'
```

### **Retrieve Specific Fields**
```bash
curl -X GET "https://mobile-api.kourai.com/api/users/<user_uid>?fields=UserName,LastName,Email" \
     -H "Authorization: Bearer <Firebase-Token>"
```

### **Update a User Profile**
```bash
curl -X PUT https://mobile-api.kourai.com/api/users/<user_uid> \
     -H "Authorization: Bearer <Firebase-Token>" \
     -H "Content-Type: application/json" \
     -d '{
           "UserName": "Jane",
           "LastName": "Doe"
         }'
```

---

This documentation should provide the mobile app team with everything they need to integrate with your API. Let me know if you'd like to add or modify anything!

