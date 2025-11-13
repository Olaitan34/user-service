# 🚀 Render.com Deployment Guide

## Quick Deploy (5 Minutes)

Your Django User Service is now configured for Render.com!

---

## 📋 Step-by-Step Deployment

### Step 1: Push Code to GitHub

Make sure all files are pushed:

```bash
git add .
git commit -m "feat: add Render deployment configuration"
git push origin user-service
```

---

### Step 2: Create Render Account

1. Go to https://render.com
2. Sign up with GitHub (free)
3. Authorize Render to access your repositories

---

### Step 3: Deploy Web Service

#### Option A: Manual Setup (Recommended for first time)

1. **Click "New +"** → **"Web Service"**

2. **Connect Repository:**
   - Select `Olaitan34/user-service`
   - Branch: `user-service`
   - Root Directory: `user-service`

3. **Configure Service:**
   ```
   Name: user-service
   Region: Oregon (or closest to you)
   Branch: user-service
   Root Directory: user-service
   Runtime: Python 3
   Build Command: ./build.sh
   Start Command: gunicorn user_service.wsgi:application --bind 0.0.0.0:$PORT --workers 4
   Plan: Free
   ```

4. **Add Environment Variables** (click "Advanced" → "Add Environment Variable"):

   ```env
   SECRET_KEY=<copy from your .env file>
   DEBUG=False
   ALLOWED_HOSTS=.onrender.com
   
   USE_POSTGRES=true
   DB_NAME=defaultdb
   DB_USER=avnadmin
   DB_PASSWORD=<copy from your .env file>
   DB_HOST=user-contries-currency1.i.aivencloud.com
   DB_PORT=18360
   
   USE_REDIS=true
   REDIS_HOST=user-service-jumw-jhie-146808.leapcell.cloud
   REDIS_PORT=6379
   REDIS_DB=0
   REDIS_PASSWORD=<copy from your .env file>
   
   DJANGO_SETTINGS_MODULE=user_service.settings
   PYTHON_VERSION=3.12.0
   ```

5. **Click "Create Web Service"**

---

#### Option B: Blueprint (One-Click Deploy)

1. Click "New +" → "Blueprint"
2. Connect your repository
3. Render will detect `render.yaml` and set everything up automatically
4. You'll still need to add the secret environment variables manually

---

### Step 4: Wait for Build

Render will:
- ✅ Clone your repository
- ✅ Install dependencies from `requirements.txt`
- ✅ Collect static files
- ✅ Run database migrations
- ✅ Start Gunicorn server

Build time: ~3-5 minutes

---

### Step 5: Verify Deployment

Once deployed, Render gives you a URL like:
```
https://user-service-xxxx.onrender.com
```

Test your endpoints:

```bash
# Health check
curl https://your-app.onrender.com/health

# Should return:
# {"status": "healthy", "redis": "connected", "database": "connected"}
```

---

## 🔧 Important Notes

### Free Tier Limitations

⚠️ **Render Free Tier:**
- Service spins down after 15 minutes of inactivity
- First request after sleep takes ~30-60 seconds
- 750 hours/month (enough for continuous running)

**Solution for slow first load:**
- Use a service like UptimeRobot (free) to ping your app every 10 minutes
- Or upgrade to paid plan ($7/month for always-on)

### Static Files

Render serves static files automatically using WhiteNoise (already in your requirements.txt).

Your `build.sh` runs `collectstatic` during build.

### Database

You're using your existing Aiven PostgreSQL database - no changes needed!

---

## 🎯 Environment Variables Reference

### Required (Add in Render Dashboard):

```env
SECRET_KEY=your-django-secret-key-from-local-env
DB_PASSWORD=your-aiven-password-from-local-env
REDIS_PASSWORD=your-redis-password-from-local-env
```

### Pre-configured (already in render.yaml):

```env
DEBUG=False
PYTHON_VERSION=3.12.0
DJANGO_SETTINGS_MODULE=user_service.settings
USE_POSTGRES=true
USE_REDIS=true
REDIS_PORT=6379
REDIS_DB=0
```

### Database Settings:

```env
DB_NAME=defaultdb
DB_USER=avnadmin
DB_HOST=user-contries-currency1.i.aivencloud.com
DB_PORT=18360
```

### Redis Settings:

```env
REDIS_HOST=user-service-jumw-jhie-146808.leapcell.cloud
```

### Hosts:

```env
ALLOWED_HOSTS=.onrender.com
```

(Render will add your specific domain automatically)

---

## 📊 Post-Deployment

### View Logs

In Render dashboard:
1. Click on your service
2. Go to "Logs" tab
3. See real-time logs

### Update ALLOWED_HOSTS

After first deploy, update your environment variable:

```env
ALLOWED_HOSTS=.onrender.com,your-app-name.onrender.com
```

---

## 🔄 Continuous Deployment

**Auto-deploy is enabled by default!**

Every push to the `user-service` branch will trigger a new deployment.

To disable:
1. Service Settings → Build & Deploy
2. Toggle "Auto-Deploy" off

---

## 🎤 For Your Presentation

### Live Demo URLs:

```
Health: https://your-app.onrender.com/health
API Docs: https://your-app.onrender.com/api/v1/users/
Login: https://your-app.onrender.com/api/v1/users/login
```

### Test Commands:

```bash
# Register user
curl -X POST https://your-app.onrender.com/api/v1/users/ \
  -H "Content-Type: application/json" \
  -d '{
    "email": "demo@example.com",
    "password": "SecurePass123!",
    "name": "Demo User"
  }'

# Login
curl -X POST https://your-app.onrender.com/api/v1/users/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "demo@example.com",
    "password": "SecurePass123!"
  }'
```

---

## 🛠️ Troubleshooting

### Build Fails

**Check Render logs for:**
- Missing dependencies in `requirements.txt`
- Python version mismatch
- Database connection errors

**Common fixes:**
- Ensure `build.sh` is executable (it should be)
- Check all environment variables are set
- Verify DATABASE_URL or individual DB vars

### App Won't Start

**Check:**
- `ALLOWED_HOSTS` includes `.onrender.com`
- `SECRET_KEY` is set
- Database credentials are correct
- Port is set correctly (Render provides `$PORT`)

### Database Connection Errors

**Verify:**
- `USE_POSTGRES=true` is set
- All DB credentials match your Aiven database
- Aiven database allows connections from Render IPs

---

## 💰 Cost

**Free Plan:**
- ✅ 750 hours/month
- ✅ Unlimited bandwidth
- ✅ Free SSL certificates
- ⚠️ Spins down after 15 min inactivity

**Paid Plan ($7/month):**
- Always-on
- No spin-down
- Better for production

---

## ✅ Summary

**Your app is now Render-ready!**

**Files added:**
- `render.yaml` - Render configuration
- `build.sh` - Build script
- `RENDER_DEPLOY.md` - This guide

**Next steps:**
1. Push to GitHub
2. Create Render account
3. Connect repository
4. Add environment variables
5. Deploy!

**Deploy time:** ~5 minutes  
**Your app URL:** `https://user-service-xxxx.onrender.com`

Good luck with your presentation! 🎉
