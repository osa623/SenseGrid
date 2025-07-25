# Sensegrid Project - Complete Setup Guide

## Project Overview
Sensegrid is a full-stack application with Admin and Web interfaces, featuring:
- **Admin Panel**: React + Vite frontend with Node.js backend
- **Web Application**: React frontend with Node.js backend
- **Database**: MongoDB
- **Containerization**: Docker & Docker Compose

## Prerequisites Installation

### 1. Install Node.js
```bash
# Download and install Node.js 18+ from https://nodejs.org/
# Verify installation
node --version
npm --version
```

### 2. Install Docker
```bash
# Download Docker Desktop from https://www.docker.com/products/docker-desktop/
# Verify installation
docker --version
docker-compose --version
```

### 3. Install Git
```bash
# Download Git from https://git-scm.com/
# Verify installation
git --version
```

## Project Initialization

### 1. Clone/Setup Project Directory
```bash
# Create project directory
mkdir Sensegrid
cd Sensegrid

# Initialize git repository
git init
```

### 2. Generate JWT Secret Key
```bash
# Generate a secure JWT secret (save this for environment variables)
node -e "console.log(require('crypto').randomBytes(64).toString('hex'))"
```

### 3. Create Directory Structure
```bash
# Create main directories
mkdir Admin Web

# Create Admin directories
mkdir Admin/frontend Admin/backend

# Create Web directories
mkdir Web/frontend Web/backend
```

## Admin Panel Setup

### 1. Backend Setup
```bash
# Navigate to Admin backend
cd Admin/backend

# Initialize package.json (if not exists)
npm init -y

# Install dependencies
npm install bcryptjs cors dotenv express jsonwebtoken mongoose nodemon

# Create environment file
echo "MONGO_URI=mongodb://admin:password123@localhost:27017/sensgrid?authSource=admin
JWT_SECRET=your_generated_jwt_secret_here
PORT=5000
NODE_ENV=development" > .env
```

### 2. Frontend Setup
```bash
# Navigate to Admin frontend
cd ../frontend

# Install dependencies
npm install

# Install additional dependencies if missing
npm install @emotion/react @emotion/styled @mui/material vite typescript
```

## Web Application Setup

### 1. Backend Setup
```bash
# Navigate to Web backend
cd ../../Web/backend

# Initialize package.json (if not exists)
npm init -y

# Install dependencies
npm install cors dotenv express mongoose morgan multer nodemailer nodemon

# Create environment file
echo "MONGO_URI=mongodb://admin:password123@localhost:27017/sensgrid?authSource=admin
JWT_SECRET=your_generated_jwt_secret_here
PORT=5001
NODE_ENV=development" > .env
```

### 2. Frontend Setup
```bash
# Navigate to Web frontend
cd ../frontend

# Install dependencies
npm install

# Install additional dependencies if missing
npm install react react-dom react-scripts tailwindcss
```

## MongoDB Setup Options

### Option 1: Local MongoDB Installation
```bash
# Install MongoDB Community Server from https://www.mongodb.com/try/download/community

# Start MongoDB service
# Windows:
net start MongoDB

# macOS:
brew services start mongodb/brew/mongodb-community

# Linux:
sudo systemctl start mongod

# Connect to MongoDB and create database
mongo
use sensgrid
db.createUser({
  user: "admin",
  pwd: "password123",
  roles: ["readWrite", "dbAdmin"]
})
```

### Option 2: MongoDB Atlas (Cloud)
```bash
# 1. Go to https://www.mongodb.com/atlas
# 2. Create free account
# 3. Create cluster
# 4. Get connection string
# 5. Update MONGO_URI in .env files with Atlas connection string
```

## Docker Setup

### 1. Build and Run All Services
```bash
# From project root directory
cd e:\Sensegrid

# Build and start all services
docker-compose up --build

# Run in detached mode
docker-compose up -d --build
```

### 2. Individual Service Commands
```bash
# Start only Admin services
cd Admin
docker-compose up --build

# Start only Web services
cd Web
docker-compose up --build

# Stop all services
docker-compose down

# Remove volumes and containers
docker-compose down -v
```

## Development Commands

### Admin Panel Development
```bash
# Backend development
cd Admin/backend
npm run dev

# Frontend development (in new terminal)
cd Admin/frontend
npm run dev
```

### Web Application Development
```bash
# Backend development
cd Web/backend
npm start

# Frontend development (in new terminal)
cd Web/frontend
npm start
```

## Environment Variables Setup

### Admin Backend (.env)
```bash
cd Admin/backend
cat > .env << EOF
MONGO_URI=mongodb://admin:password123@localhost:27017/sensgrid?authSource=admin
JWT_SECRET=$(node -e "console.log(require('crypto').randomBytes(64).toString('hex'))")
PORT=5000
NODE_ENV=development
EOF
```

### Web Backend (.env)
```bash
cd Web/backend
cat > .env << EOF
MONGO_URI=mongodb://admin:password123@localhost:27017/sensgrid?authSource=admin
JWT_SECRET=$(node -e "console.log(require('crypto').randomBytes(64).toString('hex'))")
PORT=5001
NODE_ENV=development
EOF
```


### Complete Setup from Scratch
```bash
# 1. Generate JWT secret
JWT_SECRET=$(node -e "console.log(require('crypto').randomBytes(64).toString('hex'))")
echo "Generated JWT Secret: $JWT_SECRET"

# 2. Setup Admin backend
cd Admin/backend
npm install
echo "MONGO_URI=mongodb://admin:password123@localhost:27017/sensgrid?authSource=admin
JWT_SECRET=$JWT_SECRET
PORT=5000
NODE_ENV=development" > .env

# 3. Setup Web backend
cd ../../Web/backend
npm install
echo "MONGO_URI=mongodb://admin:password123@localhost:27017/sensgrid?authSource=admin
JWT_SECRET=$JWT_SECRET
PORT=5001
NODE_ENV=development" > .env

# 4. Setup frontends
cd ../frontend && npm install
cd ../../Admin/frontend && npm install

# 5. Start with Docker
cd ../../
docker-compose up --build
```

### Development Mode
```bash
# Terminal 1: Admin Backend
cd Admin/backend && npm run dev

# Terminal 2: Admin Frontend
cd Admin/frontend && npm run dev

# Terminal 3: Web Backend
cd Web/backend && npm start

# Terminal 4: Web Frontend
cd Web/frontend && npm start

# Terminal 5: MongoDB (if local)
mongod
```

## Important Notes

1. **JWT Secret**: Generate a new secret for production
2. **Database**: Use MongoDB Atlas for production
3. **Ports**: Ensure no port conflicts
4. **Environment**: Set NODE_ENV=production for production builds
5. **Security**: Never commit .env files to version control

## Next Steps

1. Configure your IDE with ESLint and Prettier
2. Set up CI/CD pipeline
3. Configure production environment variables
4. Set up monitoring and logging
5. Implement backup strategies