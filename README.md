# Feeders — Food Donation Web Application

Feeders is a full-stack web application designed to help connect food donors—including individuals and restaurants—with NGOs and organizations that can coordinate food collection and distribution. The project aims to support food redistribution, reduce avoidable food waste, and make donation-related workflows easier to manage.

> **Project note:** This README describes the features and technologies visible in the source code. Availability of individual workflows may depend on configuration and backend/database setup.

## Table of Contents

- [Project Overview](#project-overview)
- [Objectives](#objectives)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [Running the Application](#running-the-application)
- [API Overview](#api-overview)
- [Security and Configuration Notes](#security-and-configuration-notes)
- [Future Improvements](#future-improvements)
- [Contributing](#contributing)
- [License](#license)

## Project Overview

Food surplus can often go unused while community organizations need support in obtaining food. Feeders provides a web-based platform concept for coordinating donations and related collection activities between donors and organizations.

The application contains a React frontend and a Node.js/Express backend. The backend uses MongoDB through Mongoose and exposes REST-style API routes for authentication, individual users, organizations, restaurant-related workflows, food donations, waste-food collection, vendor donations, and contact requests.

## Objectives

- Provide a common platform for food donation-related activities.
- Help individuals and restaurants submit or manage food donations.
- Support NGO/organization workflows for viewing and coordinating donations.
- Provide forms and dashboards for donation and volunteer-related activities.
- Use web technology to support food redistribution and awareness of food waste.

## Features

The source code includes pages, components, routes, controllers, or models for the following areas:

- **User access:** registration, login, authentication context, and authenticated API requests.
- **Role-oriented experiences:** pages and dashboard components for individual users, organizations/NGOs, and restaurants.
- **Donation workflows:** food donation pages, donation requests, vendor food donations, and donation-related management components.
- **Organization management:** organization dashboard, profile, donor, donation, and volunteer-related components.
- **Food collection:** a waste-food collection form and corresponding backend route/controller/model.
- **Volunteer workflows:** volunteer form and volunteer application-related components.
- **Contact and information pages:** home, About Us, How It Works, Contact Us, and related interface components.
- **Location support:** a geolocation component and Leaflet/React Leaflet dependencies for map-related functionality.
- **Backend API:** Express routes, controllers, Mongoose models, request middleware, and centralized error handling.

Feature availability can depend on database contents, environment configuration, and the completion of each workflow. Do not assume every feature is production-ready without testing it.

## Technology Stack

### Frontend

- React
- Vite
- React Router
- Redux Toolkit and React Redux
- Axios
- Tailwind CSS
- Leaflet and React Leaflet
- React Icons and Lucide React
- React Hook Form and Yup
- React Toastify and React Hot Toast

### Backend

- Node.js
- Express.js
- MongoDB
- Mongoose
- JSON Web Tokens (JWT)
- bcryptjs
- dotenv
- CORS
- express-validator and Joi
- Nodemailer
- express-rate-limit

## Project Structure

```text
food-donation/
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── app/
│   │   ├── components/
│   │   ├── context/
│   │   ├── pages/
│   │   ├── service/
│   │   └── utils/
│   ├── package.json
│   └── vite.config.js
│
├── backend/
│   ├── config/
│   ├── controllers/
│   ├── middleware/
│   ├── models/
│   ├── routes/
│   ├── services/
│   ├── utils/
│   ├── package.json
│   └── server.js
│
└── README.md
```

## Prerequisites

Install the following before running the project:

- [Node.js](https://nodejs.org/) (an active LTS version is recommended)
- npm (included with Node.js)
- A MongoDB instance, either local or hosted
- Git (optional, for cloning and version control)

## Getting Started

### 1. Clone the repository

Replace `YOUR-USERNAME` with the GitHub account that owns the repository.

```bash
git clone https://github.com/YOUR-USERNAME/food-donation.git
cd food-donation
```

If your GitHub repository uses a different name, use its actual clone URL and folder name.

### 2. Install backend dependencies

```bash
cd backend
npm install
```

### 3. Configure the backend environment

Create a local environment file in the `backend` folder. Use the variable names listed in the [Environment Variables](#environment-variables) section. Do not commit this file.

### 4. Install frontend dependencies

Open another terminal from the project root, then run:

```bash
cd frontend
npm install
```

### 5. Configure the frontend environment

Create a local `.env` file in the `frontend` folder and set `VITE_API_BASE_URL` to the base URL of the backend API. For local development, the expected API base URL is commonly:

```env
VITE_API_BASE_URL=http://localhost:3000/api/v1
```

The frontend environment variable must match the address and port where the backend is running.

## Environment Variables

### Backend

Create `backend/.env` locally with the required values:

```env
PORT=3000
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=replace_with_a_long_random_secret
FRONTEND_URL=http://localhost:5173
EMAIL_USERNAME=your_email_username
EMAIL_PASSWORD=your_email_or_provider_app_password
EMAIL_FROM=your_sender_email
EMAIL_SERVICE=your_email_service
ADMIN_EMAIL=your_admin_email
```

These are variable names and placeholders, not working credentials. Set only the values required by the features you use. Email-related values are needed only for workflows that send email.

**Configuration note:** The current backend code calls `dotenv.config({ path: "./config.env" })` in `server.js`, while the database module also calls `dotenv.config()`. To use `backend/.env` consistently, check the dotenv loading path in `backend/server.js` and change it to `dotenv.config()` (or consistently use a local `config.env` file). Do not maintain real secrets in source control.

### Frontend

Create `frontend/.env`:

```env
VITE_API_BASE_URL=http://localhost:3000/api/v1
```

Vite exposes variables prefixed with `VITE_` to frontend code. Never put secrets or private credentials in frontend environment variables.

After changing environment variables, restart the relevant development server.

## Running the Application

Run the backend and frontend in **separate terminals**.

### Terminal 1 — Backend

```bash
cd backend
npm run dev
```

The backend's `dev` script uses Nodemon. To run without Nodemon:

```bash
npm start
```

The server code defaults to port `3000` unless `PORT` is configured. A successful startup also depends on a reachable MongoDB instance and valid environment settings.

### Terminal 2 — Frontend

```bash
cd frontend
npm run dev
```

Vite will print a local development URL in the terminal, typically `http://localhost:5173`. Open the URL shown in your terminal.

### Optional frontend checks

From the `frontend` directory:

```bash
npm run lint
npm run build
npm run preview
```

`npm run build` creates a production build. `npm run preview` serves that build locally for previewing; it is not a production hosting setup by itself.

## API Overview

The Express server registers the following API route prefixes:

| Area | Prefix |
|---|---|
| Authentication | `/api/v1/auth` |
| Individual users | `/api/v1/individual` |
| Organizations | `/api/v1/org` |
| Food donation dashboard | `/api/v1/dashboard/foodDonation` |
| Waste-food collection | `/api/v1/dashboard/wastFoodCollection` |
| Vendor food donation | `/api/v1/dashboard/vendorFoodDonation` |
| Contact | `/api/v1/dashboard/contact` |
| Restaurant | `/api/v1/restaurant` |

The backend also defines a simple health/test endpoint at `/success`, which responds with a welcome message.

These prefixes are taken from the route registrations in `backend/server.js`. For exact HTTP methods, request bodies, and individual endpoint paths, refer to the corresponding files in `backend/routes/`.

## Security and Configuration Notes

- Never upload `.env`, `config.env`, API keys, database credentials, email passwords, JWT secrets, or real user data.
- Keep `node_modules/` out of Git; dependencies can be installed with `npm install`.
- Use a strong, unique JWT secret for local testing and deployment.
- Configure MongoDB access permissions appropriately and avoid exposing database credentials.
- Restrict CORS to the intended frontend origin when deploying.
- Validate authentication, authorization, input handling, and error responses before using the application with real users.
- This project should be tested and reviewed before being treated as a production service.

## Future Improvements

Possible next steps for the project include:

- Add screenshots or a short demo video to the repository.
- Document each API endpoint, including example requests and responses.
- Add automated frontend and backend tests.
- Improve setup validation and provide a sanitized `.env.example`.
- Add deployment instructions and configure production environment variables.
- Review role-based authorization and security behavior across all donation workflows.
- Add accessibility checks and responsive-layout testing.

## Contributing

Contributions and suggestions are welcome. For changes:

1. Fork the repository.
2. Create a branch for your change.
3. Make and test your changes.
4. Open a pull request describing the change.

## License

No license is currently specified in the project files. Add a license only after deciding how you want others to use, modify, and distribute this project.
