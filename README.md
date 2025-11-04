# Solana Token Indexing Service 🪙

A high-performance, enterprise-grade backend service designed for real-time indexing and monitoring of Solana blockchain tokens. Built with TypeScript/Node.js, this service provides comprehensive token tracking, automated Telegram notifications, and a fully documented RESTful API with Swagger integration for seamless integration into your DeFi applications.

## ✨ Key Features

- **🔍 Real-Time Token Indexing** - Automatically discover and index new token mints as they're created on Solana
- **📊 Comprehensive Monitoring** - Track live token metrics including price movements, liquidity pools, holder counts, and trading volume
- **⚡ Instant Notifications** - Receive immediate Telegram alerts when new tokens match your custom filtering criteria
- **📘 Interactive API Documentation** - Explore and test all endpoints through our built-in Swagger UI interface
- **🛠️ Modern Tech Stack** - Built with Node.js and TypeScript for type safety and maintainability
- **💾 Flexible Storage** - Choose between PostgreSQL or Redis based on your scalability requirements
- **🎯 Customizable Filters** - Configure token discovery rules to focus on relevant opportunities
- **🚀 Production-Ready** - Docker support with environment-based configuration for easy deployment

---

[![image](https://github.com/user-attachments/assets/ea441bdd-81ae-4d81-b22d-181a7150bd6d)](https://coin-indexing-app-backend.vercel.app/)



## 🚀 Getting Started

### Prerequisites

Before you begin, ensure you have the following installed and configured:

- **Node.js** (version 18 or higher) - [Download here](https://nodejs.org/)
- **npm** or **yarn** - Package manager (comes with Node.js)
- **Redis** (version 6.x or higher) - [Installation guide](https://redis.io/download)
- **Telegram Bot Token** - Obtain from [@BotFather](https://t.me/BotFather) on Telegram
- **Telegram Channel** - A channel where the bot will post notifications
- **Solana Tracker API Key** - Sign up at [SolanaTracker.io](https://solanatracker.io) to get your key

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/0xTan1319/coin-indexing-app-backend.git
cd coin-indexing-app-backend
```

### 2️⃣ Install Dependencies

Install all required Node.js packages:

```bash
npm install
```

Or if you prefer using yarn:

```bash
yarn install
```

### 3️⃣ Configure Environment Variables

Create a `.env` file in the root directory with the following configuration:

```env
# Solana Tracker API Key (get from solanatracker.io)
SOLANA_TRACKER_API_KEY=your_api_key_here

# Server port (default: 3000)
PORT=3000

# Telegram bot token from @BotFather
BOT_TOKEN=your_telegram_bot_token

# Telegram channel handle (e.g., @yourchannel)
CHANNEL_HANDLE=@yourchannel

# Your bot's name
BOT_NAME=YourBotName

# API base URL for the service
API_URL=http://localhost:3000
```

### 4️⃣ Start the Service

**Development Mode** (with hot reload for code changes):
```bash
npm run dev
```

**Production Mode**:
```bash
npm run start
```

**Build for Production** (compile TypeScript):
```bash
npm run build
```

The service will start on the port specified in your `.env` file (default: `3000`). You should see console output indicating successful connection to Redis and the Solana network.

---

## 🧠 Architecture & Workflow

The service operates through a sophisticated multi-stage pipeline:

1. **Blockchain Connection** - Establishes a WebSocket connection to Solana RPC nodes and monitors the blockchain in real-time for new token creation events
2. **Metadata Retrieval** - Automatically fetches comprehensive token metadata from Metaplex standard accounts and validated token lists
3. **Data Persistence** - Stores token information, historical data, and metrics in your configured database (PostgreSQL/Redis) for fast retrieval
4. **Intelligent Filtering** - Applies customizable business logic to identify tokens matching your specific criteria (volume thresholds, liquidity requirements, etc.)
5. **Alert Distribution** - Dispatches formatted notifications via Telegram Bot API when tokens meet your configured parameters
6. **API Layer** - Exposes a RESTful API with comprehensive endpoints for querying indexed tokens, trending data, and historical metrics

---

## 📘 Interactive API Documentation

Once your server is running, access the comprehensive Swagger UI documentation:

**Local Development:**
```
http://localhost:3000/api-docs
```

**Production:**
```
https://your-domain.com/api-docs
```

The Swagger interface provides:
- 📋 Complete endpoint documentation with request/response schemas
- 🧪 Interactive API testing - try requests directly from your browser
- 📝 Detailed parameter descriptions and validation rules
- 💡 Example requests and responses for every endpoint

---

## 🔔 Telegram Notifications

Stay informed in real-time! The service automatically sends beautifully formatted alerts to your configured Telegram channel whenever new tokens matching your criteria are detected.

**Example Notification:**

```
🚀 New Token Detected!
━━━━━━━━━━━━━━━━━━
💎 Name: Banana Coin 🍌
🔑 Mint: F5vA...DxP
🏷️ Symbol: BANANA
💰 Initial Liquidity: $50,000
📊 24h Volume: $125,000
👥 Holders: 1,247
🔗 View Details: [Link]
```

**Customization:**

Fine-tune your notification criteria by editing `src/indexer/filter.ts`:
- Set minimum liquidity thresholds
- Filter by trading volume requirements
- Specify holder count minimums
- Add custom token metadata rules
- Configure notification frequency limits

---

## 📡 API Endpoints Reference

### 🔥 Trending Data

#### Trending SOL Liquidity Pools
```http
GET /api/v1/trending
```
**Description:** Retrieves currently trending Solana liquidity pools ranked by trading activity, volume, and recent price action.

**Response:** Array of pool objects with liquidity metrics, volume data, and performance indicators.

---

#### Trending Tokens
```http
GET /api/v1/tracker/trending
```
**Description:** Returns tokens experiencing significant trading activity or price movement, updated in real-time.

**Use Case:** Identify emerging opportunities and high-momentum tokens.

---

### 📈 Token Discovery & Lists

#### Tokens Sorted by Volume
```http
GET /api/v1/tracker/tokens/volume
```
**Description:** Fetches tokens ordered by 24-hour trading volume, helping you identify the most actively traded assets.

**Query Parameters:**
- `limit` - Number of results (default: 50, max: 100)
- `offset` - Pagination offset

---

#### Multi-Metric Token Rankings
```http
GET /api/v1/tracker/tokens/multi
```
**Description:** Advanced endpoint that ranks tokens using a composite score based on volume, liquidity, holder growth, and price performance.

**Best For:** Discovering well-rounded token opportunities with balanced metrics.

---

#### Recently Listed Tokens
```http
GET /api/v1/tracker/tokens/latest
```
**Description:** Returns the most recently created or listed tokens, perfect for finding brand new opportunities.

**Freshness:** Updated every 30 seconds with new token discoveries.

---

#### Graduated Tokens
```http
GET /api/v1/tracker/tokens/graduated
```
**Description:** Lists tokens that have met specific milestone criteria (e.g., minimum liquidity, holder count, trading history).

**Benefit:** Pre-filtered tokens that have demonstrated legitimacy and staying power.

---

### 🔍 Token Details

#### Get Comprehensive Token Information
```http
GET /api/v1/tracker/tokens/{tokenAddress}
```
**Description:** Retrieves detailed information for a specific token by its Solana address.

**Path Parameters:**
- `tokenAddress` - The token's mint address on Solana

**Returns:**
- Token metadata (name, symbol, decimals)
- Current price and market cap
- Liquidity pool information
- Holder statistics
- 24h/7d/30d trading metrics
- Historical price data


---

## 🧪 Testing

Run the comprehensive test suite to ensure everything is working correctly:

```bash
# Run all tests
npm run test

# Run tests in watch mode (for development)
npm run test:watch

# Generate coverage report
npm run test:coverage
```

Tests include:
- Unit tests for indexing logic
- API endpoint integration tests
- Database connection tests
- Telegram notification mocking

---

## 🛡️ Production Deployment

### Docker Deployment (Recommended)

**Build the Docker image:**
```bash
docker build -t solana-token-indexer .
```

**Run the container:**
```bash
docker run -d \
  --name solana-indexer \
  --env-file .env \
  -p 3000:3000 \
  --restart unless-stopped \
  solana-token-indexer
```

**Using Docker Compose:**
```bash
docker-compose up -d
```

### Traditional Deployment

**Build and run on your server:**
```bash
npm run build
NODE_ENV=production npm start
```

### Deployment Checklist

- ✅ Set `NODE_ENV=production` in your environment
- ✅ Use a process manager like PM2 or systemd
- ✅ Configure reverse proxy (nginx/Apache) for SSL
- ✅ Set up monitoring and logging (PM2, Winston, etc.)
- ✅ Enable database backups
- ✅ Configure firewall rules
- ✅ Set up rate limiting on API endpoints

---

## 📁 Project Structure

```
coin-indexing-app-backend/
├── src/
│   ├── indexer/          # Token indexing logic
│   │   └── filter.ts     # Filtering criteria for tokens
│   ├── api/              # REST API routes
│   ├── telegram/         # Telegram bot integration
│   ├── database/         # Database models and connections
│   └── utils/            # Utility functions
├── tests/                # Test files
├── .env                  # Environment variables
├── package.json          # Node.js dependencies
├── tsconfig.json         # TypeScript configuration
└── README.md             # This file
```

---

## 🔧 Troubleshooting Guide

### Redis Connection Errors

**Symptoms:** `ECONNREFUSED` or `Redis connection failed` errors

**Solutions:**
```bash
# Verify Redis is running
redis-cli ping
# Should return: PONG

# Start Redis if not running
# On Ubuntu/Debian:
sudo systemctl start redis

# On macOS with Homebrew:
brew services start redis

# Check Redis logs
sudo journalctl -u redis -n 50
```

### Telegram Bot Issues

#### Bot Not Sending Messages

**Common Causes:**
- ❌ Invalid `BOT_TOKEN` - Verify token from [@BotFather](https://t.me/BotFather)
- ❌ Bot not added to channel - Add bot as administrator to your channel
- ❌ Missing permissions - Grant bot "Post Messages" permission
- ❌ Wrong channel format - Use `@channelname` (with @) or numeric chat ID
- ❌ Private channel - Bot must be explicitly added as admin

**Test your bot:**
```bash
# Test bot token validity
curl https://api.telegram.org/bot<YOUR_BOT_TOKEN>/getMe
```

### Solana Tracker API Issues

**Rate Limiting:**
- Free tier: 100 requests/hour
- Paid tier: Higher limits based on plan
- Implement caching to reduce API calls

**Invalid API Key:**
```bash
# Verify your key works
curl -H "x-api-key: YOUR_KEY" https://api.solanatracker.io/tokens/trending
```

**Solutions:**
- Check key validity at [solanatracker.io](https://solanatracker.io)
- Upgrade plan if hitting rate limits
- Monitor usage in your dashboard

### Performance Issues

**Slow Response Times:**
- Check Redis memory usage: `redis-cli info memory`
- Monitor Node.js memory: Enable heap snapshots
- Review database query performance
- Consider scaling horizontally with load balancer

**High Memory Usage:**
- Adjust cache TTL settings in configuration
- Clear Redis cache: `redis-cli FLUSHDB` (careful in production!)
- Limit concurrent API requests

### Common Error Messages

**"Cannot find module"**
```bash
# Reinstall dependencies
rm -rf node_modules package-lock.json
npm install
```

**"Port already in use"**
```bash
# Find and kill process using port 3000
lsof -ti:3000 | xargs kill -9
# Or change PORT in .env file
```

**"TypeScript compilation errors"**
```bash
# Clean build directory and rebuild
npm run clean
npm run build
```

---

## 🤝 Contributing

We welcome contributions from the community! Whether it's bug fixes, new features, documentation improvements, or suggestions, your input is valuable.

### How to Contribute

1. **Fork the Repository**
   ```bash
   # Click the "Fork" button on GitHub, then clone your fork
   git clone https://github.com/0xTan1319/coin-indexing-app-backend.git
   cd coin-indexing-app-backend
   ```

2. **Create a Feature Branch**
   ```bash
   git checkout -b feature/your-amazing-feature
   # or for bug fixes:
   git checkout -b fix/bug-description
   ```

3. **Make Your Changes**
   - Write clean, documented code
   - Follow existing code style and patterns
   - Add tests for new functionality

4. **Test Your Changes**
   ```bash
   npm run test        # Run test suite
   npm run lint        # Check code style
   npm run build       # Ensure it compiles
   ```

5. **Commit Your Changes**
   ```bash
   git add .
   git commit -m "feat: add amazing new feature"
   # Use conventional commits: feat|fix|docs|style|refactor|test|chore
   ```

6. **Push and Create Pull Request**
   ```bash
   git push origin feature/your-amazing-feature
   # Then open a PR on GitHub
   ```

### Code Quality Standards

Before submitting, ensure your code:
- ✅ Passes all existing tests (`npm run test`)
- ✅ Includes tests for new features
- ✅ Follows TypeScript best practices
- ✅ Has no linting errors (`npm run lint`)
- ✅ Includes JSDoc comments for public APIs
- ✅ Updates documentation if needed

### Areas We'd Love Help With

- 🐛 Bug fixes and issue resolution
- ✨ New API endpoints and features
- 📚 Documentation improvements
- 🧪 Additional test coverage
- ⚡ Performance optimizations
- 🌐 Internationalization support

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for complete details.

**TL;DR:** You're free to use, modify, and distribute this software, even commercially, as long as you include the original copyright notice.

---

## 🙏 Acknowledgments & Credits

This project is built on the shoulders of giants:

- **[Solana Foundation](https://solana.com/)** - High-performance blockchain infrastructure enabling fast, low-cost transactions
- **[Metaplex](https://www.metaplex.com/)** - The standard for token metadata and NFTs on Solana
- **[SolanaTracker](https://solanatracker.io/)** - Comprehensive token tracking and analytics API
- **[Telegram Bot API](https://core.telegram.org/bots/api)** - Robust messaging platform for real-time notifications
- **[Node.js](https://nodejs.org/)** & **[TypeScript](https://www.typescriptlang.org/)** - Our core development stack
- **[Redis](https://redis.io/)** - Lightning-fast in-memory data storage

Special thanks to the open-source community and all contributors who make projects like this possible.

---

## 📞 Support & Community

### Need Help?

- 🐛 **Report Bugs:** [Open an issue](https://github.com/0xTan1319/coin-indexing-app-backend/issues/new?template=bug_report.md)
- 💡 **Feature Requests:** [Suggest a feature](https://github.com/0xTan1319/coin-indexing-app-backend/issues/new?template=feature_request.md)
- 💬 **Contact** [Telegram](https://t.me/shiny0103) | [Twitter](https://x.com/0xTan1319)


### Stay Connected

- ⭐ Star this repo if you find it useful
- 👀 Watch for updates and new releases
- 🔔 Subscribe to release notifications

---


<div align="center">

**Made with ❤️ for the Solana community**

*Building the future of decentralized finance, one token at a time.*

[⬆ Back to Top](#solana-token-indexing-service-)

</div>
