---
icon: store
---

# Marketplace

### Overview

The Ebonquill Marketplace enables players to trade game assets on-chain via web browser and in‑game, sharing a single order book and settlement layer. All listings, purchases, and transfers are executed by audited smart contracts on BSC and settled in $EPIC.

### Access

- **Web**: Connect wallet (MetaMask, WalletConnect). Create, list, buy, and manage assets.
- **In‑Game**: Integrated UI mirrors the same order book. Actions are signed in‑client and relayed on‑chain. Custodial session keys are supported for convenience; the underlying assets remain in the player’s wallet or escrow as appropriate.

### Supported Assets and Standards

- **Dungeon Keys (ERC‑721)**: The NFT that represents dungeon ownership.
- **Heroes (ERC‑721)**: Unique playable characters with class/role metadata.
- **Gear (ERC‑1155/721)**: Items with roll-based stats. Commoditized variants may use 1155; uniques use 721.
- **Potions (ERC‑1155)**: Consumables traded as stackable tokens.
- **Skills (ERC‑721)**: Legendary skills minted as NFTs.

All assets include standardized metadata for rarity, season, item type, and relevant traits to enable filtering and discovery.

### Fees and Royalties

- **Marketplace fee**: 2.5% in $EPIC on the seller’s proceeds, routed to the DAO Treasury.
- **Creator/system royalties**: Optional per‑collection royalty up to 2% (configurable per season/collection; disclosed on the collection page). Royalties are paid in $EPIC on settlement.

### Listing Types

- **Fixed‑Price**: Seller sets a price in $EPIC.
- **Dutch Auction**: Price decays from a start to a reserve over time.
- **English Auction**: Highest bidder wins at end time; optional reserve.

Listings are escrowed by the marketplace contract until sale/cancel. Buyers pay in $EPIC; on success, assets transfer to buyer and proceeds (minus fees/royalties) to seller.

### Settlement and Escrow

- Assets are transferred to secure escrow on listing and released on sale or cancellation.
- Payments are held in contract and atomically distributed to the seller, DAO Treasury, and royalty recipient on settlement.
- Failed or expired auctions auto‑refund bidders in $EPIC.

### Discovery and UX

- Filters: category, season, rarity, tier, stat ranges, price, auction type.
- Saved searches and alerts.
- Collection/creator pages with verified badges.

### Anti‑Abuse and Integrity

- Rate limits on list/cancel cycles to reduce wash trading.
- Minimum tick/step and minimal list duration on auctions.
- Fraud heuristics (multi‑account, circular trading) coordinated with Oracle’s detection; suspicious trades may be flagged for review.
- Blacklist for compromised or malicious contracts/collections.

### Interactions with Game Economy

- Marketplace trades are independent of dungeon **taxation** (which applies to in‑dungeon earnings only).
- $EPIC spent in marketplace does not count toward Oracle conversion limits.
- Asset transfers update game state (e.g., dungeon ownership) in near‑real‑time via indexers.

### Developer Notes

- Indexing: Public read API and push webhooks for new listings, sales, and transfers.
- Standards: ERC‑721 and ERC‑1155 with metadata normalization; EIP‑2981 royalties where applicable.
- Security: Audited contracts, pause/upgrade mechanisms governed by the DAO.
