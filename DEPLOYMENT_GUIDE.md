# 🚀 Deployment Guide for Travel Security App

## Pre-Deployment Checklist

- [ ] All code committed and pushed to `develop` branch
- [ ] Environment variables configured
- [ ] Database migrations completed
- [ ] Tests passing (frontend and backend)
- [ ] No console errors or warnings
- [ ] Performance optimizations applied
- [ ] Security review completed
- [ ] Documentation updated

---

## Part 1: Backend Deployment

### Option A: Deploy to Heroku

#### Prerequisites
- Heroku account (free tier available)
- Heroku CLI installed
- MongoDB Atlas account (free tier available)
- Git repository connected to Heroku

#### Step 1: Setup MongoDB Atlas

1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create free account and sign in
3. Create new project: "Travel Security App"
4. Create a cluster (free M0 tier)
5. Create database user with strong password
6. Get connection string

#### Step 2: Create Heroku App

```bash
# Login to Heroku
heroku login

# Create new app
heroku create travel-security-app-backend

# Or use existing app
heroku apps:create travel-security-backend --remote heroku
```

#### Step 3: Set Environment Variables

```bash
# Set MongoDB URI
heroku config:set MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/travel-security-app

# Set JWT secret (generate strong random string)
heroku config:set JWT_SECRET=$(openssl rand -hex 32)

# Set Node environment
heroku config:set NODE_ENV=production

# Set frontend URL
heroku config:set FRONTEND_URL=https://your-frontend-domain.com

# Verify configuration
heroku config
```

#### Step 4: Deploy Backend

```bash
# Navigate to backend directory
cd TRAVEL-SECURITY-APP

# Add Heroku remote (if not done)
heroku git:remote -a travel-security-app-backend

# Deploy
git push heroku develop:main

# Monitor logs
heroku logs --tail
```

#### Step 5: Verify Deployment

```bash
# Check app status
heroku ps

# Test health endpoint
curl https://travel-security-app-backend.herokuapp.com/api/health
```

---

### Option B: Deploy to Railway

#### Prerequisites
- Railway account (railway.app)
- GitHub repository connected
- MongoDB Atlas account

#### Step 1: Connect GitHub Repository

1. Go to [Railway.app](https://railway.app)
2. Click "New Project"
3. Select "Deploy from GitHub"
4. Authorize Railway to access your GitHub
5. Select `TRAVEL-SECURITY-APP` repository
6. Select `develop` branch

#### Step 2: Configure Environment Variables

1. In Railway dashboard, go to "Variables"
2. Add variables:

```
MONGODB_URI=mongodb+srv://user:password@cluster.mongodb.net/db
JWT_SECRET=<generate-random-string>
NODE_ENV=production
FRONTEND_URL=https://your-frontend-domain.com
PORT=5000
```

#### Step 3: Deploy

1. Click "Deploy"
2. Wait for deployment to complete
3. Check logs for errors
4. Get production URL from Railway dashboard

#### Step 4: Verify

```bash
curl https://your-railway-url.railway.app/api/health
```

---

### Option C: Deploy to AWS (Elastic Beanstalk)

#### Prerequisites
- AWS account
- AWS CLI installed
- Elastic Beanstalk CLI (EB CLI)

#### Step 1: Initialize EB Application

```bash
cd TRAVEL-SECURITY-APP

# Initialize EB app
eb init -p node.js-18 travel-security-api --region us-east-1
```

#### Step 2: Create Environment

```bash
# Create environment
eb create production-env --instance-type t3.micro

# Monitor creation
eb status
```

#### Step 3: Configure Environment Variables

```bash
# Set environment variables
eb setenv MONGODB_URI=mongodb+srv://user:password@cluster.mongodb.net/db
eb setenv JWT_SECRET=$(openssl rand -hex 32)
eb setenv NODE_ENV=production
eb setenv FRONTEND_URL=https://your-frontend-domain.com

# Verify
eb printenv
```

#### Step 4: Deploy

```bash
# Deploy code
eb deploy

# Open app in browser
eb open

# Monitor logs
eb logs --stream
```

---

## Part 2: Frontend Deployment

### Option A: Deploy to Vercel

#### Prerequisites
- Vercel account (vercel.com)
- GitHub repository connected

#### Step 1: Connect Repository

1. Go to [Vercel](https://vercel.com)
2. Click "New Project"
3. Import Git Repository
4. Select `Travel-security-App-frontend`
5. Select `develop` branch

#### Step 2: Configure Build Settings

```
Framework: Create React App
Build Command: npm run build
Output Directory: build
Install Command: npm install
```

#### Step 3: Set Environment Variables

```
REACT_APP_API_URL=https://your-backend-url.com/api
REACT_APP_SOCKET_URL=https://your-backend-url.com
```

#### Step 4: Deploy

1. Click "Deploy"
2. Wait for build and deployment
3. Get production URL
4. Test the application

#### Step 5: Configure Custom Domain (Optional)

1. In Vercel dashboard, go to "Domains"
2. Add your custom domain
3. Update DNS records at your domain registrar
4. Wait for verification (usually 24 hours)

---

### Option B: Deploy to Netlify

#### Prerequisites
- Netlify account (netlify.com)
- GitHub repository connected

#### Step 1: Connect Repository

1. Go to [Netlify](https://netlify.com)
2. Click "New site from Git"
3. Select GitHub provider
4. Authorize Netlify
5. Select `Travel-security-App-frontend` repository
6. Select `develop` branch

#### Step 2: Configure Build Settings

```
Build command: npm run build
Publish directory: build
Node version: 18.x
```

#### Step 3: Set Environment Variables

1. Go to "Site Settings" → "Build & Deploy" → "Environment"
2. Add variables:

```
REACT_APP_API_URL=https://your-backend-url.com/api
REACT_APP_SOCKET_URL=https://your-backend-url.com
```

#### Step 4: Deploy

1. Click "Deploy site"
2. Wait for build completion
3. Get production URL
4. Test all features

---

### Option C: Deploy to GitHub Pages

#### Step 1: Configure Package.json

```json
{
  "homepage": "https://yourusername.github.io/Travel-security-App-frontend",
  "scripts": {
    "predeploy": "npm run build",
    "deploy": "gh-pages -d build"
  }
}
```

#### Step 2: Install gh-pages

```bash
npm install --save-dev gh-pages
```

#### Step 3: Set Environment Variables

Create `.env.production`:
```
REACT_APP_API_URL=https://your-backend-url.com/api
REACT_APP_SOCKET_URL=https://your-backend-url.com
```

#### Step 4: Deploy

```bash
npm run deploy
```

---

## Part 3: Post-Deployment Setup

### 1. Database Initialization

```bash
# Connect to MongoDB Atlas
# Create collections (optional, MongoDB creates automatically)

# Add initial data (optional)
node scripts/seedDatabase.js
```

### 2. Add Sample Data

```javascript
// Create sample roads
const road = new Road({
  name: "Lagos-Ibadan Expressway",
  distance: 120,
  startLocation: {
    city: "Lagos",
    state: "Lagos",
    coordinates: { type: "Point", coordinates: [3.1390, 6.4969] }
  },
  endLocation: {
    city: "Ibadan",
    state: "Oyo",
    coordinates: { type: "Point", coordinates: [3.8952, 7.3774] }
  },
  roadType: "highway",
  roadCondition: "good",
  securityRating: 4
});
await road.save();
```

### 3. Create Admin Account

```bash
# Via API
curl -X POST https://your-backend.com/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "firstName": "Admin",
    "lastName": "User",
    "email": "admin@example.com",
    "password": "securepassword123",
    "state": "Lagos",
    "userType": "admin"
  }'

# Then update in MongoDB to set admin status
db.users.updateOne(
  { email: "admin@example.com" },
  { $set: { userType: "admin" } }
)
```

### 4. Configure CORS

Update backend `.env`:
```env
FRONTEND_URL=https://your-frontend-domain.com
```

Update Helmet security:
```javascript
// In server.js
app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'", "'unsafe-inline'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      imgSrc: ["'self'", "data:", "https:"],
    }
  }
}));
```

### 5. Enable HTTPS

All deployment platforms automatically provide HTTPS. Ensure:
- Backend redirects HTTP to HTTPS
- Frontend uses HTTPS URLs only
- API calls use HTTPS

---

## Part 4: Monitoring & Maintenance

### 1. Setup Error Tracking

#### Option A: Sentry (Recommended)

**Backend**:
```bash
npm install @sentry/node
```

```javascript
// In server.js
const Sentry = require("@sentry/node");

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: 1.0,
});

app.use(Sentry.Handlers.requestHandler());
app.use(Sentry.Handlers.errorHandler());
```

**Frontend**:
```bash
npm install @sentry/react
```

```javascript
// In index.js
import * as Sentry from "@sentry/react";

Sentry.init({
  dsn: process.env.REACT_APP_SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: 1.0,
});
```

#### Option B: LogRocket

```bash
npm install logrocket
```

```javascript
// In App.js
import LogRocket from "logrocket";

LogRocket.init(process.env.REACT_APP_LOGROCKET_ID);
```

### 2. Setup Logging

```javascript
// Backend logging
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});

// Use in routes
logger.info('User logged in', { userId: user._id });
```

### 3. Performance Monitoring

```javascript
// Heroku Labs feature
heroku labs:enable log-runtime-metrics -a travel-security-app-backend

// Or use New Relic
npm install newrelic
```

### 4. Database Monitoring

1. MongoDB Atlas dashboard for metrics
2. Set up alerts for:
   - High CPU usage (> 80%)
   - High memory (> 85%)
   - Connection limits
   - Query performance

### 5. Setup Uptime Monitoring

Use services like:
- [UptimeRobot](https://uptimerobot.com) - Free
- [Pingdom](https://www.pingdom.com)
- [StatusPage.io](https://www.statuspage.io)

```bash
# UptimeRobot - Monitor these endpoints
https://your-backend.com/api/health
https://your-frontend.com
```

---

## Part 5: Security Hardening

### 1. SSL/TLS Certificate
- All platforms provide free SSL
- Ensure HTTPS enforced everywhere
- Set security headers:

```javascript
app.use(helmet());
app.use(express.json({ limit: '10mb' }));
app.use(cors({
  origin: process.env.FRONTEND_URL,
  credentials: true
}));
```

### 2. Rate Limiting

```bash
npm install express-rate-limit
```

```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
});

app.use('/api/', limiter);
```

### 3. Environment Variables Security

- Never commit `.env` files
- Use platform-specific secret managers
- Rotate secrets regularly
- Restrict database access by IP

### 4. Database Security

```javascript
// MongoDB Atlas - IP Whitelist
// Add only your server IP addresses

// Enable encryption at rest
// Enable VPC Peering

// Create separate database users with minimal permissions
db.createUser({
  user: "app_user",
  pwd: "strong_password",
  roles: [ { role: "readWrite", db: "travel-security-app" } ]
})
```

### 5. API Security

- Enable CORS only for trusted origins
- Implement API rate limiting
- Use JWT with short expiration
- Implement refresh token mechanism

---

## Part 6: Testing Production

### 1. Smoke Tests

```bash
# Test health endpoints
curl https://your-backend.com/api/health
curl https://your-frontend.com

# Test critical endpoints
curl -X POST https://your-backend.com/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"test123"}'

# Test map endpoint
curl https://your-backend.com/api/map/states
```

### 2. User Acceptance Testing

- [ ] User registration works
- [ ] Login/logout works
- [ ] Map loads correctly
- [ ] Roads can be filtered
- [ ] Threats can be reported
- [ ] Mobile responsiveness
- [ ] Browser compatibility (Chrome, Firefox, Safari, Edge)

### 3. Performance Testing

Use tools:
- [GTmetrix](https://gtmetrix.com) - Page speed
- [LoadImpact](https://loadimpact.com) - Load testing
- [k6](https://k6.io) - API load testing

```bash
# Simple load test
k6 run load-test.js
```

---

## Part 7: Backup & Recovery

### 1. Database Backups

```bash
# MongoDB Atlas - Automatic backups enabled
# Set retention: 35 days

# Manual backup
mongodump --uri="mongodb+srv://user:pass@cluster.mongodb.net/db"

# Manual restore
mongorestore --uri="mongodb+srv://user:pass@cluster.mongodb.net/db" dump/
```

### 2. Code Backups

```bash
# Git ensures code is backed up
# Push to multiple remotes for safety

git remote add backup https://backup-repo-url.git
git push backup develop
```

---

## Part 8: Ongoing Maintenance

### Daily
- [ ] Check application logs for errors
- [ ] Monitor server performance
- [ ] Verify backups completed

### Weekly
- [ ] Review security alerts
- [ ] Check API response times
- [ ] Update dependencies for patches

### Monthly
- [ ] Update Node.js/React versions
- [ ] Review database performance
- [ ] Test disaster recovery
- [ ] Review user feedback

---

## Deployment Checklist Summary

```
BACKEND DEPLOYMENT:
✓ MongoDB Atlas setup
✓ Backend code pushed to Git
✓ Environment variables configured
✓ Deployed to Heroku/Railway/AWS
✓ Health endpoint working
✓ CORS configured correctly
✓ JWT secret strong and secure
✓ Logging configured

FRONTEND DEPLOYMENT:
✓ Frontend code pushed to Git
✓ Build succeeds without errors
✓ Environment variables set
✓ Deployed to Vercel/Netlify
✓ API endpoints correct
✓ All pages accessible
✓ Responsive design working
✓ No console errors

POST-DEPLOYMENT:
✓ Database initialized
✓ Sample data added
✓ Admin account created
✓ SSL/HTTPS enabled
✓ Monitoring setup
✓ Backups configured
✓ Security hardened
✓ Performance optimized
```

---

## Getting Production URLs

After deployment, update these URLs:

**Backend URL**: `https://your-backend.com`
**Frontend URL**: `https://your-frontend.com`

Update frontend `.env`:
```
REACT_APP_API_URL=https://your-backend.com/api
REACT_APP_SOCKET_URL=https://your-backend.com
```

---

## Troubleshooting Deployment

### Backend won't start
```bash
# Check logs
heroku logs --tail

# Check environment variables
heroku config

# Restart app
heroku restart
```

### Frontend blank screen
```bash
# Check console for errors
# Check if API URL is correct
# Clear browser cache
# Check CORS configuration
```

### Database connection fails
```bash
# Verify MongoDB URI
# Check IP whitelist in MongoDB Atlas
# Verify username/password
# Check network connectivity
```

---

## Support Resources

- **Heroku Docs**: https://devcenter.heroku.com
- **Vercel Docs**: https://vercel.com/docs
- **MongoDB Atlas**: https://docs.atlas.mongodb.com
- **Railway**: https://docs.railway.app

---

## Next Steps

1. Choose deployment platform
2. Set up hosting accounts
3. Configure databases
4. Deploy backend
5. Deploy frontend
6. Test thoroughly
7. Setup monitoring
8. Announce to users
9. Gather feedback
10. Plan improvements

**Congratulations! Your app is now live! 🎉**
