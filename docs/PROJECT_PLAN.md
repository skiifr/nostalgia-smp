# NOSTALGIA SMP — Product and Gameplay Plan

**Status:** Draft specification, version 0.1  
**Platform target:** Minecraft Bedrock  
**Experience:** Single-player-first persistent Economy SMP

## 1. Product vision

Recreate the atmosphere and progression of classic Minecraft economy servers from roughly 2012–2016, but make the world feel populated and competitive when only one human player is present.

The player should be able to log in, see familiar player-like competitors, gather resources, visit spawn, sell goods, browse listings, negotiate trades, take risks, open crates and compete for wealth and influence.

This is not intended to be a normal village/NPC survival add-on. The simulated population must participate in a coherent economy.

## 2. Core gameplay loop

1. Enter the persistent survival world.
2. Gather resources through mining, farming, hunting and exploration.
3. Visit spawn or use the remote shop.
4. Sell resources to the server shop or trade with simulated competitors.
5. Reinvest in tools, equipment, supplies and market opportunities.
6. Negotiate or compete against other merchants.
7. Use keys and casino wagers as optional high-risk reward loops.
8. Improve wealth and market-share rankings.
9. Leave the world; the economy advances within defined offline limits.
10. Return to a changed but economically consistent market.

The loop must remain enjoyable even if the player ignores gambling, crates or rankings.

## 3. Simulated competitors

### 3.1 Identity and profile

Each agent should have a stable unique ID and a persistent profile containing:

- Display name and player-style appearance/skin reference.
- Profession and specialisation.
- Starting and current balance.
- Inventory and reserved inventory.
- Personality and risk tolerance.
- Buy/sell preferences and price limits.
- Market reputation.
- Activity schedule and last simulation timestamp.
- Transaction history reference.
- Optional home/base location and faction membership in a later phase.

Names must be unique within a world. Profiles must survive reloads even if their visible entities are unloaded or respawned.

### 3.2 Initial professions

| Profession | Primary economic role | Initial behaviour |
|---|---|---|
| Miner | Ore and stone supply | Produces a bounded resource allowance and lists stock |
| Farmer | Food and crop supply | Produces crops/food and responds to demand |
| Merchant | Market liquidity | Buys and resells goods within capital and inventory limits |
| Hunter | Mob drops and materials | Later expansion |
| Blacksmith | Tools and equipment | Later expansion |
| General trader | Opportunistic trading | Later expansion |

The MVP starts with five visible agents across miner, farmer and merchant roles. The broader target population can grow after performance testing.

### 3.3 Agent behaviour boundaries

- Agents are economic simulations, not real player clients.
- Do not claim a bot mined a physical block unless the implementation actually performed that action.
- Abstract production must be recorded as a bounded economic event with a source and quantity.
- Bots cannot spend more than their available balance.
- Bots cannot list more items than their unreserved inventory.
- Listings reserve inventory to prevent double-selling.
- Agent-to-agent trades must transfer actual ledgered balances and inventory.
- Bot schedules should create believable activity without requiring constant entity ticking.
- Visible entities are presentation; persistent profiles and economic state are authoritative.

## 4. Trading and market

### 4.1 Required actions

- Create listing.
- Cancel listing.
- Browse and filter listings.
- Submit offer.
- Counter-offer.
- Accept or reject offer.
- Expire offer.
- Complete transaction.
- View trade history.

### 4.2 Market competition

Merchants should have different capital, stock, specialisation and pricing preferences. Prices may respond to supply and demand, but must not fluctuate randomly without a reason.

The market engine should account for:
- Recent completed volume.
- Current active listings.
- Available supply.
- Demand signals / purchase history.
- Merchant costs and minimum acceptable prices.
- Maximum reasonable price movement per simulation interval.
- Price floors and ceilings to prevent runaway values.

No agent should be able to coordinate with or inspect hidden player state unless explicitly designed as a game mechanic.

### 4.3 Physical and remote shops

The physical shop is located at spawn and offers better prices. The remote shop is convenient but less favourable.

The price difference must be visible to the player before confirming a transaction. The shop is a distinct counterparty with a controlled budget or explicit currency sink/source policy; it must not silently mint unlimited money.

## 5. Economy rules

### 5.1 Authoritative ownership

There is one authoritative record for:
- Currency balances.
- Item quantities.
- Reserved quantities.
- Listings and offers.
- Completed transactions.
- Fees, rewards and sinks.
- Agent and player account identities.

UI, entities and menus are clients of this state; they do not independently decide whether a transaction is valid.

### 5.2 Transaction invariants

Every completed transaction must be atomic from the game's perspective:

1. Validate participants and state.
2. Validate balance and item ownership.
3. Validate listing/offer status and expiry.
4. Reserve or debit the required assets.
5. Apply all transfers and fees.
6. Record the transaction.
7. Commit the new state.

If validation or persistence fails, do not partially apply the trade. Prevent duplicate processing when a menu action or callback fires more than once.

### 5.3 Money sources and sinks

All currency creation and removal must have an explicit reason.

Potential sources:
- Controlled server-shop purchases of eligible goods.
- Defined job rewards, if later introduced.
- Crate/casino rewards, funded through an explicit reward budget or designed sink/source policy.

Potential sinks:
- Shop purchases.
- Listing or transaction fees.
- Gambling losses.
- Crate key purchases.
- Repair, service or convenience fees in later versions.

The project must track net currency creation and destruction. Do not let agent production or offline simulation generate unlimited money.

### 5.4 Single-player balancing

The economy cannot rely on a large real player base. Agent demand, shop budgets and production rates must create enough liquidity for the player to sell goods, while preserving scarcity and meaningful price competition.

Start with conservative fixed baselines. Add dynamic pricing only after transaction telemetry and repeatable tests exist.

## 6. Offline simulation

The world does not need to keep every bot entity loaded while closed.

On world load:
1. Read the last successful simulation timestamp.
2. Validate saved state and schema version.
3. Calculate elapsed time with a maximum catch-up window.
4. Advance agent schedules in bounded time steps.
5. Apply production, consumption, offers and trades under resource limits.
6. Record economic events and update rankings.
7. Save the resulting state and new timestamp.

Requirements:
- Cap elapsed time processed per load.
- Cap each agent's production and number of simulated transactions.
- Use deterministic inputs where practical so the same saved state and elapsed interval produce predictable outcomes.
- Prevent duplicate catch-up after a crash or interrupted save.
- Provide a migration path for saved schema changes.
- Define behaviour for device clock changes and corrupted timestamps.

Offline progression is an economic simulation, not a promise that all agents physically move around the world while the game is closed.

## 7. Crates and keys

Target design:
- Common, Rare and Legendary keys.
- Weighted reward tables.
- Reward preview and clear probability communication where appropriate.
- Reward pools may include tools, armour, currency and server buffs.
- Every key consumption and reward must be recorded.
- Every currency reward must use the economy transaction system.
- Item rewards must be granted exactly once, including after interrupted UI or save operations.

Crates are a later phase, after inventory and currency transactions are stable.

## 8. Gambling

Initial concept: risk-based wagers against simulated competitors. Candidate games include coin flip, slots and blackjack; exact launch games remain to be decided.

Rules:
- Every wager is reserved/debited through the economy.
- Payouts are validated and recorded.
- Game outcome generation must be fair, testable and not manipulable through repeated UI callbacks.
- Define house/bot bankroll and payout limits.
- Avoid unbounded currency creation.
- Show wager and payout rules before confirmation.

Casino mechanics are later-phase systems and must share the same economy ledger.

## 9. Competition and rankings

Two primary competitive dimensions:

### Wealth leaderboard
- Net worth calculation must define treatment of inventory, equipment, reserved assets and debts.
- Rankings update after committed transactions and periodic valuation.
- Avoid double-counting listed items.

### Market dominance
Possible measures:
- Share of completed market volume by category.
- Active stock share.
- Number/value of completed trades.
- Reputation or reliability.

The exact formula must be chosen before implementation. Do not equate largest inventory with market dominance automatically.

## 10. Out of scope for the MVP

- Real connected bot accounts.
- Full autonomous human-like survival AI.
- Factions, land claims, PvP wars and territory.
- Ranks, kits and permissions.
- Large-scale pathfinding or hundreds of visible entities.
- Full casino suite.
- Crates and complex reward progression.
- Java Edition support.
- Realms compatibility guarantees for beta APIs.

These can be evaluated after the economy foundation works on the target Bedrock client.

## 11. Product acceptance standard

A feature is not complete because its menu appears. It is complete when:
- Its state is validated.
- Its economic effects are ledgered.
- It survives world reload.
- Failure and duplicate actions are safe.
- It passes target-device testing.
- Its limitations are documented.
