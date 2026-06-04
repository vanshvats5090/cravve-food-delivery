# 🍽️ Cravve — Food Delivery Platform

A full-stack food delivery platform inspired by **Zomato/Swiggy**, built with the **MERN stack** and a **microservice-based backend architecture**. Cravve supports multiple user roles — customers, restaurant owners, delivery riders, and admins — each with dedicated dashboards and real-time capabilities.

---

## 🚀 Live Features

- 🔐 **Secure Authentication** — Google OAuth + JWT-based auth
- 🏪 **Restaurant Management** — Owners can manage menus, view and accept orders
- 🛒 **Customer Experience** — Browse restaurants, add to cart, checkout, pay
- 🏍️ **Rider Dashboard** — Real-time order assignment, live map navigation
- 📍 **Live Location Tracking** — Leaflet maps with road-based routing (similar to Zomato/Swiggy)
- 🔔 **Socket Notifications** — Real-time events via WebSockets (Socket.io)
- 💳 **Payment Integration** — Razorpay payment gateway
- 🐇 **Message Queuing** — RabbitMQ for async inter-service communication
- 🐳 **Dockerized Services** — Each microservice ships with its own Dockerfile

---

## 🏗️ Architecture Overview

```
cravve/
├── frontend/               # React + TypeScript (Vite) — Single Page App
└── services/
    ├── auth/               # Authentication service (Google OAuth, JWT)
    ├── restaurant/         # Core service (restaurants, menus, cart, orders)
    ├── rider/              # Rider service (assignment, location, delivery)
    ├── utils/              # Utility service (Razorpay payments, Cloudinary)
    ├── realtime/           # WebSocket service (Socket.io notifications)
    └── admin/              # Admin service (user & rider management)
```

### Inter-service Communication

```
[restaurant service] ──RabbitMQ──▶ [utils service]    (payment events)
[restaurant service] ──RabbitMQ──▶ [rider service]    (order ready events)
[rider service]      ──HTTP──────▶ [realtime service] (socket notifications)
[restaurant service] ──HTTP──────▶ [realtime service] (socket notifications)
```

---

## 🛠️ Tech Stack

### Frontend
| Technology | Purpose |
|---|---|
| React 19 + TypeScript | UI framework |
| Vite | Build tool |
| Tailwind CSS v4 | Styling |
| React Router v7 | Client-side routing |
| Socket.io Client | Real-time updates |
| Leaflet + React-Leaflet | Interactive maps |
| Leaflet Routing Machine | Road-based route rendering |
| Axios | HTTP client |
| @react-oauth/google | Google Sign-In |
| React Hot Toast | Notifications |

### Backend (per service)
| Technology | Purpose |
|---|---|
| Node.js + Express 5 | HTTP server |
| TypeScript | Type safety |
| MongoDB + Mongoose | Database |
| JSON Web Tokens (JWT) | Auth tokens |
| RabbitMQ (amqplib) | Message queue |
| Socket.io | WebSocket server |
| Razorpay | Payment gateway |
| Cloudinary | Image uploads |
| Google APIs | OAuth |
| Multer + Datauri | File handling |
| Docker | Containerization |

---

## 📁 Project Structure

```
cravve/
│
├── frontend/
│   ├── src/
│   │   ├── components/         # Reusable UI components
│   │   │   ├── navbar.tsx
│   │   │   ├── OrderCard.tsx
│   │   │   ├── RiderOrderMap.tsx
│   │   │   ├── UserOrderMap.tsx
│   │   │   ├── RestaurantOrders.tsx
│   │   │   └── ...
│   │   ├── pages/              # Role-based page views
│   │   │   ├── Home.tsx
│   │   │   ├── RiderDashboard.tsx
│   │   │   ├── Admin.tsx
│   │   │   ├── Checkout.tsx
│   │   │   ├── OrderPage.tsx
│   │   │   └── ...
│   │   ├── context/
│   │   │   ├── AppContext.tsx   # Global state (user, auth)
│   │   │   └── SocketContext.tsx
│   │   ├── utils/
│   │   │   └── orderflow.ts
│   │   └── types.ts
│   ├── vercel.json             # Vercel deployment config
│   └── vite.config.ts
│
└── services/
    ├── auth/
    │   └── src/
    │       ├── controllers/auth.ts
    │       ├── model/User.ts
    │       ├── routes/auth.ts
    │       └── config/googleConfig.ts
    │
    ├── restaurant/
    │   └── src/
    │       ├── controllers/
    │       │   ├── order.ts
    │       │   ├── restaraunt.ts
    │       │   ├── menuitem.ts
    │       │   ├── cart.ts
    │       │   └── address.ts
    │       ├── models/
    │       │   ├── Order.ts
    │       │   ├── Restaurant.ts
    │       │   ├── MenuItems.ts
    │       │   ├── Cart.ts
    │       │   └── Address.ts
    │       └── config/
    │           ├── rabbitmq.ts
    │           ├── order.publisher.ts
    │           └── payment.consumer.ts
    │
    ├── rider/
    │   └── src/
    │       ├── controllers/rider.ts
    │       ├── model/Rider.ts
    │       └── config/
    │           ├── rabbitmq.ts
    │           └── orderReady.consumer.ts
    │
    ├── realtime/
    │   └── src/
    │       ├── socket.ts
    │       └── routes/internal.ts
    │
    ├── utils/
    │   └── src/
    │       ├── controllers/payment.ts
    │       └── config/
    │           ├── razorpay.ts
    │           ├── rabbitmq.ts
    │           └── payment.producer.ts
    │
    └── admin/
        └── src/
            ├── controllers/admin.ts
            └── routes/admin.ts
```

---

## ⚙️ Environment Variables

Each service needs its own `.env` file. Below are the required variables:

### `services/auth/.env`
```env
PORT=
MONGO_URI=
JWT_SEC=
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
```

### `services/restaurant/.env`
```env
PORT=
MONGO_URI=
JWT_SEC=
RABBITMQ_URL=
PAYMENT_QUEUE=
RIDER_QUEUE=
REALTIME_SERVICE=
INTERNAL_SERVICE_KEY=
CLOUDINARY_CLOUD_NAME=
CLOUDINARY_API_KEY=
CLOUDINARY_API_SECRET=
```

### `services/rider/.env`
```env
PORT=
MONGO_URI=
JWT_SEC=
RABBITMQ_URL=
ORDER_READY_QUEUE=
REALTIME_SERVICE=
INTERNAL_SERVICE_KEY=
```

### `services/utils/.env`
```env
PORT=
RAZORPAY_KEY_ID=
RAZORPAY_KEY_SECRET=
RABBITMQ_URL=
PAYMENT_QUEUE=
```

### `services/realtime/.env`
```env
PORT=
JWT_SEC=
INTERNAL_SERVICE_KEY=
```

### `services/admin/.env`
```env
PORT=
MONGO_URI=
JWT_SEC=
```

### `frontend/.env`
```env
VITE_AUTH_SERVICE_URL=
VITE_RESTAURANT_SERVICE_URL=
VITE_RIDER_SERVICE_URL=
VITE_UTILS_SERVICE_URL=
VITE_REALTIME_SERVICE_URL=
VITE_GOOGLE_CLIENT_ID=
VITE_RAZORPAY_KEY_ID=
```

---

## 🚀 Getting Started

### Prerequisites
- Node.js v18+
- MongoDB (local or Atlas)
- RabbitMQ (local or CloudAMQP)
- Accounts: Razorpay, Cloudinary, Google Cloud (OAuth)

### Running Each Service Locally

Each service follows the same pattern:

```bash
cd services/<service-name>
npm install
cp .env.example .env    # fill in your values
npm run dev
```

Default ports (suggested):
| Service | Port |
|---|---|
| auth | 3001 |
| restaurant | 3002 |
| rider | 3003 |
| utils | 3004 |
| realtime | 3005 |
| admin | 3006 |

### Running the Frontend

```bash
cd frontend
npm install
cp .env.example .env    # fill in your service URLs
npm run dev
```

---

## 🐳 Docker

Each service includes a `Dockerfile` and `.dockerignore`. To build and run a service:

```bash
cd services/auth
docker build -t cravve-auth .
docker run -p 3001:3001 --env-file .env cravve-auth
```

---

## 🔄 Real-time Order Flow

```
Customer places order
        │
        ▼
[restaurant service] publishes ORDER_READY_FOR_RIDER → RabbitMQ
        │
        ▼
[rider service] consumes message, finds nearby available riders via geospatial query
        │
        ▼
[rider service] → HTTP POST → [realtime service] (emit "order:available" socket event)
        │
        ▼
Nearby riders receive notification in real time via WebSocket
        │
        ▼
Rider accepts → location polling begins → customer sees live map tracking
```

---

## 👥 User Roles

| Role | Capabilities |
|---|---|
| **Customer** | Browse restaurants, add to cart, checkout, pay, track order live on map |
| **Restaurant Owner** | Manage menu items, view incoming orders, update order status |
| **Rider** | View assigned orders, navigate via map, update delivery status |
| **Admin** | Manage users, verify riders, manage restaurants |

---

## 🗺️ Maps & Tracking

- **Library**: Leaflet + React-Leaflet + Leaflet Routing Machine
- **Rider view**: Shows route from current location to restaurant and then to customer
- **Customer view**: Live rider location updates via periodic polling
- **Geolocation**: MongoDB `$near` geospatial queries to match riders to orders within 500m

---

## 📦 Key Dependencies

### Backend (all services)
```
express, mongoose, jsonwebtoken, dotenv, cors, typescript, concurrently
```

### Restaurant / Rider specific
```
amqplib (RabbitMQ), multer, datauri, axios
```

### Utils service
```
razorpay
```

### Auth service
```
googleapis
```

### Realtime service
```
socket.io, jsonwebtoken
```

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m 'Add some feature'`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a Pull Request

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

> Built with ❤️ using the MERN stack and microservice architecture — because every craving deserves to be answered.
