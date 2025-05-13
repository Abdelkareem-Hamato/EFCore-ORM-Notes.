![Cover](./Readme.jpg)
# EF Core & ORMs - Notes

## Introduction

Back-End typically separates into two parts:

- **App (Application Logic)**
- **Database (Data Storage)**

We connect applications with databases to make data **dynamic**, so it can respond to **user inputs, updates, and deletions**. Without database connectivity, an app would be limited to static content.

---

## How Do Applications Connect to Databases?

Two main approaches:

- **ADO.NET** – Low-level database interaction via manual SQL.
- **ORM (Object Relational Mapping)** – High-level abstraction that automates the link between app objects and database tables.

---

## What is ORM?

**ORM** bridges the gap between:
- **Object** → Business model/class in the application.
- **Relation** → Table in the database.

ORM allows developers to work on **one side**, and it automatically **generates/mirrors the other** (either DB from code or code from DB).

---

## Two ORM Approaches:

### 1. Code-First
- Build classes (Models) in code.
- ORM generates database tables/views.
- Easier and more common in modern apps.

### 2. Database-First
- Start by building tables in SQL.
- ORM generates domain models and DbContext.

---

## Key Components

### Model:
Represents the shape of data (e.g., one table).

### DbContext:
Manages database connection and tracks all models via `DbSet<Model>` properties.

---

## EF Core Features

- **Automatic Schema Mapping**
- **LINQ to SQL translation**
- **Change Tracker**: Tracks object states (Unchanged, Modified, Deleted)
- Supports multiple DB providers (SQL Server, MySQL, PostgreSQL...)

---

## Querying Data with EF Core

- For display only: disable tracking → better performance.
- For modifications: use change tracker.

---

## `Update-Database` Command Workflow:

1. Creates `DbContext` object.
2. Calls `Database.Migrate()` method.
3. Applies any pending migrations.

---

## Dapper Overview

- Micro ORM, lightweight and fast.
- Ideal for **read-heavy** operations.
- No change tracker, no mapping – pure SQL queries.
- Faster than EF Core in simple data retrieval scenarios.

---

## EF Core vs Dapper

| Feature              | EF Core              | Dapper               |
|----------------------|----------------------|----------------------|
| Mapping              | Yes                  | No                   |
| LINQ Support         | Yes                  | No (Raw SQL)         |
| Performance          | Slower in reads      | Very fast            |
| Features             | Rich features        | Minimalist           |
| Ease of Use          | Easy with abstraction| Easy, but SQL needed |
| Flexibility          | Requires mapping     | Very flexible        |

---

## When to Use What?

- Use **EF Core** for full-featured CRUD + schema migration.
- Use **Dapper** when performance in reads matters most.
- **CQRS Pattern**: Use EF Core for write operations, Dapper for read.

---

## Data Annotations in EF Core

Used to define schema or perform validation:

```csharp
[Key]
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
[StringLength(50)]
[NotMapped]
[Range(22, 60)]
```

- Some are for **SQL mapping**, others only for **app-level validation** (e.g., in ASP.NET Core).

---

## ADO.NET vs ORMs

|                     | ADO.NET              | EF Core              |
|---------------------|----------------------|----------------------|
| Control             | Full manual control  | Abstracted           |
| Effort              | More code            | Less code            |
| Performance         | High                 | Moderate             |
| Mapping             | No                   | Yes                  |
| Feature Set         | Basic                | Advanced             |

---

## Summary

- EF Core is the **default ORM** for .NET applications today.
- Dapper shines in **performance-critical read operations**.
- ADO.NET is still useful for **full control** or legacy apps.

> Choose the right tool based on your project needs.
