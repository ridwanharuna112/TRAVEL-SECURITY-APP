# Travel Security App - Backend

A comprehensive REST API for managing road safety, security threats, and travel information across Nigeria.

## Features

- 🗺️ **Interactive Map** - View all roads and locations across Nigeria
- 🚗 **Road Information** - Distance, condition, travel time, and toll gates
- ⚠️ **Security Threats** - Real-time reporting and verification of security incidents
- 👥 **User Management** - Travelers and locals with personalized experience
- 📍 **Location Services** - Find cities, towns, and landmarks with safety status
- 📊 **Journey Tracking** - Log and analyze your travel history
- 🔐 **JWT Authentication** - Secure user authentication and authorization

## Tech Stack

- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MongoDB
- **Real-time**: Socket.io
- **Authentication**: JWT
- **Validation**: Express-validator

## Installation

1. Clone the repository
```bash
git clone https://github.com/ridwanharuna112/TRAVEL-SECURITY-APP.git
cd TRAVEL-SECURITY-APP
```

2. Install dependencies
```bash
npm install
```

3. Create `.env` file (copy from `.env.example`)
```bash
cp .env.example .env
```

4. Update environment variables with your configurations

5. Start the server
```bash
npm run dev
```

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user
- `GET /api/auth/me` - Get current user

### Roads
- `GET /api/roads` - Get all roads
- `GET /api/roads/:id` - Get road details
- `POST /api/roads` - Create road (admin)
- `PUT /api/roads/:id` - Update road (admin)

### Security Threats
- `GET /api/threats` - Get all threats
- `GET /api/threats/:id` - Get threat details
- `POST /api/threats` - Report threat
- `PUT /api/threats/:id/verify` - Verify threat (admin)

### Locations
- `GET /api/locations` - Get all locations
- `GET /api/locations/:id` - Get location details

### Map
- `GET /api/map/state/:state` - Get state map data
- `GET /api/map/states` - Get all Nigerian states

## License

MIT
