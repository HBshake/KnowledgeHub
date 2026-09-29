# KnowledgeHub
## Enterprise Cloud & Mobile Application Architecture

A platform for educators to share learning resources and students to discover, subscribe, and contribute to curated educational content.

---

## General Information

KnowledgeHub is an enterprise-class web application that demonstrates advanced concepts in cloud computing, microservices architecture, scalability, and security. The application connects educators (professors, tutors, content creators) with students seeking free, quality learning materials. It showcases real-world enterprise patterns including role-based access control, external system integrations, cloud deployment, containerization, and microservices-ready architecture.

---

## Part 1: Requirement Areas, Principles, and Architectural Models

### Actors & Roles

The system has four primary actors:

1. **Student** - Discovers and consumes learning resources, subscribes to educators/topics, participates in community
2. **Educator** - Creates collections, uploads resources, manages contributions, views analytics, can upgrade to premium
3. **Admin** - Manages users, moderates content, views system analytics, manages system configuration
4. **System** - External services (Stripe, SendGrid, AWS S3, Elasticsearch)

### Functional Requirements

#### Student Functionalities

- A student shall be able to register an account with email and password
- A student shall be able to login securely with JWT authentication
- A student shall be able to browse all public collections
- A student shall be able to search collections by name and filter by topic
- A student shall be able to view collection details including educator info and resources
- A student shall be able to follow individual educators
- A student shall be able to subscribe to topics (e.g., "Mathematics", "Computer Science")
- A student shall be able to see a personalized feed of new resources from followed educators and subscribed topics
- A student shall be able to save/favorite collections
- A student shall be able to comment on resources
- A student shall be able to reply to comments (nested replies)
- A student shall be able to rate resources (1-5 stars)
- A student shall be able to upload notes/resources to collections (subject to educator approval)
- A student shall be able to view approval status of their contributions
- A student shall be able to track how many times their contributions were downloaded
- A student shall receive notifications when new resources are published in subscribed topics

#### Educator Functionalities

- An educator shall be able to register as educator role
- An educator shall be able to create new collections (course modules)
- An educator shall be able to edit collection details and metadata
- An educator shall be able to upload resources (PDFs, documents, images, videos) to collections
- An educator shall be able to add title and description to resources
- An educator shall be able to view all their collections and resources
- An educator shall be able to delete collections and resources
- An educator shall be able to organize collections by topic
- An educator shall receive notifications of new student contributions
- An educator shall be able to review, approve, or reject student contributions
- An educator shall be able to mark contributions as "featured"
- An educator shall view analytics dashboard with key stats (subscribers, downloads, ratings)
- An educator shall view detailed analytics per collection and per resource
- An educator shall view growth trends over time
- An educator shall be able to export analytics as CSV
- An educator shall be able to upgrade to premium tier via Stripe payment
- An educator shall access advanced analytics if premium

#### Admin Functionalities

- A default admin account shall be created automatically
- An admin shall be able to create other admin accounts and moderator accounts
- An admin shall be able to view all users (students, educators, admins)
- An admin shall be able to deactivate users
- An admin shall be able to view flagged content
- An admin shall be able to approve or reject flagged content
- An admin shall be able to delete inappropriate resources or comments
- An admin shall view system-wide analytics (total users, active users, growth rate)
- An admin shall view platform statistics (total collections, total resources, downloads)
- An admin shall view most popular topics, educators, and resources
- An admin shall be able to manage system settings

#### External System Integrations

- The system shall integrate with **Stripe** for educator premium upgrades and payment processing
- The system shall integrate with **SendGrid** for email notifications (welcome, password reset, new resources, confirmations)
- The system shall integrate with **AWS S3** for secure file storage and downloads
- The system shall integrate with **Elasticsearch** (optional Phase 2) for advanced full-text search

---

## Part 2: The Monolithic Server Side of EAs

### Non-Functional Requirements

#### Performance
- The system shall load main pages within 3 seconds
- Search results shall return within 1 second
- API endpoints shall respond within 500ms for 95% of requests
- The system shall pass Google Lighthouse performance test with 80%+

#### Scalability
- The system shall scale horizontally by adding more server instances
- Database shall handle 100k+ records efficiently with proper indexing
- The system shall handle 1000 concurrent users without significant degradation
- File uploads shall be non-blocking (background jobs)

#### Security
- All passwords shall be hashed with bcrypt
- Authentication shall use JWT tokens with refresh tokens
- All data traffic shall be encrypted (HTTPS)
- Users shall only access their own data (authorization checks)
- Admin actions shall be logged for audit trail
- Rate limiting shall prevent brute force attacks on login

#### Availability & Reliability
- The system shall have 99.9% uptime
- No single point of failure (SPoF) - redundant systems
- Automated daily backups of database
- Graceful handling of external service failures

#### Usability
- The system shall be responsive on all devices (mobile, tablet, desktop)
- The system shall work as Progressive Web App (offline access)
- UI/UX shall follow accessibility standards

---

## Part 3: The Progressive Web/Mobile Client Side of EAs

### Architecture

The backend follows a microservices-ready monolithic architecture:

**Core Services:**
- Auth Service (login, register, JWT management)
- Educator Service (collection & resource management, analytics)
- Student Service (subscriptions, feed, contributions)
- Content Service (comments, ratings, search)
- Notification Service (email, in-app notifications)
- Payment Service (Stripe integration)
- Admin Service (user management, moderation, system analytics)

**Data Layer:**
- PostgreSQL (primary database)
- Redis (caching for performance)

**Storage Layer:**
- AWS S3 (file uploads)

**External Integrations:**
- Stripe (payments)
- SendGrid (emails)
- Elasticsearch (search)

### Physical Architecture - Initial
