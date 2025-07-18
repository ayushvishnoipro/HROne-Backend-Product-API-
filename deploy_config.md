# Deployment Configuration

## Render Deployment

Create a `render.yaml` file in your project root for easy deployment:

```yaml
services:
  - type: web
    name: ecommerce-api
    env: python
    buildCommand: pip install -r requirements.txt
    startCommand: uvicorn main:app --host 0.0.0.0 --port $PORT
    envVars:
      - key: MONGODB_URI
        value: your_mongodb_connection_string
      - key: DATABASE_NAME
        value: ecommerce
```

## Railway Deployment

Create a `railway.json` file:

```json
{
  "$schema": "https://railway.app/railway.schema.json",
  "build": {
    "builder": "NIXPACKS"
  },
  "deploy": {
    "startCommand": "uvicorn main:app --host 0.0.0.0 --port $PORT",
    "restartPolicyType": "ON_FAILURE",
    "restartPolicyMaxRetries": 10
  }
}
```

## Environment Variables for Production

Make sure to set these environment variables in your deployment platform:

- `MONGODB_URI`: Your MongoDB Atlas connection string
- `DATABASE_NAME`: ecommerce (or your preferred database name)
- `DEBUG`: false (for production)
- `HOST`: 0.0.0.0
- `PORT`: Will be set automatically by most platforms
