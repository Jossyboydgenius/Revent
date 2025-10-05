# Revent Frontend 🎬

**Next.js Application for Decentralized Event Streaming Platform**

The Revent frontend is a modern web application built with Next.js that provides an intuitive interface for creating, discovering, and participating in live events. Built on Base with Farcaster integration, it combines Web3 functionality with real-time streaming capabilities.

## ✨ Key Features

### 🗺️ Interactive Event Discovery
- **Live Event Map**: Real-time interactive map showing ongoing events
- **Location-based Search**: Find events by geographic location and filters
- **Event Categories**: Filter by event type (eat, café, bar, entertainment)
- **Real-time Updates**: Live viewer counts and event status updates

### 🎥 Live Streaming Integration
- **WebRTC Streaming**: Browser-based peer-to-peer streaming using WHIP protocol
- **Multi-format Support**: HLS adaptive streaming for optimal viewing experience
- **Stream Management**: Start, stop, and manage live streams directly from the browser
- **Viewer Interaction**: Real-time chat and engagement features

### 🎫 Event Management
- **Event Creation**: Create and configure events with IPFS metadata storage
- **Registration System**: Handle attendee registration with smart contract integration
- **NFT Ticketing**: Generate unique NFT tickets with confirmation codes
- **Revenue Management**: Track earnings and fee distribution

### 📱 Farcaster Integration
- **Frame SDK**: Native Farcaster frame support for social sharing
- **Account Association**: Connect events to Farcaster profiles
- **Social Features**: Share events and engage with the community
- **Notification System**: Real-time notifications via Farcaster channels

## 🏗️ Technical Stack

### Core Framework
- **[Next.js 15](https://nextjs.org)**: React framework with App Router
- **[TypeScript](https://www.typescriptlang.org)**: Type-safe development
- **[Tailwind CSS](https://tailwindcss.com)**: Utility-first CSS framework
- **[pnpm](https://pnpm.io)**: Fast, disk space efficient package manager

### Web3 Integration
- **[OnchainKit](https://onchainkit.xyz)**: Coinbase's toolkit for onchain experiences
- **[Wagmi](https://wagmi.sh)**: React hooks for Ethereum interaction
- **[Viem](https://viem.sh)**: TypeScript interface for Ethereum
- **[Base](https://base.org)**: Layer 2 blockchain for fast, low-cost transactions

### Social & Streaming
- **[Farcaster Frame SDK](https://docs.farcaster.xyz/reference/frames/spec)**: Native Farcaster integration
- **[MiniKit](https://docs.base.org/builderkits/minikit/overview)**: Base's toolkit for social applications
- **[WebRTC](https://webrtc.org)**: Real-time peer-to-peer communication
- **[Simple Peer](https://github.com/feross/simple-peer)**: WebRTC wrapper for streaming

### Data & Storage
- **[The Graph Protocol](https://thegraph.com)**: Decentralized indexing and querying
- **[IPFS](https://ipfs.tech)**: Decentralized storage for metadata
- **[Pinata](https://pinata.cloud)**: IPFS pinning service
- **[Upstash Redis](https://upstash.com)**: Serverless Redis for notifications

### Maps & Location
- **[Mapbox GL JS](https://docs.mapbox.com/mapbox-gl-js/)**: Interactive maps and geolocation
- **[React Query](https://tanstack.com/query/)**: Data fetching and caching

## 🚀 Getting Started

### Prerequisites
- Node.js 18 or later
- pnpm package manager  
- A modern web browser with WebRTC support

### Installation

1. **Clone and navigate to the frontend directory**
   ```bash
   cd frontend
   ```

2. **Install dependencies**
   ```bash
   pnpm install
   ```

3. **Environment Configuration**
   ```bash
   cp .env.example .env.local
   ```

4. **Configure environment variables**

Edit `.env.local` with your configuration:

```bash
# Core Application
NEXT_PUBLIC_ONCHAINKIT_PROJECT_NAME=Revent
NEXT_PUBLIC_URL=http://localhost:3000
NEXT_PUBLIC_ICON_URL=/icon.png
NEXT_PUBLIC_ONCHAINKIT_API_KEY=your_onchainkit_api_key

# Farcaster Frame Configuration  
FARCASTER_HEADER=your_farcaster_header
FARCASTER_PAYLOAD=your_farcaster_payload
FARCASTER_SIGNATURE=your_farcaster_signature

# Application Branding
NEXT_PUBLIC_APP_ICON=/icon.png
NEXT_PUBLIC_APP_SUBTITLE=Tokenizing Events & SocialFi Platform
NEXT_PUBLIC_APP_DESCRIPTION=Discover, create, and stream live events on Base
NEXT_PUBLIC_APP_SPLASH_IMAGE=/splash.png
NEXT_PUBLIC_SPLASH_BACKGROUND_COLOR=#000000
NEXT_PUBLIC_APP_PRIMARY_CATEGORY=socialfi
NEXT_PUBLIC_APP_HERO_IMAGE=/hero.png
NEXT_PUBLIC_APP_TAGLINE=Live Events, Decentralized
NEXT_PUBLIC_APP_OG_TITLE=Revent - Decentralized Event Platform
NEXT_PUBLIC_APP_OG_DESCRIPTION=Create, discover, and stream live events on Base
NEXT_PUBLIC_APP_OG_IMAGE=/hero.png

# Streaming Configuration
NEXT_PUBLIC_WHIP_URL=http://207.180.247.72:8889
NEXT_PUBLIC_HLS_URL=http://207.180.247.72:8888

# The Graph Protocol
NEXT_PUBLIC_GRAPH_URL=your_graph_endpoint

# IPFS & Storage
NEXT_PUBLIC_PINATA_JWT=your_pinata_jwt
NEXT_PUBLIC_PINATA_GATEWAY=your_pinata_gateway

# Redis Configuration (for notifications)
REDIS_URL=your_redis_url  
REDIS_TOKEN=your_redis_token

# Mapbox (for interactive maps)
NEXT_PUBLIC_MAPBOX_ACCESS_TOKEN=your_mapbox_token

# Smart Contract Configuration
NEXT_PUBLIC_CONTRACT_ADDRESS=your_deployed_contract_address
NEXT_PUBLIC_CHAIN_ID=8453  # Base mainnet (or 84532 for Base Sepolia)
```

5. **Start the development server**
   ```bash
   pnpm dev
   ```

6. **Visit the application**
   
   Open [http://localhost:3000](http://localhost:3000) to see the application running.

## 🏗️ Application Architecture

### Component Structure
```
app/
├── components/           # Reusable UI components
│   ├── EventsMap.tsx    # Interactive event map
│   ├── StreamHome.tsx   # Main event discovery interface
│   ├── live-peer.tsx    # WebRTC streaming component
│   └── EventManagement.tsx # Event creation and management
├── api/                 # API routes and serverless functions
│   ├── events/graph/    # The Graph Protocol integration
│   ├── notify/          # Notification system
│   └── webhook/         # Webhook handlers
├── e/                   # Event-specific pages and routes
└── stream-demo/         # Streaming demonstration pages
```

### Key Features Implementation

#### 🗺️ Interactive Event Discovery
- **EventsMap Component**: Mapbox-powered interactive map with real-time event markers
- **Event Filtering**: Filter events by category, location, and status
- **Real-time Updates**: Live viewer counts and event status changes
- **Search Integration**: Location-based event search and discovery

#### 🎥 Streaming Infrastructure  
- **WebRTC Integration**: Browser-based streaming using WHIP protocol
- **Live Peer Component**: Manages peer-to-peer connections and stream quality
- **HLS Fallback**: Adaptive bitrate streaming for broader device support
- **Stream Management**: Start, stop, and configure streams from the browser

#### 🎫 Event Management
- **Event Creation Flow**: Comprehensive event setup with IPFS metadata
- **Smart Contract Integration**: Direct interaction with Revent contracts
- **Registration System**: Handle attendee registration and capacity limits
- **NFT Ticketing**: Generate and manage ERC721 event tickets

#### 📱 Farcaster Integration
- **Frame Configuration**: `.well-known/farcaster.json` for Frame metadata
- **Account Association**: Connect events to Farcaster profiles  
- **Social Sharing**: Native sharing of events to Farcaster feeds
- **Notification System**: Real-time event updates via Farcaster

#### 🔔 Notification System
- **Redis Backend**: Upstash Redis for notification storage and queuing
- **Webhook Integration**: Handle incoming notifications from external services
- **Real-time Updates**: WebSocket-like functionality for live notifications
- **Farcaster Delivery**: Send notifications through Farcaster channels

#### 🎨 Design System
- **Custom Theme**: Defined in `theme.css` with OnchainKit variables
- **Dark/Light Mode**: Automatic theme switching with system preference
- **Responsive Design**: Mobile-first design with desktop optimization
- **Interactive Animations**: Smooth transitions and micro-interactions

## 🛠️ Development

### Development Commands

```bash
# Start development server with hot reload
pnpm dev

# Build for production
pnpm build

# Start production server (requires build first)
pnpm start

# Run linting and type checking
pnpm lint

# Run type checking only
pnpm type-check
```

### Code Quality Tools

```bash
# Format code with Prettier
pnpm format

# Run ESLint with auto-fix
pnpm lint:fix

# Check TypeScript types
pnpm type-check

# Run all quality checks
pnpm check-all
```

### Environment-Specific Development

```bash
# Development (localhost with hot reload)
pnpm dev

# Staging (preview deployments)
pnpm build && pnpm start

# Production (optimized build)
NODE_ENV=production pnpm build
```

## 🧪 Testing

### Component Testing
```bash
# Run component tests
pnpm test

# Run tests in watch mode
pnpm test:watch

# Run tests with coverage
pnpm test:coverage
```

### End-to-End Testing
```bash
# Run E2E tests
pnpm test:e2e

# Run E2E tests in UI mode
pnpm test:e2e:ui
```

### Manual Testing Checklist
- [ ] Event creation and publishing flow
- [ ] WebRTC streaming functionality  
- [ ] Event registration and NFT minting
- [ ] Farcaster frame sharing
- [ ] Mobile responsiveness
- [ ] Cross-browser compatibility

## 🚀 Deployment

### Vercel Deployment (Recommended)

1. **Connect Repository**
   ```bash
   # Connect your GitHub repository to Vercel
   vercel --prod
   ```

2. **Environment Variables**
   Set all required environment variables in Vercel dashboard:
   - OnchainKit API keys
   - Farcaster configuration
   - Redis connection details
   - IPFS/Pinata credentials
   - Smart contract addresses

3. **Domain Configuration**
   - Configure custom domain in Vercel settings
   - Update `NEXT_PUBLIC_URL` environment variable
   - Verify frame metadata endpoints work correctly

### Alternative Deployment Options

#### Netlify
```bash
# Build command
pnpm build

# Publish directory
out/

# Environment variables
# Set all NEXT_PUBLIC_* variables in Netlify dashboard
```

#### Docker Deployment
```bash
# Build Docker image
docker build -t revent-frontend .

# Run container
docker run -p 3000:3000 --env-file .env.local revent-frontend
```

#### Self-hosted
```bash
# Build for production
pnpm build

# Start production server
pnpm start

# Or use PM2 for process management
pm2 start npm --name "revent" -- start
```

## 🔧 Configuration Guide

### Smart Contract Integration

Update contract configuration in `lib/contract.ts`:

```typescript
export const eventAddress = "0xYourContractAddress" as const;
export const eventAbi = [...] as const; // Your contract ABI
```

### Streaming Configuration

Configure streaming endpoints in environment variables:

```bash
# WHIP endpoint for WebRTC streaming
NEXT_PUBLIC_WHIP_URL=http://your-server:8889

# HLS endpoint for adaptive streaming  
NEXT_PUBLIC_HLS_URL=http://your-server:8888
```

### Map Configuration

Set up Mapbox for interactive maps:

```bash
# Get API key from https://mapbox.com
NEXT_PUBLIC_MAPBOX_ACCESS_TOKEN=pk.your_mapbox_token
```

### The Graph Integration

Configure The Graph endpoint for event data:

```bash
# Your deployed subgraph endpoint
NEXT_PUBLIC_GRAPH_URL=https://api.studio.thegraph.com/query/your-subgraph
```

## 📊 Performance Optimization

### Bundle Analysis
```bash
# Analyze bundle size
pnpm analyze

# Check for unused dependencies
pnpm depcheck
```

### Performance Monitoring
- **Core Web Vitals**: Monitored via Vercel Analytics
- **Real User Monitoring**: Integrated performance tracking
- **Error Tracking**: Sentry integration for error monitoring
- **Stream Quality**: WebRTC connection quality metrics

### Optimization Strategies
- **Code Splitting**: Automatic route-based splitting
- **Image Optimization**: Next.js Image component with WebP support
- **Lazy Loading**: Components and routes loaded on demand
- **Edge Functions**: API routes deployed to edge locations
- **CDN Integration**: Static assets served from global CDN

## 🔍 Debugging

### Development Tools
```bash
# Enable debug logging
DEBUG=revent:* pnpm dev

# WebRTC debugging
# Open Chrome DevTools > Console > Set log level to "Verbose"

# Network debugging
# Use browser DevTools Network tab to inspect API calls
```

### Common Issues

#### Streaming Problems
- Check WebRTC connectivity and firewall settings
- Verify WHIP endpoint is accessible
- Test with different browsers and devices

#### Contract Interaction Issues  
- Verify contract address and ABI are correct
- Check network configuration (mainnet vs. testnet)
- Ensure wallet is connected to correct network

#### Farcaster Frame Issues
- Validate frame metadata with Frame Validator
- Check `.well-known/farcaster.json` endpoint
- Verify account association configuration

## 🤝 Contributing

### Development Workflow
1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Make your changes and test thoroughly
4. Run quality checks: `pnpm check-all`
5. Commit with conventional commits: `git commit -m 'feat: add amazing feature'`
6. Push and create a Pull Request

### Code Standards
- **TypeScript**: Use strict TypeScript for all new code
- **ESLint**: Follow configured linting rules
- **Prettier**: Use automatic code formatting
- **Conventional Commits**: Use semantic commit messages
- **Component Testing**: Add tests for new components

---

For more information about the complete Revent platform, see the [main README](../README.md).

## 📚 Learn More

### Platform Documentation
- [Revent Platform Overview](../README.md) - Complete platform documentation
- [Smart Contracts](../contract/README.md) - Contract deployment and interaction
- [ENS Integration](../durin/README.md) - Decentralized naming infrastructure

### Framework Documentation  
- [Next.js Documentation](https://nextjs.org/docs) - React framework for production
- [OnchainKit Documentation](https://onchainkit.xyz) - Coinbase's onchain toolkit
- [MiniKit Documentation](https://docs.base.org/builderkits/minikit/overview) - Base's social app toolkit
- [Farcaster Frames](https://docs.farcaster.xyz/reference/frames/spec) - Frame specification and development

### Web3 Resources
- [Base Network](https://base.org) - Layer 2 blockchain documentation  
- [Wagmi Documentation](https://wagmi.sh) - React hooks for Ethereum
- [Viem Documentation](https://viem.sh) - TypeScript Ethereum interface
- [The Graph Documentation](https://thegraph.com/docs) - Decentralized indexing protocol

### Streaming & Media
- [WebRTC Documentation](https://webrtc.org) - Real-time communication APIs
- [WHIP Protocol](https://datatracker.ietf.org/doc/draft-ietf-wish-whip/) - WebRTC-HTTP ingestion protocol
- [HLS Documentation](https://developer.apple.com/streaming/) - HTTP Live Streaming
