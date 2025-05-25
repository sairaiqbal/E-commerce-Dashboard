# 🛒 E-commerce Admin Dashboard

This is a web-based **E-commerce Admin Dashboard** developed for e-commerce managers to efficiently monitor and manage **revenue, orders, and inventory**. Built using **Vue.js** and **Tailwind CSS**, the dashboard offers real-time data visualization, inventory updates, and product management features—all designed with a clean and intuitive UI.

## Live Demo

🌐 [Click here to view the live dashboard](https://your-hosted-demo-link.com)



##  Features

### 1.  Revenue Analysis
- Real-time display of **total orders**, **sales (revenue)**, and **average order value**.
- Interactive **line and bar charts** showing trends over time .
- Revenue and orders data **filterable by product category** (e.g., Amazon, Walmart).
- Visually enhanced x-axis with smart date formatting.

### 2.  Inventory Management
- Full list of products with platform tags (Amazon/Walmart).
- **Filter, search, and sort** products by name or stock level.
- **Update stock levels** directly from the dashboard.
- **Low stock alerts** with forecasting tips.

### 3.  Product Registration
- Form to **register new products** with fields for:
  - Product Name
  - Description
  - Price
  - Initial Stock
  - Image Upload


## 🛠️ Tech Stack

- **Frontend:** Vue 3, Tailwind CSS
- **Backend/API:** Axios with dummy JSON data (`https://dummyjson.com`)
- **Charts:** Chart.js (via vue-chartjs)
- **State Management:** Vue `ref` + `computed`
- **Hosting:** [e.g., Vercel / Netlify / Render]

## 🧪 Sample Data

- Demo data is used from [https://dummyjson.com](https://dummyjson.com) 
- Platforms are randomly assigned between `Amazon` and `Walmart`.

## 🧑‍💻 Getting Started

### Prerequisites

- Node.js and npm installed

### Setup Instructions

1. Clone the repository:

```bash
git clone https://github.com/yourusername/ecommerce-admin-dashboard.git
cd ecommerce-admin-dashboard
