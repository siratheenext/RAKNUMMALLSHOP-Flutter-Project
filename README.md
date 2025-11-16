
# 🌊 RAKNUMMALLSHOP Flutter Project

## 🧾 Instructions for Professor/Tester

This project is a Flutter-based e-commerce system that connects to a MySQL database via a Node.js backend.

---

## 📦 Project Structure

```
📁 lib/                  ← โค้ด Flutter
├── components/         ← UI component ต่าง ๆ
├── models/             ← Model class เช่น product, cart
├── pages/              ← หน้าจอทั้งหมด (login, home, cart, etc.)
├── services/           ← ฟังก์ชัน service เช่น cart, API call
├── firebase_options.dart (✅ ถ้าใช้ Firebase ให้เก็บไว้)
├── main.dart           ← จุดเริ่มต้นแอป

📁 node-backend/         ← โค้ดฝั่ง backend Node.js + Express
├── server.js
├── Database/           ← ไฟล์ SQL สำหรับ import ฐานข้อมูล
│   ├── raknummallshop_customer.sql
│   ├── raknummallshop_products.sql
│   ├── raknummallshop_purchase_history.sql
│   └── raknummallshop_reviews.sql

📄 README.md
```

---

## 🛠️ Setup Steps After Cloning

### 1. Install Flutter Dependencies
```
flutter pub get
```

### 2. Install Node.js and Backend Dependencies

```bash
cd node-backend
npm install
```

### 3. Enable Developer Mode (if on Windows)

- Command in VS Code Terminal:

```bash
start ms-settings:developers
```

> ✅ Only required when running on an Emulator on Windows. Not necessary for a real Android device.

### 4. Create the MySQL Database

1. Open MySQL Workbench or CLI and run the SQL commands below 👇

```sql
CREATE DATABASE RAKNUMMALLSHOP;
USE RAKNUMMALLSHOP;

-- table products
CREATE TABLE products (
  productid VARCHAR(36) PRIMARY KEY,
  price DOUBLE NOT NULL,
  type_product VARCHAR(50) NOT NULL,
  description_product TEXT NOT NULL,
  imageURL TEXT
);

-- table customer
CREATE TABLE customer (
  customerid VARCHAR(36) PRIMARY KEY,
  fullname VARCHAR(100) NOT NULL,
  email VARCHAR(100) NOT NULL UNIQUE,
  password VARCHAR(255) NOT NULL,
  phone VARCHAR(20),
  postalCode VARCHAR(10),
  idCard VARCHAR(20),
  address TEXT
);

-- table reviews
CREATE TABLE reviews (
  review_id INT AUTO_INCREMENT PRIMARY KEY,
  product_id VARCHAR(36),
  username VARCHAR(100),
  comment TEXT,
  rating INT,
  date DATE
);

-- table purchase_history
CREATE TABLE purchase_history (
  id INT AUTO_INCREMENT PRIMARY KEY,
  customerid VARCHAR(255),
  productid VARCHAR(255),
  quantity INT,
  status VARCHAR(50) DEFAULT 'paid'
);
```

### 5. Add All Product Data and Samples

Import the .sql files or use the SQL from the Database/ folder, which contains the full dataset (all product types + real reviews):

```sql
-- Run these files in MySQL Workbench or CLI:
source raknummallshop_products.sql;
source raknummallshop_customer.sql;
source raknummallshop_purchase_history.sql;
source raknummallshop_reviews.sql;
```

> ✅ Alternatively, you can copy the SQL commands from each file and paste them into MySQL Workbench.

### 6. Create the 'flutter' MySQL User (for the backend)

```sql
DROP USER IF EXISTS 'flutter'@'localhost';
CREATE USER 'flutter'@'localhost' IDENTIFIED BY '123456';
GRANT ALL PRIVILEGES ON RAKNUMMALLSHOP.* TO 'flutter'@'localhost';
FLUSH PRIVILEGES;
```

---

## ▶️ How to Run the App
Important! There are two ways to run the app: connecting via an Emulator in VS Code, or running on a real mobile device. These methods use different URLs.
In login.dart, sign_in.dart, api_service.dart, purchase_service.dart, and review_service.dart, two different base URLs are used:

### -When running on an Emulator, use: http://10.0.2.2:3000/
### When running on a real phone, use: http://'your_computer_ip_address'/
***The default setting in the code is for the emulator.

### 📱 Frontend (Flutter)

```bash
flutter run -d emulator-5554
```

> Or run from the Android Emulator opened in Android Studio.

### 🔙 Backend (Node.js)

```bash
cd node-backend
node server.js
```

---

## 🔗 Notes

- **Firebase**: The project has a basic Firebase setup only to demonstrate future connectivity.
  - Firebase is not actively used in the current code.
  - If you want to enable Firebase:
    1. Register a Firebase Project on the [Firebase Console](https://console.firebase.google.com)
    2. Add the`google-services.json` to `android/app/`
    3. Configure `firebase_options.dart` with `flutterfire configure`
    4. Add `await Firebase.initializeApp()` at `main.dart`

---
