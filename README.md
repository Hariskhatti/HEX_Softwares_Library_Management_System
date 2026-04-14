# 📚 **Library Management System**

### *A Complete Spring Boot + PostgreSQL REST API*

<p align="center">
  <img src="https://img.shields.io/badge/Java-JDK%2017-red?style=for-the-badge">
  <img src="https://img.shields.io/badge/Spring%20Boot-3.5.13-brightgreen?style=for-the-badge">
  <img src="https://img.shields.io/badge/PostgreSQL-Database-blue?style=for-the-badge">
  <img src="https://img.shields.io/badge/Lombok-Enabled-yellow?style=for-the-badge">
  <img src="https://img.shields.io/badge/Swagger-OpenAPI-orange?style=for-the-badge">
  <img src="https://img.shields.io/badge/Status-Stable-brightgreen?style=for-the-badge">
</p>

A full-stack **Library Management REST API** built using **Spring Boot** and **PostgreSQL**.
This system allows librarians to **add books**, **register members**, **issue & return books**, and **track borrowing history** — with real-time availability status and complete exception handling.

---

# 🌐 **API Preview (Swagger UI)**

### 📋 **Available Endpoints**

```
┌────────────────────────────────────────────────────────────────────────┐
│  📚 HEX SOFTWARES — LIBRARY MANAGEMENT SYSTEM                          │
│----------------------------------------------------------------------  │
│                                                                        │
│  POST   /library/addBooks              → Add a New Book                │
│  POST   /library/addMember             → Register a New Member         │
│  GET    /library/available-books       → View All Available Books      │
│  POST   /library/issue                 → Issue Book to Member          │
│  GET    /library/issued-books          → View All Currently Issued     │
│  GET    /library/member-books          → View Books by Member Name     │
│  POST   /library/return                → Return an Issued Book         │
│                                                                        │
│  📖 Swagger Docs: http://localhost:8080/swagger-ui/index.html          │
└────────────────────────────────────────────────────────────────────────┘
```

### 📦 **Sample Request — Add Book**

```json
POST /library/addBooks
{
  "title":  "Clean Code",
  "author": "Robert C. Martin"
}
```

### 📦 **Sample Request — Register Member**

```json
POST /library/addMember
{
  "name":  "Haris Khatti",
  "email": "haris@example.com"
}
```

### 📦 **Sample Request — Issue Book**

```
POST /library/issue?bookTitle=Clean Code&memberName=Haris Khatti
```

### 📦 **Sample Response**

```
Book issued successfully
```

---

# 🔑 **System Features**

## 📖 **Book Management**

* ✅ **Add Book** – Save book with title and author; availability set to `false` (not issued) by default
* 📋 **View Available Books** – Lists all books where `isIssued = false`
* 🔄 **Auto Status Update** – Book marked as issued on loan, and restored to available on return

## 👤 **Member Management**

* ✅ **Register Member** – Save member with name and email
* 🔎 **Input Validation** – Name and email are required fields (validated in service layer)
* 🆔 **Auto ID Generation** – Member ID auto-generated on registration

## 🔄 **Issue & Return System**

* 📤 **Issue Book** – Links a book to a member and records `issueDate` automatically
* 📥 **Return Book** – Sets `returnDate` on the library record and marks book as available again
* 📊 **View Issued Books** – Returns all records where `returnDate` is null (currently issued)
* 👤 **Member Borrowed Books** – View all active (unreturned) books for a specific member

## 🛡️ **Exception Handling**

* ❌ **Book Not Found** – `RuntimeException` returns `404 Not Found`
* ❌ **Member Not Found** – `RuntimeException` returns `404 Not Found`
* ❌ **Book Already Issued** – `IllegalArgumentException` returns `400 Bad Request`
* ❌ **Missing Member Fields** – `IllegalArgumentException` for null/empty name or email
* ❌ **No Active Issue Record** – `RuntimeException` when trying to return a non-issued book

---

# 🛠️ **Tech Stack**

| Layer       | Technology                      |
| ----------- | ------------------------------- |
| Backend     | Java 17 + Spring Boot 3.5.13    |
| API Style   | REST (Spring Web)               |
| Database    | PostgreSQL                      |
| ORM         | Spring Data JPA (Hibernate)     |
| Boilerplate | Lombok                          |
| API Docs    | Springdoc OpenAPI (Swagger UI)  |
| Build Tool  | Maven                           |
| IDE         | Eclipse                         |

---

# 🗃️ **Database Setup (PostgreSQL)**

### **1. Create Database**

```sql
CREATE DATABASE hoteldb;
```

> ⚠️ The database name used in this project is `hoteldb` (as configured in `application.properties`).

### **2. Tables — Auto Created**

Hibernate is configured with `ddl-auto=update`, so all tables are **automatically created** on first run.

The following tables will be generated:

```
BookEntity     → stores book title, author & issued status
MemberEntity   → stores member name & email
LibraryEntity  → stores issue/return records with dates & foreign keys
```

### **3. Configure DB Connection**

Edit `src/main/resources/application.properties`:

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/hoteldb
spring.datasource.username=postgres
spring.datasource.password=your_pg_password
```

---

# 🚀 **How to Run the Project**

### **1. Clone / Import in Eclipse**

```
File → Import → Existing Maven Projects → Select project folder
```

### **2. Configure Database**

Update credentials in `application.properties` as shown above.

### **3. Run the Application**

```
Right-click project → Run As → Spring Boot App
```

### **4. Access Swagger UI**

```
http://localhost:8080/swagger-ui/index.html
```

---

# 📁 **Project Structure**

```
HexSoftwares_LibraryManagementSyetem/
│
├── src/main/java/hello/security/main/
│   ├── LibraryController.java                     # REST API Endpoints
│   ├── ServiceClass.java                          # Business Logic
│   ├── HexSoftwaresLibraryManagementSyetemApplication.java
│   │
│   ├── entities/
│   │   ├── BookEntity.java                        # Book Entity (JPA)
│   │   ├── MemberEntity.java                      # Member Entity (JPA)
│   │   └── LibraryEntity.java                     # Issue/Return Record Entity (JPA)
│   │
│   ├── repository/
│   │   ├── BookRepository.java                    # JPA Repo for Books
│   │   ├── MemberRepository.java                  # JPA Repo for Members
│   │   └── LibraryRepository.java                 # JPA Repo for Issue Records
│   │
│   └── exception/
│       └── GlobalException.java                   # @RestControllerAdvice handlers
│
├── src/main/resources/
│   └── application.properties                     # DB config & JPA settings
│
├── pom.xml                                        # Maven dependencies
└── README.md
```

---

# ✅ **API Endpoints Summary**

| Method | Endpoint                   | Params                             | Description                         |
| ------ | -------------------------- | ---------------------------------- | ----------------------------------- |
| `POST` | `/library/addBooks`        | JSON Body (title, author)          | Add a new book to the library       |
| `POST` | `/library/addMember`       | JSON Body (name, email)            | Register a new library member       |
| `GET`  | `/library/available-books` | —                                  | View all books not currently issued |
| `POST` | `/library/issue`           | `bookTitle`, `memberName` (params) | Issue a book to a member            |
| `GET`  | `/library/issued-books`    | —                                  | View all currently issued books     |
| `GET`  | `/library/member-books`    | `memberName` (param)               | View active loans for a member      |
| `POST` | `/library/return`          | `bookTitle`, `memberName` (params) | Return an issued book               |

---

# 🗂️ **Entity Relationships**

```
MemberEntity  ──────────────────────┐
                                     │  ManyToOne
BookEntity    ──────────────────────┼──→  LibraryEntity
                                     │     (issueDate, returnDate)
                              ManyToOne
```

> `LibraryEntity` acts as the **junction table** between books and members,
> recording every issue and return with timestamps.

---

# 🛡️ **Validation Rules**

| Field        | Rule                                          |
| ------------ | --------------------------------------------- |
| `name`       | Required — cannot be null or empty            |
| `email`      | Required — cannot be null or empty            |
| `bookTitle`  | Must exist in the database                    |
| `memberName` | Must exist in the database                    |
| Book Status  | Cannot be issued if already marked as issued  |
| Return       | Must have an active (unreturned) issue record |

---

# 🌱 **Future Enhancements**

* 🔐 Spring Security – Admin & Member login with JWT
* 📅 Due Date & Fine System – Overdue penalty calculation
* 📩 Email Notifications – On book issue and return
* 🔍 Search & Filter – Books by author, genre, availability
* 📊 Analytics Dashboard – Most borrowed books, active members

---

# 👨‍💻 **Author**

### **HEX Softwares Internship Task**
> Built as Task 2 of the HEX Softwares Internship Program.

---

# 📄 **License**

This project is for **educational / internship use** only.

---
