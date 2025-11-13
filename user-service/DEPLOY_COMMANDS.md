# 🚀 EXACT LEAPCELL DEPLOYMENT COMMANDS

## Repository Settings

When creating/configuring your project in Leapcell:

- **Repository:** `Olaitan34/user-service`
- **Branch:** `user-service`
- **Root Directory:** `.` (dot) or leave empty
- **Dockerfile:** `Dockerfile` (auto-detected)
- **Latest Commit:** `c00d3e5` (fixed .dockerignore)

## Build & Start Commands

### Build Command:
```
Leave EMPTY
```
Leapcell will use your Dockerfile automatically.

### Start Command:
```
Leave EMPTY
```
Leapcell will use the CMD from your Dockerfile automatically.

### Port:
```
8000
```

## Environment Variables

Set these in the Leapcell dashboard under "Environment Variables":

```env
SECRET_KEY=<copy from your .env file>
DB_PASSWORD=<copy from your .env file>
REDIS_PASSWORD=<copy from your .env file>
DEBUG=False
USE_POSTGRES=true
DB_NAME=defaultdb
DB_USER=avnadmin
DB_HOST=user-contries-currency1.i.aivencloud.com
DB_PORT=18360
USE_REDIS=true
REDIS_HOST=user-service-jumw-jhie-146808.leapcell.cloud
REDIS_PORT=6379
REDIS_DB=0
ALLOWED_HOSTS=.leapcell.io,.leapcell.app
DJANGO_SETTINGS_MODULE=user_service.settings
```

⚠️ **Replace** `<copy from your .env file>` with actual values from your local `.env` file:
- SECRET_KEY
- DB_PASSWORD  
- REDIS_PASSWORD

## That's It!

Click **"Deploy"** and wait 2-5 minutes for:
1. Git clone
2. Docker build
3. Run migrations
4. Start application

Your app will be live at: `https://your-app-name.leapcell.io`

Test: `curl https://your-app-name.leapcell.io/health`
