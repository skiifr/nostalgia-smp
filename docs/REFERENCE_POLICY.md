# NOSTALGIA SMP — Reference and Licensing Policy

## Purpose

Third-party projects may inform feature design, UX patterns and technical feasibility. Their code, assets, names and documentation are not automatically available for reuse.

## Known references supplied during planning

These references were identified in planning notes and have not yet been independently re-audited for current availability or licence changes.

| Reference | Intended use | Reuse boundary |
|---|---|---|
| Sovereign Economy (CurseForge) | Feature and UX reference for shops, bank, jobs, player trading and auction house | Reported as All Rights Reserved; do not copy code or assets |
| Gacha Crate PRO | Crate tiers, keys and weighted reward design reference | Do not assume integration with our currency or permission to reuse assets/code |
| PocketMine Casino / DeluxeCoinFlip | Casino game rules and payout design reference | PocketMine plugin implementation is not a Bedrock add-on |
| JadenRazo ServerPlugins | Java Paper casino design reference | Paper plugin implementation is not a Bedrock add-on |
| Endstone JWEconomy / JWMarket | Economy and escrow/order-book design reference | Requires Endstone/BDS environment; not assumed usable in a normal world or Realms |
| AI Players Marketplace add-on | Prior experiment with player-like agents | User reported mobile crash; do not recommend repurchase without verified fix |

## Rules

1. Read the exact licence for each repository, package, model, texture, font and sound before reuse.
2. If a project is All Rights Reserved or has no clear reuse permission, use it only as a high-level design reference.
3. Do not copy source code, distinctive text, artwork, textures or other protected assets from restricted projects.
4. Prefer original implementation based on documented behaviour and independently designed data structures.
5. Keep attribution and licence notices for dependencies that permit reuse.
6. Check dependency licences for transitive packages before release.
7. Marketplace availability does not imply permission to extract or redistribute assets.
8. Do not claim compatibility based solely on a listing or marketing description; verify in the target client.

## Design patterns that may be independently implemented

- Shop and bank account concepts.
- Listings and auction/order-book workflows.
- Escrow as a general transaction safety pattern.
- Weighted reward tables.
- Coin flip, slots and blackjack rules.
- Payout caps, house bankroll and wager settlement.
- Supply/demand price models.

Implementations must be original and integrated with NOSTALGIA SMP's authoritative economy ledger.

## Compatibility boundary

PocketMine, Paper, Endstone and Bedrock add-ons are different runtime environments. Their code cannot be assumed to run inside a normal Bedrock world. A design pattern may be portable even when its plugin implementation is not.

## Review checklist for every new dependency

- [ ] Exact source repository/package identified.
- [ ] Licence verified at the version being used.
- [ ] Runtime and Minecraft version compatibility verified.
- [ ] Transitive dependency/licence implications checked.
- [ ] Mobile behaviour tested if used in the client.
- [ ] Dependency pinned and documented.
- [ ] Removal or replacement path understood.
