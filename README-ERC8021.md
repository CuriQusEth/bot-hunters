# 🎯 Bot Hunter - ERC-8021 Transaction Attribution

## Quick Start Guide

Bot Hunter implements **ERC-8021 Transaction Attribution** to enable transparent tracking and reward distribution for blockchain transactions.

## 🚀 What's Implemented

### Core Files

```
src/
├── types/erc8021.ts              # Type definitions
├── lib/
│   ├── erc8021.ts                # Core utilities (encoding/parsing)
│   ├── attribution.ts            # Bot Hunter attribution helpers
│   └── erc8021-examples.ts       # 10+ usage examples
└── docs/
    └── ERC8021-ATTRIBUTION.md    # Full documentation
```

### Attribution Code

Bot Hunter uses the attribution code: **`bc_9b19fklw`**

This code is registered in Base's canonical Code Registry and links to:
- Payout address for reward distribution
- Metadata URI with app information
- Validation and registration status

## 📝 Basic Usage

### 1. Add Attribution to Any Transaction

```typescript
import { addBotHunterAttribution } from '@/lib/attribution';

// Original transaction
const calldata = '0x095ea7b3...';

// Add Bot Hunter attribution
const attributed = addBotHunterAttribution(calldata);

// Use in transaction
await wallet.sendTransaction({
  to: contractAddress,
  data: attributed,
});
```

### 2. Multi-Entity Attribution (App + Wallet)

```typescript
import { createMultiAttribution, appendAttribution } from '@/lib/attribution';

// Attribute to both Bot Hunter and Coinbase Wallet
const config = createMultiAttribution('coinbase');
const attributed = appendAttribution(calldata, config);
```

### 3. Parse Attribution from Transactions

```typescript
import { parseAttribution } from '@/lib/erc8021';

const parsed = parseAttribution(txData);
console.log(parsed?.codes); // ['bc_9b19fklw']
```

## 🎮 Game Integration Examples

### NFT Reward Claims

```typescript
import { prepareAttributedCall } from '@/lib/attribution';

const call = prepareAttributedCall(
  nftContract,
  mintCalldata
);

await wallet.sendTransaction(call);
```

### Smart Account (ERC-4337)

```typescript
import { prepareUserOpWithAttribution } from '@/lib/attribution';

const userOp = {
  sender: account,
  callData: prepareUserOpWithAttribution(calldata),
  // ... other fields
};
```

### ERC-5792 wallet_sendCalls

```typescript
import { createDataSuffixCapability } from '@/lib/erc8021';
import { BOT_HUNTER_ATTRIBUTION } from '@/lib/attribution';

const capability = createDataSuffixCapability(BOT_HUNTER_ATTRIBUTION);

await provider.request({
  method: 'wallet_sendCalls',
  params: [{
    capabilities: capability,
    // ...
  }],
});
```

## 🔍 Attribution Format

### Schema 0 (Canonical Registry)

```
{txData} + {codes} + {codesLength} + {schemaId} + {ercSuffix}
```

Example:
```
0x095ea7b3... + "bc_9b19fklw" + 11 + 0 + 0x80218021802180218021802180218021
```

### Multi-Entity Example

```
{txData} + "bc_9b19fklw,coinbase" + 20 + 0 + 0x80218021...
```

## 📊 Benefits

✅ **Transparent Attribution**: Standardized format recognized across ecosystem
✅ **Reward Distribution**: Direct rewards to Bot Hunter treasury
✅ **Multi-Entity Support**: Credit both app and wallet simultaneously
✅ **Analytics**: Track game engagement and transaction sources
✅ **Interoperable**: Works with any protocol that supports ERC-8021

## 🎁 Reward Flow

```
Player Transaction (with attribution)
  ↓
Protocol Contract (parses attribution)
  ↓
Code Registry (returns payout address)
  ↓
Reward Distribution
  ↓
Bot Hunter Treasury
  ↓
Player Rewards / Game Development
```

## 📚 Resources

- **Full Documentation**: [`docs/ERC8021-ATTRIBUTION.md`](./docs/ERC8021-ATTRIBUTION.md)
- **Usage Examples**: [`src/lib/erc8021-examples.ts`](./src/lib/erc8021-examples.ts)
- **Type Definitions**: [`src/types/erc8021.ts`](./src/types/erc8021.ts)
- **ERC-8021 Spec**: [Ethereum Magicians Discussion](https://ethereum-magicians.org/t/erc-8021-transaction-attribution/25561)

## 🔧 Configuration

### Canonical Code Registry (Base)

- **Base Mainnet**: Chain ID 8453, Registry TBD
- **Base Sepolia**: Chain ID 84532, Registry TBD

### Farcaster Integration

baseBuilder configuration is already set in `public/.well-known/farcaster.json`:

```json
{
  "baseBuilder": {
    "ownerAddress": "0x29536D0bc1004ab274c4F0F59734Ad74D4559b7B"
  }
}
```

## 🧪 Testing

Run the examples to see ERC-8021 in action:

```typescript
import {
  exampleBasicAttribution,
  exampleMultiEntityAttribution,
  exampleParseAttribution,
  exampleCompleteFlow,
} from '@/lib/erc8021-examples';

// Test basic attribution
exampleBasicAttribution();

// Test multi-entity
exampleMultiEntityAttribution();

// Test parsing
exampleParseAttribution();

// Full flow demo
exampleCompleteFlow();
```

## 🚧 Future Enhancements

- [ ] Deploy Bot Hunter Code Registry on Base
- [ ] Automatic attribution for all game transactions
- [ ] Reward claiming UI for players
- [ ] Analytics dashboard for attribution metrics
- [ ] Multi-chain support beyond Base

## 📞 Support

Questions? Check:
1. [Full Documentation](./docs/ERC8021-ATTRIBUTION.md)
2. [Usage Examples](./src/lib/erc8021-examples.ts)
3. [ERC-8021 Discussion](https://ethereum-magicians.org/t/erc-8021-transaction-attribution/25561)

---

**Bot Hunter** - Protecting the feed with attributed transactions 🎯🤖
