# ai-task
  
---

## 📌 Table of Contents.

1. [Task 1  ](#task-1)  
2. [Task 2  ](#task-2)  
3. [Task 3  ](#task-3)  

---

## Task 2
# Claude AI for Website Development: Technical Track Guide

## 1. Technical Track
**Website Development & Backend Engineering**

---

## 2. AI Tool Selection: Claude

**Claude** is an advanced AI assistant developed by Anthropic, designed to understand requirements and generate high-quality code. It's widely used in backend development for API creation, debugging, and architecture design.

---

## 3. Tool Overview

Claude is a large language model (LLM) that excels at:
- Generating clean, efficient backend code
- Creating REST APIs with proper structure
- Explaining complex logic and best practices
- Debugging errors and optimizing performance
- Designing database schemas

**Why Claude for Backend Development?**
- Rapid API endpoint generation
- Best practices built-in
- Handles edge cases automatically
- Provides clear explanations

---

## 4. Key Features & Benefits

| Feature | Benefit |
|---------|---------|
| **Code Generation** | Creates complete API endpoints instantly |
| **Error Handling** | Implements proper validation and error responses |
| **Database Integration** | Generates queries and data models |
| **Documentation** | Adds comments and explains logic |
| **Optimization** | Suggests performance improvements |
| **Debugging** | Analyzes errors and provides solutions |

---

## 5. Real-World Use Case

### Challenge: E-Commerce Product Search API

**Problem:** Build a backend API that allows users to search and filter products by:
- Search keyword
- Price range
- Category
- Sort results

**Solution:** Create a simple Express.js API with Claude's help.

---

## 6. Backend Implementation (Node.js + Express)

### Installation

```bash
npm install express
```

### app.js - Main Server

```javascript
// app.js - Main Server
const express = require('express');
const app = express();

app.use(express.json());

// Sample product data
const products = [
  { id: 1, name: 'Laptop', price: 999, category: 'Electronics', rating: 4.5 },
  { id: 2, name: 'Phone', price: 599, category: 'Electronics', rating: 4.8 },
  { id: 3, name: 'Headphones', price: 199, category: 'Audio', rating: 4.2 },
  { id: 4, name: 'Keyboard', price: 89, category: 'Accessories', rating: 4.0 },
  { id: 5, name: 'Monitor', price: 299, category: 'Electronics', rating: 4.6 },
  { id: 6, name: 'Mouse', price: 29, category: 'Accessories', rating: 3.9 },
  { id: 7, name: 'Tablet', price: 449, category: 'Electronics', rating: 4.4 },
  { id: 8, name: 'Speakers', price: 149, category: 'Audio', rating: 4.3 }
];

// Search and filter products
app.post('/api/products/search', (req, res) => {
  try {
    const { query, minPrice, maxPrice, category, sortBy } = req.body;

    // Validate input
    if (!query) {
      return res.status(400).json({ error: 'Search query is required' });
    }

    // Filter products
    let results = products.filter(product => {
      const matchesQuery = product.name.toLowerCase().includes(query.toLowerCase());
      const matchesPrice = (!minPrice || product.price >= minPrice) && 
                          (!maxPrice || product.price <= maxPrice);
      const matchesCategory = !category || product.category === category;

      return matchesQuery && matchesPrice && matchesCategory;
    });

    // Sort results
    if (sortBy === 'price-low') {
      results.sort((a, b) => a.price - b.price);
    } else if (sortBy === 'price-high') {
      results.sort((a, b) => b.price - a.price);
    } else if (sortBy === 'rating') {
      results.sort((a, b) => b.rating - a.rating);
    }

    res.json({
      success: true,
      count: results.length,
      data: results
    });
  } catch (error) {
    res.status(500).json({ error: 'Search failed' });
  }
});

// Get all products
app.get('/api/products', (req, res) => {
  res.json({
    success: true,
    count: products.length,
    data: products
  });
});

// Get single product
app.get('/api/products/:id', (req, res) => {
  const product = products.find(p => p.id === parseInt(req.params.id));
  
  if (!product) {
    return res.status(404).json({ error: 'Product not found' });
  }

  res.json({
    success: true,
    data: product
  });
});

// Add new product
app.post('/api/products', (req, res) => {
  try {
    const { name, price, category, rating } = req.body;

    // Validate required fields
    if (!name || price === undefined || !category) {
      return res.status(400).json({ error: 'Missing required fields' });
    }

    const newProduct = {
      id: Math.max(...products.map(p => p.id)) + 1,
      name,
      price,
      category,
      rating: rating || 0
    };

    products.push(newProduct);

    res.status(201).json({
      success: true,
      message: 'Product added successfully',
      data: newProduct
    });
  } catch (error) {
    res.status(500).json({ error: 'Failed to add product' });
  }
});

// Update product
app.put('/api/products/:id', (req, res) => {
  try {
    const product = products.find(p => p.id === parseInt(req.params.id));

    if (!product) {
      return res.status(404).json({ error: 'Product not found' });
    }

    const { name, price, category, rating } = req.body;

    if (name) product.name = name;
    if (price !== undefined) product.price = price;
    if (category) product.category = category;
    if (rating !== undefined) product.rating = rating;

    res.json({
      success: true,
      message: 'Product updated successfully',
      data: product
    });
  } catch (error) {
    res.status(500).json({ error: 'Failed to update product' });
  }
});

// Delete product
app.delete('/api/products/:id', (req, res) => {
  try {
    const index = products.findIndex(p => p.id === parseInt(req.params.id));

    if (index === -1) {
      return res.status(404).json({ error: 'Product not found' });
    }

    const deletedProduct = products.splice(index, 1);

    res.json({
      success: true,
      message: 'Product deleted successfully',
      data: deletedProduct[0]
    });
  } catch (error) {
    res.status(500).json({ error: 'Failed to delete product' });
  }
});

// Start server
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

### package.json

```json
{
  "name": "product-search-api",
  "version": "1.0.0",
  "description": "Simple product search API built with Claude AI",
  "main": "app.js",
  "scripts": {
    "start": "node app.js",
    "dev": "nodemon app.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  },
  "devDependencies": {
    "nodemon": "^2.0.20"
  }
}
```

---

## 7. API Endpoints

### Search Products
Search for products with filters and sorting options.

```bash
POST /api/products/search
Content-Type: application/json

{
  "query": "phone",
  "minPrice": 100,
  "maxPrice": 600,
  "category": "Electronics",
  "sortBy": "price-low"
}
```

**Response:**
```json
{
  "success": true,
  "count": 1,
  "data": [
    {
      "id": 2,
      "name": "Phone",
      "price": 599,
      "category": "Electronics",
      "rating": 4.8
    }
  ]
}
```

### Get All Products
Retrieve all available products.

```bash
GET /api/products
```

**Response:**
```json
{
  "success": true,
  "count": 8,
  "data": [...]
}
```

### Get Single Product
Retrieve a specific product by ID.

```bash
GET /api/products/1
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "Laptop",
    "price": 999,
    "category": "Electronics",
    "rating": 4.5
  }
}
```

### Add Product
Create a new product.

```bash
POST /api/products
Content-Type: application/json

{
  "name": "Wireless Mouse",
  "price": 49,
  "category": "Accessories",
  "rating": 4.1
}
```

**Response:**
```json
{
  "success": true,
  "message": "Product added successfully",
  "data": {
    "id": 9,
    "name": "Wireless Mouse",
    "price": 49,
    "category": "Accessories",
    "rating": 4.1
  }
}
```

### Update Product
Update an existing product.

```bash
PUT /api/products/1
Content-Type: application/json

{
  "name": "Gaming Laptop",
  "price": 1299,
  "rating": 4.7
}
```

**Response:**
```json
{
  "success": true,
  "message": "Product updated successfully",
  "data": {
    "id": 1,
    "name": "Gaming Laptop",
    "price": 1299,
    "category": "Electronics",
    "rating": 4.7
  }
}
```

### Delete Product
Delete a product by ID.

```bash
DELETE /api/products/1
```

**Response:**
```json
{
  "success": true,
  "message": "Product deleted successfully",
  "data": {
    "id": 1,
    "name": "Laptop",
    "price": 999,
    "category": "Electronics",
    "rating": 4.5
  }
}
```

---

## 8. Quick Start

### Prerequisites
- Node.js (v14 or higher)
- npm or yarn

### Installation & Setup

1. Clone the repository:
```bash
git clone <repository-url>
cd product-search-api
```

2. Install dependencies:
```bash
npm install
```

3. Start the server:
```bash
npm start
```

4. The server will run on `http://localhost:3000`

### Development Mode

For development with auto-reload:
```bash
npm run dev
```

---

## 9. Testing the API

You can test the API using curl, Postman, or any HTTP client.

### Example with curl

Search for products:
```bash
curl -X POST http://localhost:3000/api/products/search \
  -H "Content-Type: application/json" \
  -d '{
    "query": "laptop",
    "minPrice": 500,
    "maxPrice": 1200,
    "sortBy": "price-low"
  }'
```

Get all products:
```bash
curl http://localhost:3000/api/products
```

---

## 10. How Claude AI Helps

✅ **Quick Code Generation** - Generates API endpoints in seconds  
✅ **Best Practices** - Error handling built-in  
✅ **Debugging** - Helps fix issues when they arise  
✅ **Documentation** - Explains what the code does  
✅ **Optimization** - Suggests improvements  

---

## 11. Key Features

- 🔍 **Advanced Search** - Find products by name, price, and category
- 📊 **Filtering** - Filter by price range and category
- 🔄 **Sorting** - Sort by price (low to high, high to low) or rating
- ✨ **CRUD Operations** - Create, Read, Update, and Delete products
- 🛡️ **Error Handling** - Comprehensive error messages and validation
- 📝 **RESTful API** - Clean and intuitive API design

---

## 12. Getting Started with Claude

Want to build similar projects? Try Claude AI!

1. Visit **[claude.ai](https://claude.ai)**
2. Describe your backend needs (e.g., "Create a product search API")
3. Receive complete, working code
4. Ask for modifications or explanations
5. Deploy and iterate

**Learn More:** https://docs.claude.com

---


## Task 3

---

## 🛠️ Recommended AI Tools for MERN Stack

### 1. **Lovable (GPT Engineer)** ⭐ Best for Full-Stack
- **Website:** https://lovable.dev
- **Capabilities:** Frontend + Backend + Database
- **Output:** React + Node.js + Express + MongoDB
- **Features:** Real-time preview, deployment, iterations

### 2. **Bolt.new (StackBlitz)**
- **Website:** https://bolt.new
- **Capabilities:** Full-stack applications
- **Output:** Complete MERN projects
- **Features:** In-browser development, instant preview

### 3. **Cursor AI**
- **Website:** https://cursor.sh
- **Capabilities:** AI-powered code editor
- **Output:** Any tech stack including MERN
- **Features:** Code completion, debugging, refactoring

### 4. **Claude (Artifacts)**
- **Website:** https://claude.ai
- **Capabilities:** Components, API design, database schemas
- **Output:** React components, Express routes, MongoDB models
- **Features:** Code explanation, best practices

### 5. **GitHub Copilot**
- **Website:** https://github.com/features/copilot
- **Capabilities:** Code completion for entire MERN stack
- **Output:** Context-aware code suggestions
- **Features:** Works in VS Code, supports all MERN technologies

