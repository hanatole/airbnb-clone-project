# 🏡 Airbnb Clone Backend API

A RESTful backend API built with Django and Django REST Framework (DRF), replicating Airbnb’s core functionalities: user authentication, property listings, reservations, payments, and reviews.

## 🏆 Project Goals

1. User Management: Secure registration, authentication, and profile management.
2. Property Management: Create, update, and retrieve property listings efficiently.
3. Booking System: Enable users to reserve properties, manage bookings, and track availability.
4. Payment Processing: Handle transactions and record payment details securely.
5. Review System: Users can leave ratings and feedback for properties, hosts, and guests.
6. Data Optimization: Ensure fast and efficient data retrieval with optimized database design.

## 👥 Team Roles

| Role                  | Responsibilities                                                                                                                                                                      |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Product Owner**     | Defines product vision and roadmap, prioritizes features, manages the backlog, and ensures the product meets user needs and business goals.                                           |
| **DevOps Engineer**   | Manages deployment pipelines, CI/CD, and infrastructure; ensures scalability, reliability, and security; automates builds, tests, and deployments; monitors system performance.       |
| **QA Engineer**       | Designs and executes test plans (manual & automated), identifies bugs, verifies fixes, ensures features meet requirements, and supports continuous code quality improvement.          |
| **Backend Developer** | Develops and maintains server-side logic and APIs, implements database models and business logic, integrates external services (like payments), and ensures performance and security. |


## 🏗️ Technology Stack

| Layer                          | Technology                                |
|--------------------------------|-------------------------------------------|
| **Backend Framework**          | `Django 5.2`                              |
| **API Framework**              | `Django REST Framework`                   |
| **Database**                   | `PostgreSQL`                              |
| **Data Querying**              | `GraphQL`                                 |
| **Asynchronous tasks & Cache** | `Celery`, `Redis`                         |
| **Containerization**           | `Docker`                                  |
| **CI/CD**                      | `Github actions`                          |
| **Authentication**             | JWT (via `djangorestframework-simplejwt`) |
| **Environment Management**     | `uv`                                      |

## 🛢️ Database Design

- **Users**

    **Purpose**: Represents both hosts (property owners) and guests (travelers).

    **Important Fields**:

    > **id**: Unique identifier (UUID).

    > **username**: User’s unique handle or name.
    
    > **email**: Contact and login credential.
    
    > **role**: Defines whether the user is a host, guest, or admin.
    
    > **date_joined**: Timestamp of account creation.

    **Relationships**:
    
    - A user can own multiple properties (if host).
    - A user can make multiple bookings (if guest).
    - A user can write multiple reviews.
---
- **Properties**

    **Purpose**: Represents a listed accommodation (home, apartment, etc.).
    
    **Important Fields**:
  
    > **id**: Unique identifier (UUID).
   
    > **host**: Foreign key to the user who owns the property.

    > **title**: Name of the property.

    > **description**: Description of the property.
  
    > **location**: City, address, or coordinates.

    > **price**: Price per night.

    **Relationships**:

    - Each property belongs to one host (user).
    - A property can have multiple bookings.
    - A property can receive multiple reviews.
    - A property can have multiple images/media uploads.
---
- **Bookings**

    **Purpose**: Represents a reservation made by a guest.
    
    **Important Fields**:
  
    > **id**: Unique identifier (UUID).
   
    > **property**: Foreign key to the property being reserved.

    > **guest**: Foreign key to the user who made the reservation.

    > **check_in**: Check-in date.
    
    > **check_out**: Check-out date.

    > **status**: Defines whether the booking is pending, confirmed, or canceled.

    **Relationships**:

    - A booking belongs to one property and one guest (user).
    - A booking may have one payment record associated with it.
---
- **Reviews**

    **Purpose**: Allows guests to leave feedback about properties (and optionally hosts).

    **Important Fields**:

    > **id**: Unique identifier (UUID).
    
    > **property**: Foreign key to the Property being reviewed.
    
    > **author**: Foreign key to the User (guest) who wrote it.
    
    > **rating**: Numeric rating (e.g., 1–5 stars).
    
    > **comment**: Text feedback about the stay.

    **Relationships**:
    
    - A review belongs to one property.
    - A review is written by one user (guest).
    - A property can have many reviews.
---
- **Payments**

    **Purpose**: Tracks transactions for completed or pending bookings.

    **Important Fields**:

    > **id**: Unique payment identifier.
    
    > **booking**: Foreign key to the Booking being paid for.
    
    > **amount**: Total payment amount.
    
    > **payment_method**: e.g., credit card, PayPal, Stripe, etc.
    
    > **status**: e.g., Pending, Completed, Failed.
    
    **Relationships**:
    
    - A payment belongs to one booking.
    
    - A booking typically has one payment record.

## 🚀 Feature Breakdown

- 🔐 **User Authentication** — JWT-based login, signup, and profile management  
- 🏘️ **Property Listings** — Create, update, and browse listings  
- 📅 **Reservations** — Book, cancel, and view reservations
- ⭐ **Reviews** — Leave reviews and ratings for hosts and guests  
- 📤 **Media Uploads** — Upload property images  
- 🧩 **RESTful API** — Clean, modular architecture with DRF  
- ⚡️ **Asynchronous Tasks** — Background jobs with Celery for emails, notifications, etc.

## 🔒 API Security

### Key Security Measures

| **Security Measure**                          | **Description**                                                                                                 | **Purpose**                                                                                                           |
|-----------------------------------------------|-----------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **Authentication (JWT)**                      | Implement JSON Web Tokens (JWT) using `djangorestframework-simplejwt` to verify user identity for each request. | Ensures only authenticated users can access protected endpoints and prevents unauthorized access.                     |
| **Authorization (Role-Based Access Control)** | Define user roles (e.g., *admin*, *host*, *guest*) and restrict access to resources accordingly.                | Prevents users from performing actions outside their permission level (e.g., guests editing another host’s property). |
| **HTTPS & SSL Encryption**                    | Enforce HTTPS to encrypt all traffic between client and server.                                                 | Protects sensitive data (like passwords and payment info) from interception or tampering.                             |
| **Rate Limiting & Throttling**                | Configure DRF throttling to limit requests per user/IP.                                                         | Reduces brute-force login attempts and prevents API abuse.                                                            |
| **Input Validation & Sanitization**           | Validate and sanitize all incoming data to avoid malicious payloads.                                            | Prevents injection attacks (SQL, XSS) and ensures data integrity.                                                     |
| **Password Hashing**                          | Use Django’s built-in password hashing (PBKDF2 by default).                                                     | Keeps user passwords secure even if the database is compromised.                                                      |
| **Secure File Uploads**                       | Validate file types and store uploaded media safely (e.g., AWS S3).                                             | Prevents malicious file uploads or code execution.                                                                    |
| **CORS & CSRF Protection**                    | Use `django-cors-headers` and Django’s CSRF middleware.                                                         | Blocks unauthorized cross-origin requests and protects session data.                                                  |
| **Logging & Monitoring**                      | Track authentication attempts, API usage, and unusual activity.                                                 | Helps detect intrusions early and supports audit trails.                                                              |

### Why Security Is Crucial

| **Area**                           | **Why It Matters**                                                                                                                                       |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **User Data Protection**           | Users share personal details (emails, phone numbers, identities). Securing this data builds trust and ensures compliance with privacy laws (e.g., GDPR). |
| **Authentication & Authorization** | Prevents unauthorized access to sensitive actions like bookings, payments, or user details.                                                              |
| **Payments**                       | Ensures safe transaction processing and protects against fraud, chargebacks, or payment data theft.                                                      |
| **Property & Booking Information** | Prevents unauthorized users from accessing or modifying property details, bookings, or host information.                                                 |
| **Platform Integrity**             | Maintains system reliability, reputation, and user confidence by minimizing the risk of data breaches.                                                   |

## ⚙️ CI/CD Pipeline

### What Is a CI/CD Pipeline?

A CI/CD (Continuous Integration and Continuous Deployment) pipeline automates
the process of building, testing, and deploying your application.
It ensures that every code change passes through a consistent workflow before
reaching production — reducing human error, improving reliability, and speeding
up development.

### Why CI/CD Pipeline?

| **Benefit**                   | **Description**                                                                                                   |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Automation & Efficiency**   | Automates repetitive tasks like testing and deployment, allowing developers to focus on writing code.             |
| **Early Bug Detection**       | Runs tests automatically after each commit, helping catch errors early in the development cycle.                  |
| **Consistency & Reliability** | Ensures that all deployments follow the same verified steps, reducing environment inconsistencies.                |
| **Faster Delivery**           | Speeds up feature releases and updates by integrating and deploying continuously.                                 |
| **Improved Collaboration**    | Enables multiple developers to work seamlessly by merging code frequently and automatically testing integrations. |

### Tools for CI/CD Implementation

| **Tool**           | **Purpose**                                                                                                 | **Usage in Project**                                                       |
|--------------------|-------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| **GitHub Actions** | Automate testing, building, and deployment directly from the GitHub repository.                             | Run automated tests and trigger deployments on every push or pull request. |
| **Docker**         | Containerize the application to ensure consistent environments across development, testing, and production. | Build and deploy the Django backend in isolated, reproducible containers.  |
| **Docker Compose** | Manage multi-container setups (e.g., Django app, PostgreSQL, Redis).                                        | Simplifies local and production deployments with one configuration file.   |
| **Redis & Celery** | Handle background jobs, async tasks, and scheduled tasks.                                                   | Used for notifications, emails, and other non-blocking processes.          |
| **PostgreSQL**     | Production-grade database.                                                                                  | Host Dockerized containers and manage production environments.             |
| **AWS**            | Cloud platforms for hosting and deployment.                                                                 | Host Dockerized containers and manage production environments.             |

## 👩🏻‍💻 Installing in Development

1. Clone the repository using ssh or https:
    > **ssh**: `git clone git@github.com:hanatole/airbnb-clone-project.git`

    > **https**: `git clone https://github.com/hanatole/airbnb-clone-project.git`
2. Navigate to the project directory: `cd airbnb-clone-project`
