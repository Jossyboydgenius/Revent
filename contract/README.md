# Revent Smart Contracts 📋

This directory contains the smart contracts powering the Revent platform - a decentralized event management and ticketing system built on Ethereum-compatible networks.

## 🏗️ Contract Architecture

The Revent smart contract system follows a modular architecture pattern, separating concerns into specialized contracts:

```
StreamEvents (Main Contract)
├── EventAttendees    (Attendee management)
├── EventQueries      (Data retrieval)
├── EventAdmin        (Administrative functions)
└── EventTickets      (NFT ticketing system)
```

### Core Contracts

#### `StreamEvents.sol`
The main facade contract that composes all event management features:
- **Event lifecycle management** (create, publish, start, end)
- **Attendee registration and management**
- **Revenue sharing and fee collection**
- **Administrative controls**

#### Modular Components

- **`events/Management.sol`**: Event creation, publishing, and lifecycle management
- **`events/Attendees.sol`**: Registration, attendance tracking, and participant management
- **`events/Queries.sol`**: Read-only functions for event data retrieval
- **`events/Admin.sol`**: Owner-only administrative functions and configurations
- **`events/Tickets.sol`**: NFT-based ticketing system with unique confirmation codes

## ✨ Key Features

### 🎫 Event Management
- **Draft → Published → Live → Ended** event lifecycle
- **IPFS metadata integration** for decentralized event information storage
- **Flexible capacity management** with attendee limits
- **Time-based event scheduling** with start/end time validation

### 💰 Revenue Model
- **Platform fee system** (configurable basis points)
- **Registration fee limits** (min/max constraints)
- **Automated fee distribution** to platform and event creators
- **Fee recipient management** for revenue collection

### 🎟️ NFT Ticketing
- **ERC721-based tickets** with unique token IDs
- **Confirmation codes** for event entry verification
- **Metadata integration** with event information
- **Transfer restrictions** to prevent ticket scalping

### 🔐 Access Control
- **Event creator permissions** for management functions
- **Owner-only administrative controls**
- **Multi-signature support** for critical operations
- **Pausable functionality** for emergency stops

## 🚀 Quick Start

### Prerequisites
- [Foundry](https://book.getfoundry.sh/) installed
- A funded wallet with ETH for gas fees
- RPC endpoint for target network

### Installation

```bash
# Clone and navigate to contract directory
cd contract

# Install dependencies
forge install

# Copy environment configuration
cp .env.example .env
```

### Environment Setup

Edit `.env` file with your configuration:

```bash
# Wallet Configuration
PRIVATE_KEY=your_private_key_here

# Network Configuration (choose one)
ETHEREUM_RPC_URL=https://eth-mainnet.g.alchemy.com/v2/your-api-key
BASE_RPC_URL=https://mainnet.base.org
SEPOLIA_RPC_URL=https://eth-sepolia.g.alchemy.com/v2/your-api-key
BASE_SEPOLIA_RPC_URL=https://sepolia.base.org

# Optional: Contract address for reconfiguration
CONTRACT_ADDRESS=0x...
```

## 🛠️ Development

### Building Contracts

```bash
# Compile all contracts
forge build

# Compile with specific Solidity version
forge build --use 0.8.19
```

### Testing

```bash
# Run all tests
forge test

# Run tests with gas reporting
forge test --gas-report

# Run specific test file
forge test --match-contract CounterTest

# Run with verbosity for detailed output
forge test -vvv
```

### Code Quality

```bash
# Format code
forge fmt

# Generate gas snapshots
forge snapshot

# Check coverage
forge coverage
```

## 🚀 Deployment

### Standard Deployment

Deploy with default configuration (2.5% platform fee, 0.001-1 ETH registration limits):

```bash
# Deploy to Base Sepolia (testnet)
forge script script/DeployStreamEvents.s.sol:DeployStreamEvents \
    --rpc-url $BASE_SEPOLIA_RPC_URL \
    --broadcast \
    --verify

# Deploy to Base Mainnet
forge script script/DeployStreamEvents.s.sol:DeployStreamEvents \
    --rpc-url $BASE_RPC_URL \
    --broadcast \
    --verify
```

### Custom Configuration Deployment

Deploy with custom platform fees and registration limits:

```bash
# Edit script/DeployStreamEventsCustom.s.sol to set:
# - customPlatformFee (in basis points, e.g., 100 = 1%)
# - customMinRegFee (minimum registration fee in wei)
# - customMaxRegFee (maximum registration fee in wei)

forge script script/DeployStreamEventsCustom.s.sol:DeployStreamEventsCustom \
    --rpc-url $BASE_SEPOLIA_RPC_URL \
    --broadcast \
    --verify
```

### Deployment with Testing

Run comprehensive deployment and testing in one script:

```bash
forge script script/DeployAndTest.s.sol:DeployAndTest \
    --rpc-url $BASE_SEPOLIA_RPC_URL \
    --broadcast
```

### Reconfiguring Existing Contracts

Update configuration of already deployed contracts:

```bash
# Set CONTRACT_ADDRESS in .env file first
export CONTRACT_ADDRESS=0x...

forge script script/ReconfigureStreamEvents.s.sol:ReconfigureStreamEvents \
    --rpc-url $BASE_SEPOLIA_RPC_URL \
    --broadcast
```

## 📊 Contract Interaction

### Using Cast (Foundry CLI)

```bash
# Read contract state
cast call $CONTRACT_ADDRESS "owner()" --rpc-url $BASE_RPC_URL
cast call $CONTRACT_ADDRESS "platformFee()" --rpc-url $BASE_RPC_URL

# Create an event
cast send $CONTRACT_ADDRESS \
    "createEvent(string,uint256,uint256,uint256,uint256)" \
    "QmYourIPFSHash" \
    1703980800 \
    1703984400 \
    100 \
    "1000000000000000" \
    --private-key $PRIVATE_KEY \
    --rpc-url $BASE_RPC_URL

# Register for an event
cast send $CONTRACT_ADDRESS \
    "registerForEvent(uint256)" \
    1 \
    --value 0.01ether \
    --private-key $PRIVATE_KEY \
    --rpc-url $BASE_RPC_URL
```

### Using Etherscan/Basescan

1. Navigate to your deployed contract on the block explorer
2. Go to the "Write Contract" tab
3. Connect your wallet
4. Interact with functions directly through the UI

## 🔍 Contract Verification

### Automatic Verification

Contracts are automatically verified when using the `--verify` flag with forge script.

### Manual Verification

```bash
# Verify on Etherscan
forge verify-contract \
    --chain-id 1 \
    --num-of-optimizations 200 \
    --watch \
    --constructor-args $(cast abi-encode "constructor()") \
    --etherscan-api-key $ETHERSCAN_API_KEY \
    $CONTRACT_ADDRESS \
    src/StreamEvents.sol:StreamEvents

# Verify on Basescan
forge verify-contract \
    --chain-id 8453 \
    --num-of-optimizations 200 \
    --watch \
    --constructor-args $(cast abi-encode "constructor()") \
    --etherscan-api-key $BASESCAN_API_KEY \
    $CONTRACT_ADDRESS \
    src/StreamEvents.sol:StreamEvents
```

## 📝 Event Lifecycle

### 1. Creation (DRAFT)
```solidity
uint256 eventId = streamEvents.createEvent(
    "QmIPFSHash",     // IPFS metadata hash
    startTime,        // Unix timestamp
    endTime,          // Unix timestamp  
    maxAttendees,     // Maximum capacity
    registrationFee   // Fee in wei
);
```

### 2. Publishing
```solidity
streamEvents.publishEvent(eventId);
```

### 3. Going Live
```solidity
streamEvents.startLiveEvent(eventId);
```

### 4. Registration
```solidity
streamEvents.registerForEvent{value: registrationFee}(eventId);
```

### 5. Ending Event
```solidity
streamEvents.endEvent(eventId);
```

## 🏷️ NFT Ticketing

Each registration creates an ERC721 NFT ticket with:
- **Unique token ID** linked to the event and attendee
- **Confirmation code** for entry verification
- **Event metadata** embedded in token URI
- **Transfer restrictions** to prevent unauthorized resale

```solidity
// Get ticket information
uint256 tokenId = streamEvents.getTicketTokenId(eventId, attendeeAddress);
string memory confirmationCode = streamEvents.getConfirmationCode(eventId, attendeeAddress);
string memory tokenURI = streamEvents.tokenURI(tokenId);
```

## 🔧 Configuration Parameters

### Platform Fees
- **Default**: 250 basis points (2.5%)
- **Range**: 0-1000 basis points (0-10%)
- **Configurable**: By contract owner only

### Registration Fees
- **Default Min**: 0.001 ETH
- **Default Max**: 1 ETH
- **Configurable**: By contract owner with validation

### Event Parameters
- **Start Time**: Must be in the future
- **End Time**: Must be after start time
- **Max Attendees**: Must be greater than current attendees
- **Registration Fee**: Must be within configured limits

## 🛡️ Security Features

### Access Control
- **Ownable Pattern**: Critical functions restricted to contract owner
- **Event Creator Permissions**: Event-specific actions limited to creators
- **Input Validation**: Comprehensive parameter validation

### Economic Security
- **Fee Limits**: Configurable min/max registration fees prevent abuse
- **Overflow Protection**: SafeMath patterns for arithmetic operations
- **Reentrancy Guards**: Protection against reentrancy attacks

### Upgrade Safety
- **Immutable Core Logic**: Core contract logic cannot be changed post-deployment
- **Configurable Parameters**: Only safe parameters can be updated
- **Event Validation**: Comprehensive validation prevents invalid state transitions

## 📈 Gas Optimization

- **Modular Architecture**: Reduces deployment and interaction costs
- **Efficient Storage**: Optimized struct packing and storage patterns
- **Batch Operations**: Where possible, operations are batched to reduce gas costs
- **View Functions**: Read operations optimized for minimal gas consumption

## 🔍 Monitoring & Analytics

### Events Emitted
- **EventCreated**: New event creation
- **EventStatusChanged**: Event state transitions
- **AttendeeRegistered**: New registrations
- **EventUpdated**: Event modifications
- **TicketMinted**: NFT ticket creation

### The Graph Integration
Events are indexed by The Graph Protocol for efficient querying:
- Real-time event data
- Historical analytics
- Attendee tracking
- Revenue metrics

## ⚡ Foundry Reference

### Build System
```bash
forge build              # Compile contracts
forge clean              # Clean build artifacts
forge fmt                # Format code
forge snapshot           # Gas snapshots
```

### Testing Framework  
```bash
forge test               # Run all tests
forge test -vvv          # Verbose output
forge coverage           # Coverage report
```

### Deployment Tools
```bash
forge script             # Run deployment scripts
forge verify-contract    # Verify on block explorers
cast                     # CLI for contract interaction
anvil                    # Local test network
```

### Documentation
- [Foundry Book](https://book.getfoundry.sh/) - Complete Foundry documentation
- [Forge Reference](https://book.getfoundry.sh/reference/forge/) - Forge command reference
- [Cast Reference](https://book.getfoundry.sh/reference/cast/) - Cast command reference

---

For more information about the Revent platform, see the [main README](../README.md).
