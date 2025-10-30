# 🍕 Food Delivery Web – MongoDB Data Model

## 📋 Project Overview
This project defines the **MongoDB database schema** for a web application that manages **food delivery and in-store pickup orders**.  
The model covers **customers, orders, products, stores, employees, and pizza categories**.

All validation schemas are stored in the DB_Collections folder and are automatically executed when the MongoDB Docker container starts for the first time.

---

## 🧩 MongoDB Collections

### 👤 `customer`
**File:** [`customer.json`](DB_Collections/customer.json)

| Field | Type | Description |
|--------|------|-------------|
| `name` | string | Customer's first name |
| `surname` | string | Customer's last name |
| `address` | object | Includes `street`, `number`, `floor` (optional), `city`, `postalCode`, `province` |
| `telephone` | string | Contact phone number |
| `registrationDate` | date | Date when the customer was registered |

---

### 🏬 `store`
**File:** [`store.json`](DB_Collections/store.json)

| Field | Type | Description |
|--------|------|-------------|
| `address` | string | Store address |
| `postalCode` | string | ZIP or postal code |
| `city` | string | City |
| `province` | string | Province or region |
| `phone` | string / null | Optional contact phone number |
| `createdAt`, `updatedAt` | date / null | Record creation and update timestamps |

---

### 💼 `employee`
**File:** [`employee.json`](DB_Collections/employee.json)

| Field | Type | Description |
|--------|------|-------------|
| `firstName` | string | Employee’s first name |
| `lastName` | string | Employee’s last name |
| `nif` | string | Unique identifier (NIF or ID number) |
| `phone` | string | Employee’s phone number |
| `role` | string (`cook`, `courier`) | Job role: cook or courier |
| `storeId` | objectId | Reference to the store where the employee works |
| `createdAt`, `updatedAt` | date / null | Record creation and update timestamps |

---

### 🍔 `product`
**File:** [`product.json`](DB_Collections/product.json)

| Field | Type | Description |
|--------|------|-------------|
| `name` | string | Product name |
| `description` | string | Product description |
| `imageUrl` | string (URL) | Product image URL |
| `price` | double | Current price |
| `type` | string (`pizza`, `burger`, `drink`) | Product type |
| `categoryIds` | array / null | List of category IDs (only for pizzas) |
| `createdAt` | date / null | Date when the product was created |

---

### 🍕 `product_category`
**File:** [`productCategory.json`](DB_Collections/product_category.json)

| Field | Type | Description |
|--------|------|-------------|
| `kind` | string (`pizza`) | Type of product this category belongs to |
| `currentName` | string | Current name of the category (may change over the year) |

---

### 🧾 `order`
**File:** [`order.json`](DB_Collections/order.json)

| Field | Type | Description |
|--------|------|-------------|
| `customerId` | objectId | Reference to the customer who placed the order |
| `storeId` | objectId | Reference to the store managing the order |
| `placedAt` | date | Date and time when the order was placed |
| `mode` | string (`delivery`, `pickup`) | Whether the order is for delivery or pickup |
| `items` | array | Ordered items, each containing `productId`, `productName`, `unitPrice`, `quantity`, `subtotal` |
| `typeCounts` | object | Product counts by type: `pizzas`, `burgers`, `drinks` |
| `total` | double | Total price of the order |
| `delivery` | object / null | Delivery info (`courierId`, `deliveredAt`) for delivery orders |

---

## 🗂️ Main Relationships

- **Customer 1 ⟶ n Orders**  
- **Store 1 ⟶ n Orders**  
- **Store 1 ⟶ n Employees**  
- **Order 1 ⟶ n Products**  
- **Pizza 1 ⟶ n Categories**  
- **Courier 1 ⟶ n Deliveries**  

---

## 🗺️ Database Diagrams

All entity-relationship diagrams are:

| Diagram | Description |
|----------|-------------|
| ![Customer](src/Collections_diagrams/customer.png) | Customer schema |
| ![Store](src/Collections_diagrams/store.png) | Store schema |
| ![Employee](src/Collections_diagrams/employee.png) | Employee schema |
| ![Product](src/Collections_diagrams/product.png) | Product schema |
| ![Product Category](src/Collections_diagrams/productCategory.png) | Pizza category schema |
| ![Order](src/Collections_diagrams/order.png) | Order schema |

---

## 🐳 Running with Docker

### 1️⃣. Requirements
- **Docker** and **Docker Compose**
- **MongoDB Compass** (optional, for visualization)

### 2️⃣. Start the container
From the project root, run:

```bash
docker compose up -d
```

MongoDB will start on port `localhost:27017`.

### 3️⃣. Access credentials

| Variable | Value |
|-----------|--------|
| `MONGO_INITDB_ROOT_USERNAME` | `admin` |
| `MONGO_INITDB_ROOT_PASSWORD` | `secret123` |

Connection URI (Compass or app):  
```
mongodb://admin:secret123@localhost:27017/
```

---

## ⚙️ `docker-compose.yml`

```yaml
version: "3.8"

services:
  mongodb:
    image: mongo:7.0
    container_name: mongodb_delivery
    restart: unless-stopped
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: secret123
    volumes:
      - mongodb_data:/data/db
      - ./init:/docker-entrypoint-initdb.d

volumes:
  mongodb_data:
```

---
