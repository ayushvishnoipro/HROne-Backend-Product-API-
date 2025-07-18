# E-commerce API - Testing Guide

Welcome to the E-commerce API! This guide will walk you through testing each API endpoint using the deployed application.

## 🚀 Live API Documentation

**Deployed API**: https://hrone-backend-product-api.onrender.com/docs

**Base URL**: https://hrone-backend-product-api.onrender.com

---

## 📋 API Endpoints Overview

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/v1/products` | Create a new product |
| GET | `/api/v1/products` | Get all products with filtering |
| POST | `/api/v1/orders` | Create a new order |
| GET | `/api/v1/orders/{user_id}` | Get orders for a specific user |

---

## 🛍️ Product Management

### 1. Create Product

**Endpoint**: `POST /api/v1/products`

**Request Body**:
```json
{
  "name": "Premium T-Shirt",
  "size": "large",
  "price": 299.99,
  "quantity": 50
}
```

**Expected Response (201 Created)**:
```json
{
  "name": "Premium T-Shirt",
  "size": "large",
  "price": 299.99,
  "quantity": 50,
  "id": "60f7b3b3b3b3b3b3b3b3b3b3"
}
```

**Screenshot Placeholder**:

<img width="1919" height="979" alt="image" src="https://github.com/user-attachments/assets/ede80d3e-e6b0-42f9-9533-543c7bb4b89b" />
<img width="1919" height="791" alt="image" src="https://github.com/user-attachments/assets/964a92bc-71f3-458a-8d21-449f8f7e47f7" />

### 2. Get All Products

**Endpoint**: `GET /api/v1/products`

**Query Parameters** (all optional):
- `name`: Search by product name (partial match)
- `size`: Filter by size
- `limit`: Number of products to return (default: 10)
- `offset`: Number of products to skip (default: 0)

**Examples**:

#### Get All Products
```bash
curl "https://hrone-backend-product-api.onrender.com/api/v1/products"
```

#### Search by Name
```bash
curl "https://hrone-backend-product-api.onrender.com/api/v1/products?name=shirt"
```

#### Filter by Size with Pagination
```bash
curl "https://hrone-backend-product-api.onrender.com/api/v1/products?size=large&limit=5&offset=0"
```

**Expected Response (200 OK)**:
```json
{
  "products": [
    {
      "name": "T-shirt",
      "size": "large",
      "price": 299.99,
      "quantity": 20,
      "_id": "687a825c2f0645e8313cf8ac"
    },
    {
      "name": "Denim Jeans",
      "size": "large",
      "price": 899.5,
      "quantity": 25,
      "_id": "687a832b2f0645e8313cf8ae"
    }
  ],
  "total": 2,
  "offset": 0,
  "limit": 10
}
```

**Screenshot Placeholder**:
<img width="1919" height="792" alt="image" src="https://github.com/user-attachments/assets/87387a07-acfa-4e69-a187-f904b6195b62" />
<img width="1919" height="801" alt="image" src="https://github.com/user-attachments/assets/bd05db7c-a010-4c38-b647-556a423fdce6" />


---

## 🛒 Order Management

### 3. Create Order

**Endpoint**: `POST /api/v1/orders`

**Request Body**:
```json
{
  "user_id": "user123",
  "products": [
    {
      "product_id": "60f7b3b3b3b3b3b3b3b3b3b3",
      "quantity": 2
    },
    {
      "product_id": "60f7b3b3b3b3b3b3b3b3b3b4",
      "quantity": 1
    }
  ]
}
```

**cURL Example**:
```bash
curl -X POST "https://hrone-backend-product-api.onrender.com/api/v1/orders" \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": "user123",
    "products": [
      {
        "product_id": "60f7b3b3b3b3b3b3b3b3b3b3",
        "quantity": 2
      }
    ]
  }'
```

**Expected Response (201 Created)**:
```json
{
  "user_id": "user123",
  "products": [
    {
      "product_id": "60f7b3b3b3b3b3b3b3b3b3b3",
      "quantity": 2
    }
  ],
  "id": "60f7b3b3b3b3b3b3b3b3b3b5",
  "created_at": "2025-07-18T10:30:00.000Z"
}
```

**Screenshot Placeholder**:
<img width="1660" height="785" alt="image" src="https://github.com/user-attachments/assets/e9415bf0-ccfd-44f6-836b-dfabafdfec80" />
<img width="1864" height="810" alt="image" src="https://github.com/user-attachments/assets/1d507eb5-f7a3-4891-81d7-8a369ae7dbda" />



---

### 4. Get User Orders

**Endpoint**: `GET /api/v1/orders/{user_id}`

**Path Parameters**:
- `user_id`: The ID of the user

**Query Parameters** (optional):
- `limit`: Number of orders to return (default: 10)
- `offset`: Number of orders to skip (default: 0)

**Examples**:

#### Get All Orders for User
```bash
curl "https://hrone-backend-product-api.onrender.com/api/v1/orders/user123"
```

#### Get Orders with Pagination
```bash
curl "https://hrone-backend-product-api.onrender.com/api/v1/orders/user123?limit=5&offset=0"
```

**Expected Response (200 OK)**:
```json
{
  "orders": [
    {
      "user_id": "user123",
      "products": [
        {
          "product_id": "60f7b3b3b3b3b3b3b3b3b3b3",
          "quantity": 2
        }
      ],
      "id": "60f7b3b3b3b3b3b3b3b3b3b5",
      "created_at": "2025-07-18T10:30:00.000Z"
    }
  ],
  "total": 1,
  "offset": 0,
  "limit": 10
}
```

**Screenshot Placeholder**:
<img width="1919" height="700" alt="image" src="https://github.com/user-attachments/assets/43b839b0-f2e0-49a1-ba07-8d8f50f2c949" />
<img width="1916" height="825" alt="image" src="https://github.com/user-attachments/assets/64f104ce-ea67-4e53-a783-8060f99b6279" />



---

## 🧪 Testing Workflow

### Step 1: Create Products
1. Go to https://hrone-backend-product-api.onrender.com/docs
2. Find `POST /api/v1/products` endpoint
3. Click "Try it out"
4. Use the sample JSON above
5. Click "Execute"
6. **Copy the product ID** from the response

**Screenshot Placeholder**:
```
[Screenshot: Step-by-step product creation in Swagger UI]
```

### Step 2: Test Product Listing
1. Find `GET /api/v1/products` endpoint
2. Click "Try it out"
3. Try different combinations of filters
4. Click "Execute"

**Screenshot Placeholder**:
```
[Screenshot: Product listing with different filters applied]
```

### Step 3: Create an Order
1. Find `POST /api/v1/orders` endpoint
2. Click "Try it out"
3. **Use the actual product ID** from Step 1
4. Set a user_id (e.g., "user123")
5. Click "Execute"

**Screenshot Placeholder**:
```
[Screenshot: Order creation using real product ID]
```

### Step 4: View User Orders
1. Find `GET /api/v1/orders/{user_id}` endpoint
2. Click "Try it out"
3. Enter the same user_id from Step 3
4. Click "Execute"

**Screenshot Placeholder**:
```
[Screenshot: User orders display]
```

---

## 🔍 Additional Testing Endpoints

### Health Check
**Endpoint**: `GET /health`
```bash
curl "https://hrone-backend-product-api.onrender.com/health"
```

**Response**:
```json
{
  "status": "healthy"
}
```

### API Root
**Endpoint**: `GET /`
```bash
curl "https://hrone-backend-product-api.onrender.com/"
```

**Response**:
```json
{
  "message": "Welcome to E-commerce API",
  "docs": "/docs",
  "redoc": "/redoc"
}
```

---

## 📊 Testing Scenarios

### Scenario 1: Complete E-commerce Flow
1. Create 3 different products
2. List all products
3. Create an order with multiple products
4. Check inventory reduction
5. View user order history

**Screenshot Placeholder**:
```
[Screenshot: Complete testing flow results]
```

### Scenario 2: Error Handling
1. Try creating an order with invalid product ID
2. Try creating a product with negative price
3. Try creating an order with insufficient quantity

**Screenshot Placeholder**:
```
[Screenshot: Error responses and validation messages]
```

### Scenario 3: Pagination Testing
1. Create 15 products
2. Test pagination with limit=5
3. Test different offset values
4. Test search functionality

**Screenshot Placeholder**:
```
[Screenshot: Pagination results showing different pages]
```

---

## 🐛 Common Testing Issues

### Issue 1: Product ID Format
- **Problem**: Using invalid ObjectId format
- **Solution**: Use the actual ID returned from product creation
- **Valid Format**: `"60f7b3b3b3b3b3b3b3b3b3b3"`

### Issue 2: Insufficient Inventory
- **Problem**: Ordering more items than available
- **Solution**: Check product quantity before placing order
- **Error Response**: `"Insufficient quantity for product"`

### Issue 3: Missing Required Fields
- **Problem**: Not providing required fields
- **Solution**: Ensure all required fields are included
- **Required**: `name`, `size`, `price`, `quantity` for products

---

## 🎯 Performance Testing

### Load Testing Commands
```bash
# Test concurrent product creation
for i in {1..10}; do
  curl -X POST "https://hrone-backend-product-api.onrender.com/api/v1/products" \
    -H "Content-Type: application/json" \
    -d "{\"name\":\"Product $i\",\"size\":\"medium\",\"price\":199.99,\"quantity\":100}" &
done
```

**Screenshot Placeholder**:
```
[Screenshot: Performance testing results]
```

---

## 📱 Mobile Testing

The API works perfectly with mobile applications. Here are some mobile-specific considerations:

### React Native Example
```javascript
const createProduct = async () => {
  const response = await fetch('https://hrone-backend-product-api.onrender.com/api/v1/products', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      name: 'Mobile T-Shirt',
      size: 'medium',
      price: 199.99,
      quantity: 25
    })
  });
  
  const data = await response.json();
  console.log(data);
};
```

---

## 🔗 Additional Resources

- **API Documentation**: https://hrone-backend-product-api.onrender.com/docs
- **Alternative Docs**: https://hrone-backend-product-api.onrender.com/redoc
- **Base URL**: https://hrone-backend-product-api.onrender.com

---

## 🎉 Congratulations!

You've successfully tested all the API endpoints! Your e-commerce API is now ready for production use. The API supports:

- ✅ Product management (CRUD operations)
- ✅ Order processing with inventory management
- ✅ User order history
- ✅ Pagination and filtering
- ✅ Error handling and validation
- ✅ MongoDB integration
- ✅ Production deployment

**Screenshot Placeholder**:
```
[Screenshot: Final success message with all endpoints working]
```

Happy coding! 🚀
