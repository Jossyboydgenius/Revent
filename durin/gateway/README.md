# Revent ENS Gateway 🌐

**CCIP Read Gateway Server for Revent ENS Integration**

This is a TypeScript-based HTTP server that implements [CCIP Read](https://eips.ethereum.org/EIPS/eip-3668) functionality, enabling cross-chain ENS resolution between L1 and L2 networks for the Revent platform. It forwards ENS resolution requests from Ethereum mainnet to Layer 2 networks where event data is stored.

## 🎯 Purpose

The gateway server enables:
- **Cross-chain Name Resolution**: Resolve ENS names that point to L2 data
- **Event Discovery**: Look up event information stored on L2 via ENS names
- **IPFS Integration**: Fetch metadata from IPFS for rich event information
- **Decentralized Infrastructure**: Serverless-ready architecture for global deployment

## 🏗️ Architecture

```
L1 ENS Resolver ──CCIP Read──► Gateway Server ──Query──► L2 Registry
                                      │
                                      ▼
                              IPFS Metadata Fetch
```

## 🚀 Quick Start

### Development Setup

1. **Navigate to the gateway directory**
   ```bash
   cd durin/gateway
   ```

2. **Install dependencies**
   ```bash
   bun install
   ```

3. **Environment configuration**
   ```bash
   cp .env.example .env
   ```

4. **Configure environment variables**
   
   Edit `.env` with your configuration:
   ```bash
   # Private key for signing responses (must match resolver contract signer)
   PRIVATE_KEY=your_private_key_here
   
   # L2 RPC endpoints for different chains
   BASE_RPC_URL=https://mainnet.base.org
   OPTIMISM_RPC_URL=https://mainnet.optimism.io
   ARBITRUM_RPC_URL=https://arb1.arbitrum.io
   
   # IPFS gateway for metadata fetching
   IPFS_GATEWAY=https://ipfs.io/ipfs/
   
   # Port for local development (optional)
   PORT=3001
   ```

5. **Start the development server**
   ```bash
   bun dev
   ```

6. **Test the gateway**
   ```bash
   curl http://localhost:3001/health
   ```

## 🛠️ Development

### Project Structure
```
src/
├── ccip-read/           # CCIP Read implementation
│   ├── query.ts        # Query handling and L2 chain configuration
│   └── resolve.ts      # Resolution logic and response formatting
├── types/              # TypeScript type definitions
├── utils/              # Utility functions and helpers
└── index.ts           # Main server entry point
```

### Key Components

#### CCIP Read Implementation
- **Query Handler**: Processes incoming ENS resolution requests
- **L2 Integration**: Queries multiple L2 networks for event data
- **Response Signing**: Cryptographically signs responses for security
- **Error Handling**: Robust error handling for network and data issues

#### Multi-chain Support
- **Base**: Primary L2 for Revent events
- **Optimism**: Alternative L2 deployment
- **Arbitrum**: Additional L2 support
- **Configurable**: Easy to add new L2 networks

### API Endpoints

#### Health Check
```bash
GET /health
```
Returns server status and configuration information.

#### ENS Resolution
```bash
GET /{sender}/{data}
```
CCIP Read endpoint for ENS resolution requests.
- `sender`: The L1 resolver contract address
- `data`: Encoded resolution request data

#### Metadata Fetch
```bash
GET /metadata/{ipfsHash}
```
Fetch event metadata from IPFS with caching.

## 🚀 Deployment

### Cloudflare Workers (Recommended)

1. **Configure Wrangler**
   ```bash
   # Install Wrangler CLI
   npm install -g wrangler
   
   # Login to Cloudflare
   wrangler login
   ```

2. **Deploy to Workers**
   ```bash
   # Deploy to production
   bun run deploy
   
   # Deploy to staging
   bun run deploy:staging
   ```

3. **Set Environment Variables**
   ```bash
   # Set secrets in Cloudflare Workers
   wrangler secret put PRIVATE_KEY
   wrangler secret put BASE_RPC_URL
   # ... set all required environment variables
   ```

### Vercel Deployment

1. **Install Vercel CLI**
   ```bash
   npm install -g vercel
   ```

2. **Deploy**
   ```bash
   # Deploy to Vercel
   vercel --prod
   ```

3. **Configure Environment Variables**
   Set all required environment variables in Vercel dashboard.

### Docker Deployment

1. **Build Docker Image**
   ```bash
   docker build -t revent-gateway .
   ```

2. **Run Container**
   ```bash
   docker run -p 3001:3001 --env-file .env revent-gateway
   ```

## ⚙️ Configuration

### Chain Configuration

Add new L2 networks in `src/ccip-read/query.ts`:

```typescript
export const CHAIN_CONFIG = {
  base: {
    rpcUrl: process.env.BASE_RPC_URL,
    registryFactory: "0xDddddDdDDD8Aa1f237b4fa0669cb46892346d22d",
    chainId: 8453,
  },
  // Add new chains here
  polygon: {
    rpcUrl: process.env.POLYGON_RPC_URL,
    registryFactory: "0x...",
    chainId: 137,
  },
};
```

### IPFS Configuration

Configure IPFS gateways for metadata fetching:

```typescript
const IPFS_GATEWAYS = [
  "https://ipfs.io/ipfs/",
  "https://gateway.pinata.cloud/ipfs/",
  "https://cloudflare-ipfs.com/ipfs/",
];
```

## 🔍 Monitoring & Debugging

### Logging
```bash
# Enable debug logging
DEBUG=revent:gateway bun dev

# Production logging
LOG_LEVEL=info bun start
```

### Health Monitoring
- **Health Endpoint**: `/health` for uptime monitoring
- **Metrics**: Request count, response time, error rates
- **Alerts**: Configure alerts for gateway downtime

### Testing
```bash
# Run unit tests
bun test

# Run integration tests
bun test:integration

# Test ENS resolution
curl "http://localhost:3001/0x123.../0xabcd..."
```

## 🔐 Security

### Response Signing
- All responses are cryptographically signed
- Private key must match resolver contract signer
- Prevents response tampering and ensures authenticity

### Rate Limiting
- Built-in rate limiting to prevent abuse
- Configurable limits per IP address
- DDoS protection through Cloudflare

### Input Validation
- Strict validation of all input parameters
- Sanitization of user-provided data
- Protection against injection attacks

## 🤝 Contributing

### Development Workflow
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Run the test suite: `bun test`
6. Submit a pull request

### Code Standards
- **TypeScript**: Use strict TypeScript for all code
- **ESLint**: Follow configured linting rules
- **Prettier**: Use automatic code formatting
- **Testing**: Add tests for all new features

---

For more information about the complete Revent ENS integration, see the [Durin README](../README.md).
