# Cyber Event Manager - Backend

Express.js REST API server for managing cybersecurity events and user authentication with PostgreSQL database integration.

## Features

- RESTful API for event management
- User authentication with role-based access
- PostgreSQL database integration
- CORS enabled for frontend communication
- Environment variable configuration

## Tech Stack

- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: PostgreSQL
- **ORM/Client**: pg (node-postgres)
- **Environment**: dotenv
- **Middleware**: CORS

## Project Structure

```
BackEnd/
├── .env                    # Environment variables
├── .gitignore             # Git ignore rules
├── server.js              # Main server file
├── db.js                  # Database connection
├── package.json           # Dependencies & scripts
└── routes/
    ├── auth.js            # Authentication routes
    └── users.js           # Event management routes
```

## Setup & Installation

### Prerequisites
- Node.js (v14 or higher)
- PostgreSQL database
- npm or yarn package manager

### Installation

```bash
# Clone the repository
git clone <repository-url>
cd BackEnd

# Install dependencies
npm install

# Configure environment variables
cp .env.example .env
# Edit .env with your database credentials

# Start the server
npm start

# For development (with auto-reload)
npm run dev
```

## Environment Variables

Create a .env file in the root directory:

```env
DATABASE_URL=postgresql://username:password@localhost:5432/SpecialTopics
PORT=3001
vite_server_url=http://localhost:5000
```

## Database Setup

### Create Database and Tables

```sql
-- Connect to PostgreSQL
psql -U postgres

-- Create database
CREATE DATABASE SpecialTopics;

-- Connect to the database
\c SpecialTopics

-- Create events table
CREATE TABLE events (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    event_date DATE NOT NULL,
    event_time TIME NOT NULL,
    price NUMERIC(10, 2) NOT NULL
);

-- Create users table
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    role VARCHAR(20) DEFAULT 'user' CHECK (role IN ('admin', 'user'))
);

-- Insert sample events
INSERT INTO events (name, event_date, event_time, price) VALUES 
('Cybersecurity Conference 2024', '2024-07-15', '09:00:00', 3500.00),
('Ethical Hacking Workshop', '2024-07-20', '10:00:00', 2800.00),
('Digital Forensics Seminar', '2024-07-25', '14:00:00', 4200.00);

-- Insert default admin user
INSERT INTO users (name, email, password, role) VALUES 
('Admin User', 'admin@example.com', 'admin123', 'admin');
```

## API Endpoints

**Base URL**: `http://localhost:3001`

### Events Management

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| GET | `/api/events` | Get all events | Public |
| GET | `/api/events/:id` | Get event by ID | Public |
| POST | `/api/events` | Create new event | Admin |
| PUT | `/api/events/:id` | Update event | Admin |
| DELETE | `/api/events/:id` | Delete event | Admin |

### Authentication

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| POST | `/api/auth/login` | User login | Public |
| POST | `/api/auth/signup` | User registration | Public |

### Utility Routes

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Home route |
| GET | `/test` | Test route |

## Request/Response Examples

### GET /api/events
```json
[
  {
    "id": 1,
    "name": "Cybersecurity Conference 2024",
    "event_date": "2024-07-15",
    "event_time": "09:00:00",
    "price": "3500.00"
  }
]
```

### POST /api/events
```json
// Request
{
  "name": "New Security Workshop",
  "event_date": "2024-08-01",
  "event_time": "14:30",
  "price": 2500.00
}

// Response
{
  "id": 4,
  "name": "New Security Workshop",
  "event_date": "2024-08-01",
  "event_time": "14:30:00",
  "price": "2500.00"
}
```

### POST /api/auth/login
```json
{
  "email": "admin@example.com",
  "password": "admin123"
}

// Response
{
  "message": "Login successful",
  "user": {
    "id": 1,
    "name": "Admin User",
    "email": "admin@example.com",
    "role": "admin"
  }
}
```

### POST /api/auth/signup
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123",
  "role": "user"
}

{
  "message": "User created successfully",
  "user": {
    "id": 2,
    "name": "John Doe",
    "email": "john@example.com",
    "role": "user"
  }
}
```

## Scripts

```json
{
  "start": "node server.js",      // Production start
  "dev": "nodemon server.js",     // Development with auto-reload
  "test": "echo \"Error: no test specified\" && exit 1"
}
```

## Dependencies

### Production
- **express**: Web framework
- **pg**: PostgreSQL client
- **cors**: Cross-origin resource sharing
- **dotenv**: Environment variable loader

### Development
- **nodemon**: Auto-restart server on changes

## Database Schema

### Events Table
```sql
Column     | Type                        | Constraints
-----------|-----------------------------|------------
id         | SERIAL                      | PRIMARY KEY
name       | VARCHAR(255)                | NOT NULL
event_date | DATE                        | NOT NULL
event_time | TIME WITHOUT TIME ZONE      | NOT NULL
price      | NUMERIC(10,2)               | NOT NULL
```

### Users Table
```sql
Column   | Type         | Constraints
---------|--------------|------------------
id       | SERIAL       | PRIMARY KEY
name     | VARCHAR(255) | NOT NULL
email    | VARCHAR(255) | UNIQUE, NOT NULL
password | VARCHAR(255) | NOT NULL
role     | VARCHAR(20)  | DEFAULT 'user', CHECK (role IN ('admin', 'user'))
```

## Error Handling

The API returns standardized error responses:

```json
{
  "error": "Error message description"
}
```

**Common HTTP Status Codes:**
- `200`: Success
- `201`: Created
- `400`: Bad Request
- `401`: Unauthorized
- `404`: Not Found
- `500`: Internal Server Error


```bash
# Start development server with auto-reload
npm run dev

# Check database connection
npm start
# Look for "Listening on 3001" message
```

## Deployment

1. Set up PostgreSQL database on your hosting service
2. Configure `DATABASE_URL` environment variable
3. Deploy the application
4. Run database migrations if needed

