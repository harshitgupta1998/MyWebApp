# MyWebApp

A simple **Spring Boot + Thymeleaf + MySQL** web application for managing users (CRUD operations).

This project provides a basic UI to:
- View users
- Add a new user
- Edit an existing user
- Delete a user

## Overview

The application uses:
- **Spring Boot** for application bootstrap and MVC
- **Spring Data JPA** for database access
- **Thymeleaf** for server-side rendered HTML templates
- **MySQL** as the backing database
- **Bootstrap (WebJars)** for UI styling

## Features

- Home page (`/`) with a link to user management
- User list page (`/users`)
- Create user form (`/users/new`)
- Edit user form (`/users/edit/{id}`)
- Delete user (`/users/delete/{id}`)
- Save user (`POST /users/save`)
- Flash messages after save/delete operations

## Tech Stack

- **Java 17**
- **Spring Boot 3.5.3**
- **Spring Boot Starter Web**
- **Spring Boot Starter Thymeleaf**
- **Spring Boot Starter Data JPA**
- **MySQL Connector/J**
- **Bootstrap 4.3.1 (WebJar)**
- **Maven** (includes Maven Wrapper: `mvnw`, `mvnw.cmd`)

## Project Structure

```text
MyWebApp/
├── pom.xml
├── mvnw / mvnw.cmd
├── .mvn/
└── src/
    └── main/
        ├── java/
        │   └── com/mycompany/
        │       ├── MyWebAppApplication.java
        │       ├── MainController.java
        │       └── user/
        │           ├── User.java
        │           ├── UserController.java
        │           ├── UserRepository.java
        │           ├── UserService.java
        │           └── UserNotFoundException.java
        └── resources/
            ├── application.properties
            └── templates/
                ├── index.html
                ├── users.html
                └── user_form.html
```

## How the Codebase Works

### 1) Application entry point
- `MyWebAppApplication.java` starts the Spring Boot app.

### 2) Home route
- `MainController.java` maps the root path (`/`) and returns the `index` template.

### 3) User module (MVC + Service + Repository)
- `UserController.java` handles web routes for listing, creating, editing, saving, and deleting users.
- `UserService.java` contains business logic and delegates DB operations to the repository.
- `UserRepository.java` extends Spring Data `CrudRepository` and includes `countById(Integer id)`.
- `User.java` is the JPA entity mapped to the `users` table.
- `UserNotFoundException.java` is used when edit/delete is attempted with a missing user ID.

### 4) Templates
- `index.html` – simple landing page with “Manage Users” action
- `users.html` – displays users in a table and supports edit/delete actions
- `user_form.html` – form for add/edit user flows

## Database Configuration

Current configuration in `application.properties` points to a local MySQL database:

- Database: `usersdb`
- URL: `jdbc:mysql://localhost:3306/usersdb`
- Username: `root`
- Password: `admin`
- JPA DDL mode: `update`

> **Important:** For local development this works, but for production use environment variables or externalized config instead of committing credentials.

## Prerequisites

Before running the app, install:
- **JDK 17**
- **MySQL Server** (running locally)
- (Optional) **Maven** — not required if using the Maven Wrapper (`./mvnw`)

## Setup and Run

### 1) Create the database
Run this in MySQL:

```sql
CREATE DATABASE usersdb;
```

### 2) Update database credentials (if needed)
Edit `src/main/resources/application.properties` and change:
- `spring.datasource.username`
- `spring.datasource.password`
- `spring.datasource.url`

### 3) Run the application
Using Maven Wrapper:

```bash
# macOS / Linux
./mvnw spring-boot:run
```

```bat
:: Windows
mvnw.cmd spring-boot:run
```

Or with Maven installed:

```bash
mvn spring-boot:run
```

### 4) Open in browser
- Home: `http://localhost:8080/`
- User Management: `http://localhost:8080/users`

## User Entity (Observed Fields)

The `User` entity is mapped to the `users` table and includes fields such as:
- `id` (auto-generated)
- `email`
- `password`
- `firstName` (`first_name` column)
- `lastName` (`last_name` column)
- `enabled`

## Routes Summary

### View Routes
- `GET /` → Home page
- `GET /users` → List all users
- `GET /users/new` → Show new user form
- `GET /users/edit/{id}` → Show edit form
- `GET /users/delete/{id}` → Delete a user and redirect

### Form Submission
- `POST /users/save` → Save new/updated user

## Improvement Opportunities (from current code)

These are optional but recommended if you continue developing the project:

1. **Do not store plaintext DB credentials in source control**
   - Move DB config to environment variables or profiles.

2. **Do not store plaintext user passwords**
   - Hash passwords (e.g., BCrypt) before saving.

3. **Use generics in repository/service/controller lists**
   - Example: `CrudRepository<User, Integer>`, `List<User>`.

4. **Clean unused imports in `UserController`**
   - There are unnecessary imports (e.g., Swing / extra Spring annotations).

5. **Improve `UserNotFoundException` message handling**
   - Pass the message to `super(message)` so flash messages are consistent.

6. **Add validation**
   - Use Bean Validation annotations (`@NotBlank`, `@Email`, etc.) and show validation messages in the form.

7. **Add tests**
   - Controller/service tests for CRUD flows.

## Notes

- `spring.jpa.hibernate.ddl-auto=update` is convenient for development but should be used carefully in production.
- The UI templates are Thymeleaf-based and render data from MVC model attributes.

## License

No license file is currently present in the repository. Add a `LICENSE` file if you plan to share or reuse this project publicly.
