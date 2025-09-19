# 🎫 EOXS Automation API

A REST API wrapper around EOXS Playwright automation script for creating tickets with customer data and log notes. Designed for seamless integration with n8n workflows and other automation platforms.

## 🚀 Features

- ✅ **Headless Browser Automation**: Runs without GUI in production
- ✅ **REST API**: Easy integration with n8n, webhooks, and other systems
- ✅ **Batch Processing**: Create multiple tickets in one request
- ✅ **Customer Management**: Automatically sets "Discount Pipe & Steel" as customer
- ✅ **Log Notes**: Adds email content as log notes to tickets
- ✅ **Error Handling**: Comprehensive error responses and logging
- ✅ **Health Monitoring**: Built-in health checks for uptime monitoring
- ✅ **Railway Ready**: Optimized for Railway.app deployment

## 🌐 Live API

**Base URL**: `https://your-app.up.railway.app` (after deployment)

## 📡 API Endpoints

### Health Check
```http
GET /health
```

### Create Single Ticket
```http
POST /create-ticket
Content-Type: application/json

{
  "subject": "Login Issues - Cannot Access Dashboard",
  "customer": "Discount Pipe & Steel",
  "body": "Hi Support Team, I'm having trouble logging into my account...",
  "assignedTo": "Sahaj Katiyar"
}
```

### Create Multiple Tickets (Batch)
```http
POST /create-tickets-batch
Content-Type: application/json

{
  "tickets": [
    {
      "subject": "Login Issues",
      "customer": "Discount Pipe & Steel",
      "body": "Cannot access dashboard"
    },
    {
      "subject": "Password Reset",
      "customer": "Discount Pipe & Steel", 
      "body": "Need password reset"
    }
  ]
}
```

## 🔗 n8n Integration

### HTTP Request Node Configuration:
- **Method**: POST
- **URL**: `https://your-app.up.railway.app/create-ticket`
- **Headers**: `Content-Type: application/json`
- **Body**:
```json
{
  "subject": "{{$json.subject}}",
  "customer": "Discount Pipe & Steel",
  "body": "{{$json.body}}",
  "assignedTo": "Sahaj Katiyar"
}
```

### Example n8n Workflow:
```
Email Trigger → Extract Data → HTTP Request (API) → Process Response → Send Confirmation
```

## 🚀 Quick Deploy to Railway

### 1. Deploy this Repository
[![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/new/template)

### 2. Set Environment Variables
In Railway dashboard → Variables:
```
EOXS_EMAIL=your-email@example.com
EOXS_PASSWORD=your-password
NODE_ENV=production
HEADLESS=true
```

### 3. Get Your API URL
Railway provides: `https://your-app-name.up.railway.app`

## 🛠️ Local Development

### Prerequisites
- Node.js 18+
- npm or yarn

### Installation
```bash
# Clone repository
git clone https://github.com/yourusername/eoxs-automation-api.git
cd eoxs-automation-api

# Install dependencies
npm install

# Install Playwright browsers
npx playwright install chromium

# Set environment variables
export EOXS_EMAIL="your-email@example.com"
export EOXS_PASSWORD="your-password"
export HEADLESS=true

# Start server
npm start
```

### Testing
```bash
# Test API locally
node test-api.js

# Or test with curl
curl -X POST http://localhost:3000/create-ticket \
  -H "Content-Type: application/json" \
  -d '{"subject": "Test Ticket", "customer": "Discount Pipe & Steel", "body": "Test body"}'
```

## 📊 Technical Details

### Architecture
```
Email/Data → n8n → HTTP Request → Railway API → Playwright → EOXS System
                                     ↓
                              Response with ticket data
```

### Performance
- **Execution Time**: ~2-3 minutes per ticket
- **Concurrent Requests**: Handled via Railway auto-scaling
- **Browser Resources**: Chromium launched per request (optimized for Railway)

### Error Handling
- HTTP status codes (200, 400, 500)
- Detailed error messages
- Request/response logging
- Graceful browser cleanup

## 💰 Cost Analysis

### Railway Free Tier:
- **500 hours/month** execution time
- **Each ticket**: ~3 minutes
- **Monthly capacity**: ~150+ tickets **FREE**
- **Upgrade**: $5/month for unlimited usage

## 🔧 Environment Variables

| Variable | Description | Default | Required |
|----------|-------------|---------|----------|
| `EOXS_EMAIL` | EOXS login email | - | Yes |
| `EOXS_PASSWORD` | EOXS login password | - | Yes |
| `NODE_ENV` | Environment mode | development | No |
| `HEADLESS` | Run browser headless | true in production | No |
| `PORT` | Server port | 3000 | No |

## 📁 Project Structure

```
├── server.js                 # Express.js API server
├── eoxs_playwright_automation_with_log.js  # Core automation script
├── package.json              # Dependencies and scripts
├── Dockerfile               # Container configuration
├── railway.json             # Railway deployment config
├── test-api.js              # API testing script
├── sample_email_data.json   # Sample data for testing
└── README_RAILWAY.md        # Deployment guide
```

## 🔒 Security

- ✅ Environment variables for credentials (never in code)
- ✅ Input validation and sanitization
- ✅ Error message sanitization (no credential exposure)
- ✅ HTTPS enforced in production
- ✅ Request logging for audit trail

## 📞 Support & Monitoring

### Health Check
Monitor your deployment:
```bash
curl https://your-app.up.railway.app/health
```

### Logs
View logs in Railway dashboard for debugging and monitoring.

### Common Issues
1. **Browser timeout**: Increase timeout in production
2. **Memory limits**: Railway auto-scales, but monitor usage
3. **EOXS changes**: Update selectors if EOXS UI changes

## 🤝 Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Playwright** for browser automation
- **Railway** for hosting platform
- **Express.js** for web framework
- **n8n** for workflow automation

---

**Made with ❤️ for seamless EOXS ticket automation**