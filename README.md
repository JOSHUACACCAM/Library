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
              "Message": "Username and password cannot be empty."
          }
      }
      ```
  - **Username Already Exists**
    - **Status Code**: `400`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "Username already taken!"
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
        "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMDM4NTAsImV4cCI6MTczMDMwNzQ1MCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.cHV3NFEarnoRpLqfmSgkMohh42rQ4tZ42bzRKKcf6yA"
    }
    ```
  - **token**: A JWT token generated for the user, valid for 1 hour for admin users and regular users.

- **Error Responses**:
  - **Invalid Credentials**
    - **Status Code**: `401`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "Invalid username or password."
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

### Endpoint 3: Add Book (Admin)

- **URL**: `/add/books`
- **Method**: `POST`
- **Description**: Allows admins to add multiple books to the library. Expects an array of book details, including title, genre, and author name.

#### Request Parameters
| Parameter | Type   | Description                                     | Required | Example                     |
|-----------|--------|-------------------------------------------------|----------|-----------------------------|
| token     | string | A valid JWT token for admin authorization       | Yes      | `"your_jwt_token_here"`     |
| books     | array  | Array of book objects, each containing details  | Yes      | See example below           |

Each book object in the `books` array should contain the following fields:

| Parameter | Type   | Description             | Required | Example                |
|-----------|--------|-------------------------|----------|------------------------|
| title     | string | The title of the book   | Yes      | `"Sample Book Title"`  |
| genre     | string | The genre of the book   | Yes      | `"Science Fiction"`    |
| author    | string | The author's name       | Yes      | `"Author Name"`        |

#### Example Request Body
```json
{
    "token": "your_jwt_token_here",
    "books": [
        {
            "title": "Sample Book Title 1",
            "genre": "Science Fiction",
            "author": "Author Name 1"
        },
        {
            "title": "Sample Book Title 2",
            "genre": "Fantasy",
            "author": "Author Name 2"
        }
    ]
}
