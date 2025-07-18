# E-commerce API

A simple e-commerce backend API built with FastAPI and MongoDB Atlas, featuring product management and order processing.

## Features

- **Product Management**: Create and list products with filtering and pagination
- **Order Management**: Create orders and view user order history
- **MongoDB Integration**: Uses MongoDB Atlas with proper indexing for performance
- **Async Operations**: Built with Motor for async MongoDB operations
- **Data Validation**: Comprehensive input validation using Pydantic
- **Error Handling**: Proper HTTP error responses
- **API Documentation**: Auto-generated OpenAPI docs

## API Endpoints

### Products

- `POST /api/v1/products` - Create a new product
- `GET /api/v1/products` - List products with optional filtering

### Orders

- `POST /api/v1/orders` - Create a new order
- `GET /api/v1/orders/{user_id}` - Get orders for a specific user

## Project Structure

```
├── app/
│   ├── __init__.py
│   ├── database.py      # Database connection and configuration
│   ├── schemas.py       # Pydantic models for request/response
│   ├── services.py      # Business logic layer
│   └── routes.py        # API route definitions
├── main.py              # FastAPI application entry point
├── requirements.txt     # Python dependencies
├── .env.example        # Environment variables example
└── README.md           # This file
```

## Setup Instructions

### Prerequisites

- Python 3.8+
- MongoDB Atlas account (free tier)
- Virtual environment (recommended)

### Local Development

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd ecommerce-api
   ```

2. **Create and activate virtual environment**
   ```bash
   python -m venv myenv
   # On Windows:
   myenv\Scripts\activate
   # On macOS/Linux:
   source myenv/bin/activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Set up environment variables**
   ```bash
   cp .env.example .env
   ```
   
   Edit `.env` and add your MongoDB Atlas connection string:
   ```
   MONGODB_URI=mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/ecommerce?retryWrites=true&w=majority
   ```

5. **Run the application**
   ```bash
   uvicorn main:app --reload
   ```

   The API will be available at `http://localhost:8000`

6. **Access API Documentation**
   - Swagger UI: `http://localhost:8000/docs`
   - ReDoc: `http://localhost:8000/redoc`

## MongoDB Atlas Setup

1. Create a free MongoDB Atlas account at [mongodb.com](https://www.mongodb.com/cloud/atlas)
2. Create a new cluster (M0 Free Tier)
3. Create a database user with read/write permissions
4. Add your IP address to the IP whitelist (or use 0.0.0.0/0 for development)
5. Get your connection string from the "Connect" button

## API Usage Examples

### Create a Product

```bash
curl -X POST "http://localhost:8000/api/v1/products" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "T-shirt",
    "size": "large",
    "price": 299,
    "quantity": 20
  }'
```

### List Products

```bash
# Get all products
curl "http://localhost:8000/api/v1/products"

# Search by name
curl "http://localhost:8000/api/v1/products?name=shirt"

# Filter by size with pagination
curl "http://localhost:8000/api/v1/products?size=large&limit=5&offset=0"
```

### Create an Order

```bash
curl -X POST "http://localhost:8000/api/v1/orders" \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": "12345",
    "products": [
      {
        "product_id": "60f7b3b3b3b3b3b3b3b3b3b3",
        "quantity": 2
      }
    ]
  }'
```

### Get User Orders

```bash
curl "http://localhost:8000/api/v1/orders/12345?limit=10&offset=0"
```

## Database Schema

### Products Collection
```json
{
  "_id": "ObjectId",
  "name": "string",
  "size": "string",
  "price": "number",
  "quantity": "number"
}
```

### Orders Collection
```json
{
  "_id": "ObjectId",
  "user_id": "string",
  "products": [
    {
      "product_id": "string",
      "quantity": "number"
    }
  ],
  "created_at": "datetime"
}
```

## Deployment

### Deploy to Render

1. Create a new Web Service on [Render](https://render.com)
2. Connect your GitHub repository
3. Use the following settings:
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`
4. Add environment variables in Render dashboard:
   - `MONGODB_URI`: Your MongoDB Atlas connection string
   - `DATABASE_NAME`: ecommerce

### Deploy to Railway

1. Create a new project on [Railway](https://railway.app)
2. Connect your GitHub repository
3. Railway will auto-detect the Python app
4. Add environment variables:
   - `MONGODB_URI`: Your MongoDB Atlas connection string
   - `DATABASE_NAME`: ecommerce

## Performance Optimizations

The application includes several performance optimizations:

1. **Database Indexes**: Automatic creation of indexes on frequently queried fields
2. **Pagination**: All list endpoints support limit/offset pagination
3. **Async Operations**: All database operations are asynchronous
4. **Connection Pooling**: Motor handles connection pooling automatically

## Error Handling

The API includes comprehensive error handling:

- **400 Bad Request**: Invalid input data or business logic errors
- **404 Not Found**: Resource not found
- **500 Internal Server Error**: Server-side errors

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

This project is licensed under the MIT License.
