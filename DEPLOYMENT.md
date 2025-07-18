# Deployment Guide for Render

## Prerequisites
- GitHub account
- Render account (free tier available)
- MongoDB Atlas database (already set up)

## Step-by-Step Deployment Instructions

### 1. Push Your Code to GitHub

1. **Initialize Git repository** (if not already done):
   ```bash
   git init
   git add .
   git commit -m "Initial commit: FastAPI e-commerce API"
   ```

2. **Create a new repository on GitHub**:
   - Go to GitHub.com
   - Click "New repository"
   - Name it: `fastapi-ecommerce-api`
   - Don't initialize with README (since you already have files)

3. **Push to GitHub**:
   ```bash
   git remote add origin https://github.com/YOUR_USERNAME/fastapi-ecommerce-api.git
   git branch -M main
   git push -u origin main
   ```

### 2. Deploy on Render

1. **Go to Render Dashboard**:
   - Visit [render.com](https://render.com)
   - Sign up/login with your GitHub account

2. **Create a New Web Service**:
   - Click "New +" button
   - Select "Web Service"
   - Choose "Build and deploy from a Git repository"
   - Connect your GitHub repository: `fastapi-ecommerce-api`

3. **Configure the Service**:
   - **Name**: `ecommerce-api` (or your preferred name)
   - **Runtime**: `Python 3`
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`
   - **Instance Type**: `Free` (for testing)

4. **Set Environment Variables**:
   In the "Environment" section, add these variables:
   
   ```
   MONGODB_URI = mongodb+srv://Admin-Ayush:@cluster0.24zm0wj.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0
   DATABASE_NAME = ecommerce
   DEBUG = false
   HOST = 0.0.0.0
   APP_NAME = E-commerce API
   ```

5. **Deploy**:
   - Click "Create Web Service"
   - Render will automatically build and deploy your app
   - Wait for the deployment to complete (usually 2-5 minutes)

### 3. Access Your Deployed API

Once deployed, you'll get a URL like:
- **API Base URL**: `https://ecommerce-api-xxxx.onrender.com`
- **API Documentation**: `https://ecommerce-api-xxxx.onrender.com/docs`
- **Health Check**: `https://ecommerce-api-xxxx.onrender.com/health`

### 4. Test Your Deployed API

#### Create a Product:
```bash
curl -X POST "https://your-app-name.onrender.com/api/v1/products" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Deployed T-shirt",
    "size": "large",
    "price": 299.99,
    "quantity": 20
  }'
```

#### Get Products:
```bash
curl "https://your-app-name.onrender.com/api/v1/products"
```

### 5. MongoDB Atlas Configuration

Make sure your MongoDB Atlas cluster:
1. **IP Whitelist**: Add `0.0.0.0/0` to allow connections from anywhere (for production, use specific IPs)
2. **Database User**: Ensure the user has read/write permissions
3. **Connection String**: Make sure it's correct in your environment variables

### 6. Important Notes

- **Cold Starts**: Free tier services may have cold starts (delay on first request after inactivity)
- **Sleep Mode**: Free services sleep after 15 minutes of inactivity
- **Logs**: Check Render logs if there are any issues
- **HTTPS**: Render automatically provides HTTPS

### 7. Troubleshooting

If deployment fails:

1. **Check Render Logs**:
   - Go to your service dashboard
   - Click on "Logs" tab
   - Look for error messages

2. **Common Issues**:
   - **Build fails**: Check `requirements.txt` has correct package versions
   - **Start fails**: Ensure the start command is correct
   - **Database connection**: Verify MongoDB Atlas connection string and IP whitelist

3. **Debug locally first**:
   ```bash
   # Test locally with production-like settings
   export DEBUG=false
   export PORT=8000
   uvicorn main:app --host 0.0.0.0 --port $PORT
   ```

### 8. Custom Domain (Optional)

If you want a custom domain:
1. Go to your service settings
2. Add custom domain
3. Follow DNS configuration instructions

## Files Created for Deployment

- `Procfile` - Heroku-style process file
- `render.yaml` - Render configuration
- `build.sh` - Build script
- `runtime.txt` - Python version specification
- Updated `main.py` - Handle PORT environment variable

Your API is now ready for production use! 🚀
