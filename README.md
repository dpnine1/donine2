# Project Documentation

## Overview
The Clock and PM2.5 Application is designed to monitor air quality and accurately display the current time. This application provides users with real-time information about PM2.5 levels in the air, helping them make informed decisions about their health.

## Features
- **Real-time clock**: Displays the current time in a clear and user-friendly format.
- **Air Quality Monitoring**: Shows real-time PM2.5 levels, allowing users to be aware of air quality conditions.
- **Alerts**: Users can set alerts based on PM2.5 levels to receive notifications when air quality drops below safe levels.

## Technology Stack
- **Frontend**: HTML, CSS, JavaScript
- **Backend**: Node.js, Express
- **Database**: MongoDB
- **API**: RESTful API for fetching PM2.5 data

## How to Use
1. **Installation**: Clone the repository and install the necessary dependencies using `npm install`.
2. **Run the Application**: Start the server using `npm start`. The application will be available on `http://localhost:3000`.
3. **Interact**: Use the interface to view the current time and PM2.5 levels.

## API Information
The application utilizes a RESTful API to fetch PM2.5 data. Below are the key endpoints:
- `GET /api/pm25`: Fetch the current PM2.5 levels.
- `GET /api/time`: Fetch the current UTC time.

## How to Contribute
We welcome contributions! Please follow these steps:
1. Fork the repository.
2. Create a new branch for your feature: `git checkout -b feature/your-feature-name`.
3. Commit your changes: `git commit -m 'Add your feature description'`.
4. Push to the branch: `git push origin feature/your-feature-name`.
5. Create a pull request.

Thank you for your interest in contributing to the Clock and PM2.5 Application!