 # Library API Documentation

## Introduction

### Overview
The Library API is designed for users viewing and library administrators to manage books and authors. It provides endpoints for registering and authenticating users, as well as viewing, adding and modifying book information.

### Authentication
This API uses **JSON Web Tokens (JWT)** for secure authentication. A valid JWT token must be included in the header of requests that require authentication. Tokens are valid for:
- **Admin Users**: 1 hour
- **Regular Users**: 1 hour

### Error Handling
The API returns consistent error codes and response formats for different error scenarios. Error responses are provided in JSON format with a `status` and `data` field, where `data` contains the error message.

---

 # Endpoints

### Endpoint 1: Register User

- **URL**: `/users/register`
- **Method**: `POST`
- **Description**: Registers a new user in the system, saving their username and password in the database.

#### Request Parameters
| Parameter   | Type   | Description                   | Required | Example                        |
|-------------|--------|-------------------------------|----------|--------------------------------|
|`username`   | string | The desired username          | Yes      | `Joshu`                        |
| `password`  | string | The password (hashed securely)| Yes      | `opengate`                     |

### Example Request
```json
{
    "username": "Joshu",
    "password": "opengate"
}
```

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
- **Description**: Authenticates a user by verifying their username and password. If successful, generates a JSON Web Token (JWT).

#### Request Parameters
| Parameter | Type   | Description                 | Required | Example    |
|-----------|--------|-----------------------------|----------|------------|
| username  | string | The user's username         | Yes      | `joshu`    |
| password  | string | The user's account password | Yes      | `opengate` |

### Example Request
```json
{
    "username": "joshu",
    "password": "opengate"
}
```

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

### Endpoint 3: Display Books by Genre
- **URL**: `/display/genrebooks`
- **Method**: `GET`
- **Description**: Displays a list of books filtered by genre if the genre exists in the database.

#### Request Parameters
| Parameter    | Type   | Description                          | Required | Example                        |
|--------------|--------|--------------------------------------|----------|--------------------------------|
| `bookgenre`  | string | The genre of the books to display.   | Yes      | `Fiction`                      |
| `token`      | string | A valid JWT token for authentication.| Yes      | `"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMDM4NTAsImV4cCI6MTczMDMwNzQ1MCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.cHV3NFEarnoRpLqfmSgkMohh42rQ4tZ42bzRKKcf6yA"`                    |

### Example Request
```json
{
    "bookgenre": "Fiction",
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMDM4NTAsImV4cCI6MTczMDMwNzQ1MCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.cHV3NFEarnoRpLqfmSgkMohh42rQ4tZ42bzRKKcf6yA"
}
```

### Responses

- **Success Response**:
- **Status Code**: `200 OK`

- **Response Body** (if books are found):

```json
{
    "status": "success",
    "new_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMDM4NTAsImV4cCI6MTczMDMwNzQ1MCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.cHV3NFEarnoRpLqfmSgkMohh42rQ4tZ42bzRKKcf6yA",
    "data": [
        {
            "bookid": "1",
            "title": "Sample Book Title",
            "genre": "Fiction",
            "bookCode": "BK001",
            "authorid": "1",
            "authorname": "Author Name"
        },
        {
            "bookid": "2",
            "title": "Another Book Title",
            "genre": "Fiction",
            "bookCode": "BK002",
            "authorid": "2",
            "authorname": "Another Author"
        }
    ]
}
```

- **Error Responses**:
- **Token Invalid or Outdated**:
    - **Status Code**: `401 Unauthorized`
    - **Error Message**:

    ```json
    {
        "status": "fail",
        "data": {
            "Message": "Invalid or Outdated Token."
        }
    }
    ```

- **Database Error**:
    - **Status Code**: `500 Internal Server Error`
    - **Error Message**:

    ```json
    {
        "status": "fail",
        "data": {
            "Message": "Database error message here."
        }
    }
    ```

- **General Exception**:
    - **Status Code**: `500 Internal Server Error`
    - **Error Message**:

    ```json
    {
        "status": "fail",
        "data": {
            "Message": "Exception message here."
        }
    }
    ```

---

## Update Author API (Admin)

- **URL**: `/update/authors`
- **Method**: `POST`
- **Description**: Updates the details of an author in the database. This action is restricted to users with admin access.

#### Request Parameters
| Parameter    | Type   | Required | Description                                       | Example                              |
|--------------|--------|----------|---------------------------------------------------|--------------------------------------|
| authorid     | string | Yes      | The ID of the author to update.                   | `"1"`                                |
| authorname   | string | No       | The new name of the author (if applicable).       | `"Joshua (Updated Author Name)"`     |
| token        | string | Yes      | A valid JWT token for authentication.             | `"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMDM4NTAsImV4cCI6MTczMDMwNzQ1MCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.cHV3NFEarnoRpLqfmSgkMohh42rQ4tZ42bzRKKcf6yA"`     |

### Example Request
```json
{
    "authorid": "1",
    "authorname": "Joshua (Updated Autohr Name)",
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMDM4NTAsImV4cCI6MTczMDMwNzQ1MCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.cHV3NFEarnoRpLqfmSgkMohh42rQ4tZ42bzRKKcf6yA"
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
| token     | string | A valid JWT token for admin authorization       | Yes      | `"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMDM4NTAsImV4cCI6MTczMDMwNzQ1MCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.cHV3NFEarnoRpLqfmSgkMohh42rQ4tZ42bzRKKcf6yA"`     |
| books     | array  | Array of book objects, each containing details  | Yes      | See example below           |

Each book object in the `books` array should contain the following fields:

| Parameter | Type   | Description             | Required | Example                |
|-----------|--------|-------------------------|----------|------------------------|
| title     | string | The title of the book   | Yes      | `"Isang Kaibigan"`  |
| genre     | string | The genre of the book   | Yes      | `"Science Fiction"`    |
| author    | string | The author's name       | Yes      | `"Hey-Hey"`        |

#### Example Request Body
```json
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMDM4NTAsImV4cCI6MTczMDMwNzQ1MCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.cHV3NFEarnoRpLqfmSgkMohh42rQ4tZ42bzRKKcf6yA",
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
```
#### Responses

- **Success Response**:
  - **Status Code**: `200`
  - **Response Body**:
    ```json
    {
        "status": "success",
        "new_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMDM4NTAsImV4cCI6MTczMDMwNzQ1MCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.cHV3NFEarnoRpLqfmSgkMohh42rQ4tZ42bzRKKcf6yA"
    }
    ```
  - **new_token**: A new JWT token is returned if the previous token is close to expiring.

- **Error Responses**:
  - **Access Denied**:
    - **Status Code**: `403`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "Access denied, only admins can add books."
          }
      }
      ```
  - **Invalid or Expired Token**:
    - **Status Code**: `401`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "Invalid or Outdated Token."
          }
      }
      ```
  - **Database Error** (e.g., issues with inserting records):
    - **Status Code**: `500`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "Database error message here."
          }
      }
      ```

---

### Endpoint 4: Update Book (Admin)

- **URL**: `/update/books`
- **Method**: `POST`
- **Description**: Allows admins to update the details of a specific book in the library. Requires a valid JWT token for authentication.

#### Request Parameters
| Parameter  | Type   | Description                                   | Required | Example                     |
|------------|--------|-----------------------------------------------|----------|-----------------------------|
| token      | string | A valid JWT token for admin authorization     | Yes      | `"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMDM4NTAsImV4cCI6MTczMDMwNzQ1MCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.cHV3NFEarnoRpLqfmSgkMohh42rQ4tZ42bzRKKcf6yA"`     |
| bookCode   | string | The unique identifier for the book to update  | Yes      | `"915AG"`                   |
| title      | string | The new title of the book                     | No       | `"Updated Book Title"`      |
| genre      | string | The new genre of the book                     | No       | `"Mystery"`                 |
| author     | string | The new author's name                         | No       | `"New Author Name"`         |

#### Example Request Body
```json
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMDM4NTAsImV4cCI6MTczMDMwNzQ1MCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.cHV3NFEarnoRpLqfmSgkMohh42rQ4tZ42bzRKKcf6yA",
    "bookCode": "12345",
    "title": "Updated Book Title",
    "genre": "Mystery",
    "author": "New Author Name"
}
```

#### Responses

- **Success Response**:
  - **Status Code**: `200`
  - **Response Body**:
    ```json
    {
        "status": "success",
        "new_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMDM4NTAsImV4cCI6MTczMDMwNzQ1MCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.cHV3NFEarnoRpLqfmSgkMohh42rQ4tZ42bzRKKcf6yA"
    }
    ```
  - **new_token**: A new JWT token is returned if the previous token is close to expiring.

- **Error Responses**:
  - **Access Denied**:
    - **Status Code**: `403`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "Access denied, only admins can add books."
          }
      }
      ```
  - **Token Invalid or Outdated**:
    - **Status Code**: `401`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "Invalid or Outdated Token."
          }
      }
      ```
  - **Invalid Book Code**:
    - **Status Code**: `404`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "Invalid Book Code."
          }
      }
      ```
  - **No Fields to Update**:
    - **Status Code**: `400`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "No fields to update."
          }
      }
      ```
  - **Database Error**:
    - **Status Code**: `500`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "Database error message here."
          }
      }
      ```

---

---

### Endpoint 4: Delete Book (Admin)

- **URL**: `/delete/books`
- **Method**: `DELETE`
- **Description**: Allows admins to delete a book from the library using its unique book code.

#### Request Parameters
| Parameter | Type   | Description                               | Required | Example                   |
|-----------|--------|-------------------------------------------|----------|---------------------------|
| `token`     | string | A valid JWT token for admin authorization | Yes      | `"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMDM4NTAsImV4cCI6MTczMDMwNzQ1MCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.cHV3NFEarnoRpLqfmSgkMohh42rQ4tZ42bzRKKcf6yA"`   |
| `bookCode`  | string | The unique code of the book to be deleted | Yes      | `"915AG"`      |

#### Example Request Body
```json
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMDM4NTAsImV4cCI6MTczMDMwNzQ1MCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.cHV3NFEarnoRpLqfmSgkMohh42rQ4tZ42bzRKKcf6yA",
    "bookCode": "915AG"
}
```

#### Responses

- **Success Response**:
  - **Status Code**: `200`
  - **Response Body**:
    ```json
    {
        "status": "success",
        "new_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMDM4NTAsImV4cCI6MTczMDMwNzQ1MCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.cHV3NFEarnoRpLqfmSgkMohh42rQ4tZ42bzRKKcf6yA"
    }
    ```
  - **new_token**: A new JWT token is returned if the previous token is close to expiring.

- **Error Responses**:
  - **Access Denied**:
    - **Status Code**: `403`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "Access denied, only admins can add books."
          }
      }
      ```
  - **Token Invalid or Outdated**:
    - **Status Code**: `401`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "Invalid or Outdated Token."
          }
      }
      ```
  - **Invalid Book Code**:
    - **Status Code**: `404`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "Invalid Book Code."
          }
      }
      ```
  - **Database Error**:
    - **Status Code**: `500`
    - **Error Message**:
      ```json
      {
          "status": "fail",
          "data": {
              "Message": "Database error message here."
          }
      }
      ```

---

### Display All Books API

- **URL**: `/displayall/books`
- **Method**: `GET`
- **Description**: This endpoint retrieves all books along with their associated authors from the library database. It requires a valid JWT token for access.

#### Request Parameters
| Parameter | Type   | Description                               | Required | Example                   |
|-----------|--------|-------------------------------------------|----------|---------------------------|
| token     | string | A valid JWT token for user authorization  | Yes      | `"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMTI2MDQsImV4cCI6MTczMDMxNjIwNCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.qvk-3h9KOakIjNvArKpYh1feoGaoTbICA-oyhOOzo4U"`   |

#### Example Request Body
```json
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMTI2MDQsImV4cCI6MTczMDMxNjIwNCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.qvk-3h9KOakIjNvArKpYh1feoGaoTbICA-oyhOOzo4U"
}
```
  
#### Responses

- **Success Response**:
- **Status Code**: `200`
- **Response Body** (if books are found):
    ```json
    {
        "status": "success",
        "new_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMTI2MDQsImV4cCI6MTczMDMxNjIwNCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.qvk-3h9KOakIjNvArKpYh1feoGaoTbICA-oyhOOzo4U",
        "data": [
            {
                "bookid": "1",
                "title": "Book Title 1",
                "genre": "Fiction",
                "bookCode": "BK001",
                "authorid": "1",
                "authorname": "Author Name 1"
            },
            {
                "bookid": "2",
                "title": "Book Title 2",
                "genre": "Non-Fiction",
                "bookCode": "BK002",
                "authorid": "2",
                "authorname": "Author Name 2"
            }
        ]
    }
    ```
- **new_token**: A new JWT token is returned if the previous token is close to expiring.

- **Response Body** (if no books are found):
    ```json
    {
        "status": "success",
        "data": "No books found."
    }
    ```

- **Error Responses**:
- **Token Invalid or Outdated**:
    - **Status Code**: `401`
    - **Error Message**:
        ```json
        {
            "status": "fail",
            "data": {
                "Message": "Invalid or Outdated Token."
            }
        }
        ```

- **Database Error**:
    - **Status Code**: `500`
    - **Error Message**:
        ```json
        {
            "status": "fail",
            "data": {
                "title": "Database error message here."
            }
        }
        ```

- **General Exception**:
    - **Status Code**: `500`
    - **Error Message**:
        ```json
        {
            "status": "fail",
            "data": {
                "title": "Exception message here."
            }
        }
        ```

---

### Display Books by Author API
- **URL**: `/display/authorsbooks`
- **Method**: `GET`
- **Description**: This endpoint retrieves all books written by a specified author. It requires a valid JWT token for access.

#### Request Parameters
| Parameter   | Type   | Description                               | Required | Example                   |
|-------------|--------|-------------------------------------------|----------|---------------------------|
| token       | string | A valid JWT token for user authorization  | Yes      | `"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMTI2MDQsImV4cCI6MTczMDMxNjIwNCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.qvk-3h9KOakIjNvArKpYh1feoGaoTbICA-oyhOOzo4U"`   |
| authorname  | string | The name of the author whose books to retrieve | Yes  | `"Author1"`           |

#### Example Request Body
```json
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMTI2MDQsImV4cCI6MTczMDMxNjIwNCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.qvk-3h9KOakIjNvArKpYh1feoGaoTbICA-oyhOOzo4U",
    "authorname": "Author1"
}
```

#### Responses

- **Success Response**:
- **Status Code**: `200`

- **Response Body** (if books are found):
    ```json
    {
        "status": "success",
        "new_token": "new_jwt_token_here",
        "data": [
            {
                "bookid": "1",
                "title": "Book Title 1",
                "genre": "Fiction",
                "bookCode": "BK001",
                "authorid": "1",
                "authorname": "Author1"
            },
            {
                "bookid": "2",
                "title": "Book Title 2",
                "genre": "Non-Fiction",
                "bookCode": "BK002",
                "authorid": "1",
                "authorname": "Author1"
            }
        ]
    }
    ```
    **new_token**: A new JWT token is returned if the previous token is close to expiring.

- **Response Body** (if no books are found):
    ```json
    {
        "status": "fail",
        "data": {
            "Message": "No such author exists."
        }
    }
    ```

- **Error Responses**:
- **Token Invalid or Outdated**:
    - **Status Code**: `401`
    - **Error Message**:
        ```json
        {
            "status": "fail",
            "data": {
                "Message": "Invalid or Outdated Token."
            }
        }
        ```

- **Database Error**:
    - **Status Code**: `500`
    - **Error Message**:
        ```json
        {
            "status": "fail",
            "data": {
                "Message": "Database error message here."
            }
        }
        ```

- **General Exception**:
    - **Status Code**: `500`
    - **Error Message**:
        ```json
        {
            "status": "fail",
            "data": {
                "Message": "Exception message here."
            }
        }
        ```

---

### Display Books by Title API

- **URL**: `/display/titlebooks`
- **Method**: `GET`
- **Description**: Displays a list of books filtered by title if the genre exists in the database. It requires a valid JWT token for access.

#### Endpoint
`GET /display/titlebooks`

#### Request Parameters
| Parameter  | Type   | Description                                | Required | Example                  |
|------------|--------|--------------------------------------------|----------|--------------------------|
| token      | string | A valid JWT token for user authorization   | Yes      | `"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMTI2MDQsImV4cCI6MTczMDMxNjIwNCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.qvk-3h9KOakIjNvArKpYh1feoGaoTbICA-oyhOOzo4U"`  |
| booktitle  | string | The title of the book to search for        | Yes      | `"Isang Kaibigan"`    |

#### Example Request Body
```json
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMTI2MDQsImV4cCI6MTczMDMxNjIwNCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.qvk-3h9KOakIjNvArKpYh1feoGaoTbICA-oyhOOzo4U",
    "booktitle": "Isang Kaibigan"
}
```

### Responses

- **Success Response**:
- **Status Code**: `200`

- **Response Body** (if books are found):
    ```json
    {
        "status": "success",
        "new_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMTI2MDQsImV4cCI6MTczMDMxNjIwNCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.qvk-3h9KOakIjNvArKpYh1feoGaoTbICA-oyhOOzo4U",
        "data": [
            {
                "bookid": "1",
                "title": "Isang Kaibigan",
                "genre": "Fiction",
                "bookCode": "BK001",
                "authorid": "1",
                "authorname": "Author Name"
            }
        ]
    }
    ```
    **new_token**: A new JWT token is returned if the previous token is close to expiring.

- **Response Body** (if no books are found):
    ```json
    {
        "status": "fail",
        "data": {
            "Message": "No such book title exists."
        }
    }
    ```

- **Error Responses**:
- **Token Invalid or Outdated**:
    - **Status Code**: `401`
    - **Error Message**:
        ```json
        {
            "status": "fail",
            "data": {
                "Message": "Invalid or Outdated Token."
            }
        }
        ```

- **Database Error**:
    - **Status Code**: `500`
    - **Error Message**:
        ```json
        {
            "status": "fail",
            "data": {
                "Message": "Database error message here."
            }
        }
        ```

- **General Exception**:
    - **Status Code**: `500`
    - **Error Message**:
        ```json
        {
            "status": "fail",
            "data": {
                "Message": "Exception message here."
            }
        }
        ```

---
        
## Display Books by Genre API

- **URL**: `/display/genrebooks`
- **Method**: `GET`
- **Description**: Displays a list of books filtered by genre if the genre exists in the database.

### Request Parameters
| Parameter   | Type   | Description                                | Required | Example                  |
|-------------|--------|--------------------------------------------|----------|--------------------------|
| `bookgenre` | string | Genre of the  book to search for           |   Yes    | `"Fiction"`        |
| `token`     | string | A valid JWT token for user authorization   |     Yes  | `"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMTI2MDQsImV4cCI6MTczMDMxNjIwNCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc1sZXZlbCI6ImFkbWluIn19.qvk-3h9KOakIjNvArKpYh1feoGaoTbICA-oyhOOzo4U"`     |

### Success Response
- **Status Code**: `200 OK`

- **Response Body** (if books are found):
    ```json
    {
        "status": "success",
        "new_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbGlicmFyeS5vcmciLCJhdWQiOiJodHRwOi8vbGlicmFyeS5jb20iLCJpYXQiOjE3MzAzMTI2MDQsImV4cCI6MTczMDMxNjIwNCwiZGF0YSI6eyJ1c2VyaWQiOjEwOSwibmFtZSI6InNhcmxlbiIsImFjY2Vzc19sZXZlbCI6ImFkbWluIn19.qvk-3h9KOakIjNvArKpYh1feoGaoTbICA-oyhOOzo4U",
        "data": [
            {
                "bookid": "1",
                "title": "Sample Book Title",
                "genre": "Fiction",
                "bookCode": "BK001",
                "authorid": "1",
                "authorname": "Author Name"
            },
            {
                "bookid": "2",
                "title": "Another Book Title",
                "genre": "Fiction",
                "bookCode": "BK002",
                "authorid": "2",
                "authorname": "Another Author"
            }
        ]
    }
    ```
    - **new_token**: A new JWT token is provided if the previous token is close to expiration.

- **Response Body** (if no books are found for the specified genre):
    ```json
    {
        "status": "fail",
        "data": {
            "Message": "No such book genre exists."
        }
    }
    ```

### Error Responses
- **Token Invalid or Outdated**:
    - **Status Code**: `401 Unauthorized`
    - **Error Message**:
        ```json
        {
            "status": "fail",
            "data": {
                "Message": "Invalid or Outdated Token."
            }
        }
        ```

- **Database Error**:
    - **Status Code**: `500 Internal Server Error`
    - **Error Message**:
        ```json
        {
            "status": "fail",
            "data": {
                "Message": "Database error message here."
            }
        }
        ```

- **General Exception**:
    - **Status Code**: `500 Internal Server Error`
    - **Error Message**:
        ```json
        {
            "status": "fail",
            "data": {
                "Message": "Exception message here."
            }
        }
        ```

### Notes
- **Token Management**: A new JWT token is returned in the response if the previous token is close to expiration. The updated token is also stored in the database for future requests.
- **Database**: Uses a MySQL database to fetch book details, and verifies the user's token for security purposes.
