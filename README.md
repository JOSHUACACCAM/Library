 # Library API Documentation

## Introduction

### Overview
The Library API is designed for users viewing and library administrators to manage books and authors. It provides endpoints for registering and authenticating users, as well as viewing, adding and modifying book information.

### Authentication
This API uses **JSON Web Tokens (JWT)** for secure authentication. A valid JWT token must be included in the header of requests that require authentication. Tokens are valid for:
- **Admin Users**: 1 hour
- **Regular Users**: 2 hours

### Error Handling
The API returns consistent error codes and response formats for different error scenarios. Error responses are provided in JSON format with a `status` and `data` field, where `data` contains the error message.

## Endpoints

### Endpoint 1: Register User

- **URL**: `/users/register`
- **Method**: `POST`
- **Description**: Registers a new user in the system, saving their email, username, and password in the database.

#### Request Parameters
| Parameter | Type   | Description                   | Required | Example                        |
|-----------|--------|-------------------------------|----------|--------------------------------|
| username  | string | The desired username          | Yes      | `Joshu`                       |
| password  | string | The password (hashed securely)| Yes      | `opengate`                    |

#### Responses

- **Success Response**:
  - **Status Code**: `200`
  - **Response Body**:
    ```json
    {
        "status": "success",
        "data": null
    }
    ```

- **Error Responses**:
  - **Missing Fields**
    - **Status Code**: `400`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "Fields cannot be empty."
          }
      }
      ```
  - **Email Already Exists**
    - **Status Code**: `400`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "Invalid Email! Try another one."
          }
      }
      ```
  - **Registration Failure**
    - **Status Code**: `500`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "Registration failed."
          }
      }
      ```

---

### Endpoint 2: User Login

- **URL**: `/users/login`
- **Method**: `POST`
- **Description**: Authenticates a user by verifying their email and password. If successful, generates a JSON Web Token (JWT).

#### Request Parameters
| Parameter | Type   | Description               | Required | Example    |
|-----------|--------|---------------------------|----------|------------|
| username  | string | The user's username       | Yes      | `joshu`    |
| password  | string | The user's account password | Yes    | `opengate` |

#### Responses

- **Success Response**:
  - **Status Code**: `200`
  - **Response Body**:
    ```json
    {
        "status": "success",
        "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..."
    }
    ```
  - **token**: A JWT token generated for the user, valid for 1 hour for admin users and 2 hours for regular users.

- **Error Responses**:
  - **Invalid Credentials**
    - **Status Code**: `401`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "Invalid email or password."
          }
      }
      ```
  - **Login Failure**
    - **Status Code**: `500`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "Login failed."
          }
      }
      ```

---

### Endpoint 3: Add Book

- **URL**: `/books/add`
- **Method**: `POST`
- **Description**: Allows admins to add new books to the library system.
- **Request Parameters**: (Define as needed)
  
## License
This project is licensed under the MIT License.

---

For more information, please refer to the individual endpoint documentation or contact our support team.
