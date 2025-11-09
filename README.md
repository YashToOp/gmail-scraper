# 📧 Gmail Scraper Pro - MERN + Puppeteer + Stealth

A production-grade full-stack web application that automates Gmail inbox scraping with advanced bot detection bypass, using Puppeteer Extra with stealth plugin, React 18 GUI, and Express backend. Exports email data to CSV format with robust pagination support (1-1000+ emails).

> ⚠️ **DISCLAIMER**: This is an educational project demonstrating web automation techniques. Use responsibly and only with accounts you own. Respect Gmail's Terms of Service and rate limits.

## 🌟 Key Features

✅ **Advanced Bot Detection Bypass** - 7-layer anti-detection system  
✅ **Stealth Plugin** - Hides automation indicators (`navigator.webdriver`, fingerprints)  
✅ **Human-Like Behavior** - Random mouse movements, scrolling, variable delays  
✅ **Robust Pagination** - Handles 1-1000+ emails with retry mechanism  
✅ **Real-Time Progress** - Server-Sent Events (SSE) terminal in GUI  
✅ **Smart CSV Export** - Chunked writing (50 emails/chunk) with automatic merging  
✅ **Duplicate Prevention** - Thread ID tracking with O(1) Set lookups  
✅ **Email Filtering** - Cleans text, validates content, removes spam patterns  
✅ **Headless/Headful Modes** - Toggle browser visibility via checkbox  
✅ **Error Recovery** - Retry logic, diagnostics (HTML/PNG/JSON snapshots)  

## 🎯 Production-Ready Enhancements

This scraper goes beyond basic automation with:
- **DOM Change Detection**: Waits for real inbox updates after pagination
- **3-Tier Click Strategy**: Marked element → Aria-label → XPath fallbacks
- **n+1 Chunks Strategy**: Scrapes extra, filters to exact count requested
- **Async Diagnostics**: Saves debug snapshots on pagination failures
- **Text Refinement**: Removes ads, unsubscribe links, image tags, URLs

---

## 🏗️ Architecture

**Tech Stack:**
- **Frontend**: React 18 + Vite + Axios
- **Backend**: Node.js + Express + CORS
- **Automation**: Puppeteer Extra + Stealth Plugin
- **Export Format**: CSV (chunked writing with csv-writer)
- **Real-Time Updates**: Server-Sent Events (SSE)

**Data Flow:**
```
User enters credentials in React GUI
    ↓
POST /api/scrape/start (with SSE connection)
    ↓
Puppeteer Extra + Stealth Plugin launches Chrome
    ↓
Automated Gmail login with human-like typing delays
    ↓
Pagination loop (50 emails/page with DOM change detection)
    ↓
Email extraction → Duplicate filtering → Text refinement
    ↓
Chunked CSV writing (50 emails per part file)
    ↓
Final CSV merge with validation
    ↓
Frontend receives download link via SSE
    ↓
User downloads sanitized CSV file
```

**Anti-Detection Flow:**
```
Browser Launch → Stealth Plugin patches automation flags
    ↓
Login → Human typing speed (100ms/char) + random pauses
    ↓
Navigation → Random micro-scrolls + mouse movements
    ↓
Pagination → Smooth scroll to bottom + hover over button
    ↓
Click → Native Puppeteer click (real mouse events)
    ↓
Wait → DOM polling for thread ID changes (25s timeout)
```

---

## 📁 Project Structure

```
gmail-scraper-pro/
├── backend/
│   ├── server.js              # Express server with SSE support
│   ├── routes/
│   │   └── scrape.js          # API routes (/start, /download)
│   ├── puppeteer/
│   │   └── scrapeGmail.js     # Main scraper (847 lines, stealth-enabled)
│   ├── utils/
│   │   └── csvMerge.js        # Robust CSV parsing and merging
│   ├── outputs/               # Generated CSV files (gitignored)
│   ├── tmp/                   # Error diagnostics (gitignored)
│   ├── package.json           # Dependencies: puppeteer-extra, stealth plugin
│   ├── .env.example           # Environment template
│   └── .gitignore
├── frontend/
│   ├── src/
│   │   ├── App.jsx            # Main React component with SSE terminal
│   │   ├── App.css            # Modern UI styles
│   │   ├── index.css          # Global styles
│   │   └── main.jsx           # Entry point
│   ├── index.html
│   ├── vite.config.js
│   ├── package.json
│   └── .gitignore
├── .gitignore                 # Root gitignore
├── README.md                  # This file
├── BOT_DETECTION_BYPASS.md    # Detailed anti-detection docs
├── IMPLEMENTATION_NOTES.md    # Technical implementation details
├── PAGINATION_PATCH_SUMMARY.md # Pagination robustness guide
├── EXPECTED_LOGS_FORMAT.md    # Log format documentation
└── TEST_VERIFICATION.md       # Test results and verification
```

---

## 🚀 Quick Start

### Prerequisites
- **Node.js** v18+ (tested on v20.x)
- **npm** or **yarn**
- **Google Chrome** (Puppeteer will use your installed Chrome)
- A **test Gmail account** (preferably without sensitive data)

### Installation

1. **Clone the repository:**
   ```bash
   git clone <your-repo-url>
   cd gmail-scraper-pro
   ```

2. **Install backend dependencies:**
   ```bash
   cd backend
   npm install
   ```
   
   This installs:
   - `puppeteer-extra` - Enhanced Puppeteer with plugin support
   - `puppeteer-extra-plugin-stealth` - Bot detection bypass
   - `express`, `cors`, `csv-writer`, `dotenv`

3. **Install frontend dependencies:**
   ```bash
   cd ../frontend
   npm install
   ```
   
   This installs:
   - `react`, `react-dom`, `axios`
   - `vite`, `@vitejs/plugin-react`

4. **Configure environment (optional):**
   ```bash
   cd ../backend
   cp .env.example .env
   ```
   
   Edit `.env` if you need custom settings:
   ```env
   PORT=4000
   PUPPETEER_HEADLESS=false
   MAX_SCRAPER_RUNTIME_MS=2400000
   CSV_OUTPUT_DIR=./outputs
   ```

---

## ▶️ Running the Application

### Method 1: Two Terminals (Recommended)

**Terminal 1 - Backend:**
```bash
cd backend
npm run dev
```
✅ Backend runs at: **http://localhost:4000**  
✅ Real-time logs show scraping progress

**Terminal 2 - Frontend:**
```bash
cd frontend
npm run dev
```
✅ Frontend runs at: **http://localhost:5173**  
✅ Open in your browser automatically

### Method 2: Production Mode

```bash
# Build frontend
cd frontend
npm run build

# Serve frontend from backend (future enhancement)
cd ../backend
npm start
```

---

## 🎯 Usage Guide

### Step-by-Step:

1. **Open the application**  
   Navigate to **http://localhost:5173** in your browser

2. **Enter credentials**
   - **Gmail Address**: Your test account email
   - **Password**: Account password (or app-specific password if 2FA enabled)
   - **Max Emails**: Number to scrape (1-1000+)
   - **Headless Mode**: Check to hide browser window

3. **Start scraping**  
   Click **"Start Scrape"** button

4. **Watch real-time progress**  
   The terminal panel shows live updates:
   ```
   🚀 Launching browser...
   🔑 Navigating to Gmail login...
   📧 Entering email address...
   ✅ Successfully logged into Gmail
   📊 Page 1: Extracting emails...
   ✅ Page 2 loaded successfully
   💾 Saved chunk 1 (50 emails total)
   📦 Merging CSV parts into final file...
   ✅ Scraping complete!
   ```

5. **Download results**  
   Click **"Download CSV"** when complete

### CSV Output Format:
```csv
FROM,SUBJECT,DATE,SNIPPET,BODY
john@example.com,Meeting Tomorrow,"Nov 10, 2025 10:30 AM","Hi, confirming our...","Full email content here"
alerts@service.com,Your Weekly Report,"Nov 9, 2025 8:00 PM","Here's your summary...","Detailed report body"
```

### Scraping Capabilities:
- ✅ **Small batches**: 1-50 emails (1 page, ~10 seconds)
- ✅ **Medium batches**: 51-200 emails (2-4 pages, ~30 seconds)
- ✅ **Large batches**: 201-1000 emails (5-20 pages, 2-5 minutes)
- ✅ **Extra large**: 1000+ emails (requires longer runtime)

---

## 🔧 API Reference

### Health Check
```http
GET /api/health
```

**Response:**
```json
{
  "ok": true,
  "timestamp": "2025-11-10T10:30:00.000Z"
}
```

### Start Scraping (with SSE)
```http
POST /api/scrape/start
Content-Type: application/json

{
  "email": "test@gmail.com",
  "password": "yourpassword",
  "maxEmails": 100,
  "headless": false
}
```

**Success Response:**
```json
{
  "ok": true,
  "count": 100,
  "file": "/api/scrape/download/emails_1699524000000.csv",
  "message": "Successfully scraped 100 emails"
}
```

**Error Response:**
```json
{
  "ok": false,
  "error": "Scraping failed",
  "message": "Failed to load inbox. Check credentials or account security settings.",
  "partial": true,
  "count": 45
}
```

### SSE Progress Events
The scraper sends real-time updates via Server-Sent Events:

```javascript
// Frontend receives these events:
{
  message: "✅ Page 2 loaded successfully",
  type: "success",
  timestamp: "2025-11-10T10:35:22.123Z"
}

{
  message: "🔄 Retrying pagination click (attempt 2/3)...",
  type: "warning",
  timestamp: "2025-11-10T10:35:25.456Z"
}
```

**Event Types:**
- `info` - General progress updates
- `success` - Successful operations (green in terminal)
- `warning` - Retries or non-critical issues (yellow)
- `error` - Critical failures (red)

### Download CSV
```http
GET /api/scrape/download/:filename
```

**Response:**  
Binary CSV file with `Content-Disposition: attachment`

---

## 🧪 Testing

### Manual Testing with cURL (Windows PowerShell)

```powershell
# Test health endpoint
curl http://localhost:4000/api/health

# Start scrape (small batch)
curl -X POST http://localhost:4000/api/scrape/start `
  -H "Content-Type: application/json" `
  -d '{\"email\":\"test@gmail.com\",\"password\":\"password123\",\"maxEmails\":50,\"headless\":false}'
```

### Verified Test Results

✅ **Test 1: 100 emails (3 pages)**  
- Duration: ~25 seconds
- Result: All unique emails, no duplicates
- CSV: 100 rows, properly formatted

✅ **Test 2: 201 emails (6 pages)**  
- Duration: ~50 seconds  
- Result: Perfect pagination, thread ID changes detected
- CSV: 201 rows, no duplicate blocks

✅ **Test 3: Stealth plugin active**
- `navigator.webdriver`: undefined ✅
- Random mouse movements: Working ✅
- Human-like delays: 200ms-2s ✅

See `TEST_VERIFICATION.md` for detailed logs.

---

## 🐛 Troubleshooting

### Issue: "Failed to load inbox"
**Symptoms**: Login succeeds but inbox doesn't load  
**Causes**:
- Invalid credentials
- 2FA enabled without app password
- Account locked by Google

**Solutions**:
1. Use an [App Password](https://myaccount.google.com/apppasswords) if 2FA is enabled
2. Try a different test account
3. Check `backend/tmp/` for debug screenshots
4. Run in non-headless mode to see what's happening

### Issue: CAPTCHA appears
**Symptoms**: Google challenges you with reCAPTCHA  
**Causes**:
- New account with no login history
- Scraping too frequently
- Detected as automated browser

**Solutions**:
1. Use accounts with prior manual login history
2. Run in non-headless mode first
3. Add longer delays between sessions
4. The stealth plugin should prevent this (already active)

### Issue: Pagination gets stuck
**Symptoms**: Log shows "DOM did not change after clicking"  
**Causes**:
- Gmail UI changed
- "Older" button not found
- Rate limiting

**Solutions**:
1. Check `backend/tmp/pagination-stuck-*.{html,png,json}` for diagnostics
2. The scraper has 3-tier click strategies (should handle this)
3. Try reducing `maxEmails` temporarily
4. Wait 5-10 minutes before retrying

### Issue: CSV has duplicate emails
**Symptoms**: Same emails appear multiple times  
**Solutions**:
- ✅ Already fixed! The scraper uses `seenThreadIds` Set
- Duplicates are now automatically skipped
- Check logs for "duplicates skipped" count

### Issue: "Cannot find module"
**Symptoms**: Import errors on startup  
**Solutions**:
```bash
# Reinstall backend dependencies
cd backend
rm -rf node_modules package-lock.json
npm install

# Reinstall frontend dependencies
cd ../frontend
rm -rf node_modules package-lock.json
npm install
```

### Issue: Port already in use
**Symptoms**: `Error: listen EADDRINUSE :::4000`  
**Solutions**:
```powershell
# Find process using port 4000
netstat -ano | findstr :4000

# Kill process (replace PID)
taskkill /PID <PID> /F

# Or change port in backend/.env
PORT=4001
```

### Issue: CORS errors in browser console
**Symptoms**: `Access-Control-Allow-Origin` errors  
**Solutions**:
- ✅ Already configured! Backend has CORS enabled for `http://localhost:5173`
- If you change frontend port, update `backend/server.js` line 14:
  ```javascript
  origin: 'http://localhost:YOUR_NEW_PORT'
  ```

---

## 🔒 Security & Privacy

### Best Practices

1. **Never commit credentials**
   - ✅ `.env` is in `.gitignore`
   - ✅ Use `.env.example` as template only
   - ❌ Don't hardcode passwords in code

2. **Use test accounts only**
   - Gmail may flag/ban accounts using automation
   - Don't use accounts with sensitive data
   - Consider creating dedicated test accounts

3. **Rate limiting**
   - Don't scrape more than 1000 emails per session
   - Add delays between multiple scrapes (5-10 minutes)
   - Respect Gmail's infrastructure

4. **Data handling**
   - CSV files in `backend/outputs/` contain personal data
   - ✅ Directory is gitignored
   - Delete old CSV files regularly
   - Don't share CSV files publicly

5. **Alternative: Official Gmail API**
   - For production use, use [Gmail API with OAuth2](https://developers.google.com/gmail/api)
   - Supports filtering, labels, threads, attachments
   - No risk of account suspension
   - Better rate limits and reliability

### Legal & Compliance

⚠️ **Important Disclaimers:**
- This tool is for **educational purposes** and **personal testing only**
- Gmail automation may violate [Google's Terms of Service](https://policies.google.com/terms)
- You are responsible for complying with applicable laws (GDPR, CCPA, etc.)
- Not intended for commercial use or mass scraping
- Use at your own risk - author assumes no liability

### Stealth Plugin Capabilities

The anti-detection system hides:
- ✅ `navigator.webdriver` flag
- ✅ `window.chrome` object inconsistencies
- ✅ WebGL vendor/renderer fingerprints
- ✅ Automation-specific properties
- ✅ Headless browser indicators

But Google can still detect:
- ❌ Unusual login patterns (new locations, devices)
- ❌ High-frequency requests from same IP
- ❌ Behavioral patterns (too fast, too consistent)

**Recommendation**: Use responsibly with realistic delays.

---

## 📝 Technical Features

### Backend Architecture
✅ **Express Server** - RESTful API with CORS support  
✅ **Server-Sent Events (SSE)** - Real-time progress streaming  
✅ **Puppeteer Extra** - Plugin system for enhanced automation  
✅ **Stealth Plugin** - 7-layer bot detection bypass  
✅ **Chunked CSV Writing** - Memory-efficient (50 emails/chunk)  
✅ **Robust CSV Merging** - Custom parser handles quoted fields  
✅ **Error Recovery** - Retry mechanism with diagnostics  
✅ **Async File Operations** - fs.promises for non-blocking I/O  

### Frontend Architecture
✅ **React 18** - Modern hooks-based components  
✅ **Vite** - Lightning-fast dev server with HMR  
✅ **Axios** - HTTP client for API calls  
✅ **EventSource** - Native SSE client for live updates  
✅ **Responsive UI** - Clean, modern design  
✅ **Form Validation** - Real-time input checks  
✅ **Loading States** - UX feedback during operations  

### Scraping Engine
✅ **DOM Change Detection** - page.waitForFunction() polling  
✅ **3-Tier Click Strategy** - Multiple fallback methods  
✅ **Thread ID Tracking** - O(1) duplicate detection with Set  
✅ **Text Refinement** - Removes ads, URLs, spam patterns  
✅ **Email Validation** - Checks content quality  
✅ **n+1 Chunks Strategy** - Scrapes extra, filters to exact count  
✅ **Pagination Retry** - Up to 3 attempts per page  
✅ **Timeout Handling** - 40-minute max runtime  

### Anti-Detection Techniques
✅ **Stealth Plugin** - Hides automation indicators  
✅ **Random Mouse Movements** - 30% chance per page  
✅ **Micro-Scrolling** - 50-150px random scrolls  
✅ **Variable Delays** - 100ms-2s random timing  
✅ **Human Typing Speed** - 100ms per character  
✅ **Smooth Scrolling** - CSS smooth behavior  
✅ **Hover Simulation** - Before clicking buttons  

See `BOT_DETECTION_BYPASS.md` for complete details.

---

## 📚 Documentation

- **README.md** (this file) - Main documentation
- **BOT_DETECTION_BYPASS.md** - Anti-detection techniques explained
- **IMPLEMENTATION_NOTES.md** - Technical implementation details
- **PAGINATION_PATCH_SUMMARY.md** - Pagination robustness guide
- **EXPECTED_LOGS_FORMAT.md** - Log format and debugging
- **TEST_VERIFICATION.md** - Test results and verification  

---

## 🚧 Known Limitations & Future Improvements

### Current Limitations
- ⚠️ Gmail UI changes may break selectors (requires manual updates)
- ⚠️ 2FA accounts need app-specific passwords
- ⚠️ Rate limiting: Avoid >1000 emails/hour
- ⚠️ No attachment downloads (emails only)
- ⚠️ Single-user mode (no concurrent sessions)

### Planned Enhancements
- [ ] **Queue System** - Bull + Redis for concurrent scrapes
- [ ] **MongoDB Integration** - Persistent storage and history
- [ ] **Gmail API Migration** - OAuth2-based official API
- [ ] **Multi-Account Support** - Scrape multiple inboxes
- [ ] **Advanced Filtering** - By date range, sender, labels
- [ ] **Attachment Downloads** - PDFs, images, documents
- [ ] **Email Threading** - Group conversations
- [ ] **Scheduled Scraping** - Cron jobs for automation
- [ ] **Dashboard Analytics** - Visualize email patterns
- [ ] **Export Formats** - JSON, Excel, SQL database

### Performance Optimizations
- [ ] Parallel page processing (multiple tabs)
- [ ] Incremental scraping (resume from last position)
- [ ] Compression for large CSV files
- [ ] Browser context reuse (avoid multiple logins)

---

## 🤝 Contributing

This is an educational project demonstrating web automation techniques. Contributions welcome!

### How to Contribute
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Areas for Improvement
- **Selector Robustness**: Better Gmail UI change detection
- **Error Handling**: More graceful failures
- **Testing**: Unit tests for critical functions
- **Documentation**: More code comments
- **Accessibility**: Better UI/UX for screen readers

---

## 📄 License

MIT License - See LICENSE file for details.

**Disclaimer**: This project is for educational purposes only. Use at your own risk. Not affiliated with Google or Gmail.

---

## 🙏 Acknowledgments

- [Puppeteer Extra](https://github.com/berstend/puppeteer-extra) - Plugin framework
- [Stealth Plugin](https://github.com/berstend/puppeteer-extra/tree/master/packages/puppeteer-extra-plugin-stealth) - Bot detection evasion
- [React](https://react.dev/) - UI framework
- [Vite](https://vitejs.dev/) - Build tool
- [Express](https://expressjs.com/) - Backend framework

---

## 📞 Support & Contact

### Getting Help
1. **Check documentation**: Read all `.md` files in root directory
2. **Review logs**: Backend terminal shows detailed progress
3. **Debug files**: Check `backend/tmp/` for error snapshots
4. **Test with small batches**: Start with 10-50 emails to verify setup

### Reporting Issues
When reporting bugs, include:
- Node.js version (`node --version`)
- npm version (`npm --version`)
- Operating system
- Error logs from terminal
- Screenshots if UI-related
- Steps to reproduce

---

## 🎓 Educational Use Cases

This project demonstrates:
- **Web Automation** - Puppeteer browser control
- **Anti-Bot Techniques** - Stealth plugins and human behavior
- **Full-Stack Development** - React + Express architecture
- **Real-Time Updates** - Server-Sent Events (SSE)
- **CSV Processing** - Chunked writing and parsing
- **Error Recovery** - Retry mechanisms and diagnostics
- **Clean Code** - Modular structure and documentation

Perfect for learning about:
- Browser automation and web scraping
- Bot detection bypass techniques
- Real-time data streaming
- File I/O and data processing
- Frontend-backend communication

---

**Built with ❤️ for educational purposes**

⭐ **Star this repo if you found it helpful!**

---

## 📈 Project Stats

- **Total Lines of Code**: ~3,500
- **Main Scraper**: 847 lines (`scrapeGmail.js`)
- **Backend**: Express server with SSE support
- **Frontend**: React 18 with real-time terminal
- **Dependencies**: 15 core packages
- **Documentation**: 6 detailed markdown files
- **Anti-Detection**: 7-layer stealth system
- **Test Results**: ✅ Verified with 201-email scrape

---

## 🔗 Quick Links

- [Puppeteer Documentation](https://pptr.dev/)
- [Gmail API Official](https://developers.google.com/gmail/api)
- [React Documentation](https://react.dev/)
- [Express.js Guide](https://expressjs.com/en/guide/routing.html)
- [Vite Documentation](https://vitejs.dev/guide/)

---

*Last Updated: November 10, 2025*