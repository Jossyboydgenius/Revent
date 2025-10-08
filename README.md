# Revent 🎬
### Tokenizing Events & SocialFi Platform

<div align="center">
  <img src="frontend/public/hero.png" alt="Revent Hero" width="400"/>
  
  [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
  [![Built on Base](https://img.shields.io/badge/Built%20on-Base-blue)](https://base.org)
  [![Next.js](https://img.shields.io/badge/Next.js-15-black)](https://nextjs.org)
  [![Solidity](https://img.shields.io/badge/Solidity-^0.8.19-blue)](https://soliditylang.org)
</div>

Revent is a decentralized live streaming and events platform that revolutionizes how we create, discover, and participate in events. Built on Base with Farcaster integration, it combines the power of blockchain technology with real-time streaming to create immersive social experiences.

## ✨ Features

### 🎥 Live Streaming
- **WebRTC Integration**: Real-time peer-to-peer streaming using WHIP protocol
- **Multi-Platform Support**: Stream to multiple platforms simultaneously
- **Interactive Viewer Experience**: Real-time chat and engagement tools
- **HLS Streaming**: Adaptive bitrate streaming for optimal viewing experience

### 🗺️ Interactive Event Discovery
- **Live Event Map**: Discover nearby events on an interactive map
- **Real-time Updates**: See live events and viewer counts in real-time
- **Location-based Search**: Find events by location and category
- **Event Filtering**: Filter by type, time, and availability

### 🎫 Decentralized Event Management
- **Smart Contract Events**: Create and manage events on-chain
- **NFT Ticketing**: Tickets as NFTs with unique confirmation codes
- **Registration Management**: Handle attendee registration and capacity limits
- **Revenue Sharing**: Automated fee distribution and revenue sharing

### 🌐 ENS Integration
- **ENS Subnames**: Leverage Durin for L2 ENS subname management
- **IPFS Metadata**: Decentralized storage for event metadata
- **Resolver Integration**: Custom L1 resolver for cross-chain name resolution
- **Gateway Server**: CCIP Read implementation for seamless resolution

### 📱 Farcaster Integration
- **Frame SDK**: Native Farcaster frame support
- **Account Association**: Connect events to Farcaster profiles
- **Social Features**: Share events and engage with the community
- **Notification System**: Real-time notifications via Farcaster

## 🏗️ Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│    Frontend     │    │   Smart         │    │     ENS         │
│   (Next.js)     │◄──►│  Contracts      │◄──►│   Integration   │
│                 │    │   (Solidity)    │    │   (Durin)       │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Streaming     │    │   The Graph     │    │     IPFS        │
│   (WebRTC/HLS)  │    │   Protocol      │    │   Storage       │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## 🚀 Quick Start

### Prerequisites
- Node.js 18+ and pnpm
- Foundry for smart contract development
- Git

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/Jossyboydgenius/Revent.git
   cd revent
   ```

2. **Install dependencies**
   ```bash
   # Frontend
   cd frontend && pnpm install
   
   # Smart Contracts
   cd ../contract && forge install
   
   # ENS Integration
   cd ../durin && forge install
   cd gateway && bun install
   ```

3. **Environment Setup**
   ```bash
   # Copy environment files
   cp frontend/.env.example frontend/.env.local
   cp contract/.env.example contract/.env
   cp durin/.env.example durin/.env
   ```

4. **Start Development**
   ```bash
   # Terminal 1: Frontend
   cd frontend && pnpm dev
   
   # Terminal 2: Streaming Server (optional)
   cd nginx-rtmp && docker-compose up
   ```

Visit `http://localhost:3000` to see the application running.

## 📦 Project Structure

### `/frontend` - Next.js Application
The main user interface built with Next.js, OnchainKit, and MiniKit for Farcaster integration.
- **Real-time event discovery and streaming**
- **Interactive maps and social features**
- **Wallet integration and Web3 functionality**

### `/contract` - Smart Contracts
Solidity contracts for event management, ticketing, and revenue sharing.
- **Event lifecycle management**
- **NFT-based ticketing system**
- **Modular architecture with upgradeable components**

### `/durin` - ENS Integration
L2 ENS subname management system for decentralized naming.
- **L1/L2 resolver architecture**
- **Registry factory for deployment**
- **CCIP Read gateway server**

### `/nginx-rtmp` - Streaming Infrastructure
Docker-based RTMP server for live streaming capabilities.
- **HLS streaming support**
- **WebRTC integration**
- **Multi-format output**

## 🛠️ Development

### Smart Contract Development
```bash
cd contract

# Compile contracts
forge build

# Run tests
forge test

# Deploy to testnet
forge script script/DeployStreamEvents.s.sol --rpc-url <RPC_URL> --broadcast

# Deploy with custom configuration
forge script script/DeployStreamEventsCustom.s.sol --rpc-url <RPC_URL> --broadcast
```

### Frontend Development
```bash
cd frontend

# Development server
pnpm dev

# Build for production
pnpm build

# Run linting
pnpm lint
```

### ENS Integration
```bash
cd durin

# Deploy L2 contracts
./bash/DeployL2Contracts.sh

# Deploy L1 resolver
./bash/DeployL1Resolver.sh

# Start gateway server
cd gateway && bun dev
```

## 🌟 Key Technologies

- **Base Blockchain**: L2 solution for fast, low-cost transactions
- **Farcaster**: Decentralized social network integration
- **OnchainKit**: Coinbase's toolkit for onchain experiences
- **The Graph Protocol**: Decentralized indexing and querying
- **IPFS**: Distributed storage for metadata and content
- **WebRTC**: Real-time peer-to-peer communication
- **ENS**: Ethereum Name Service for decentralized naming

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Links

- **Website**: [revent.social](https://revent.social)
- **Documentation**: [docs.revent.social](https://docs.revent.social)
- **Twitter**: [@ReventSocial](https://twitter.com/ReventSocial)
- **Discord**: [Join our community](https://discord.gg/revent)

---
