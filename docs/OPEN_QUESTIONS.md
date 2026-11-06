# NOSTALGIA SMP — Open Questions and Assumptions

This document prevents unknowns from silently becoming implementation requirements. Update each item when a decision is made and record the date/context in the decision log.

## A. Product decisions still required

| ID | Question | Current assumption | Why it matters |
|---|---|---|---|
| P-01 | Exact Bedrock version to target? | Latest stable version supported by the player's iPhone at implementation start | Determines Script API and pack compatibility |
| P-02 | Is the first release iPhone-only or iPhone + Windows Bedrock? | iPhone is mandatory test target; Windows may be used for development | Affects build/test workflow |
| P-03 | What should the currency be called? | Temporary generic currency | UI and economy presentation |
| P-04 | What should one unit of currency represent? | Integer minor units internally | Prevents floating-point errors |
| P-05 | How many total agents should the first public build support? | Five visible agents; broader target deferred | Performance and balance |
| P-06 | Should agent identities be a fixed cast or procedurally generated? | Fixed initial cast | Skin/name consistency |
| P-07 | What exact skin source/asset workflow is acceptable? | Original or properly licensed assets | Resource-pack and redistribution rights |
| P-08 | Should agents physically gather in the MVP or use abstract production? | Abstract, bounded production first | Avoids expensive and fragile AI |
| P-09 | Which items are tradable in Phase 1? | Small allowlist of common resources | Prevents unsupported item metadata issues |
| P-10 | What is the remote-shop price penalty? | To be tuned in playtesting | Core convenience-versus-value balance |
| P-11 | What are the starting balances and inventories? | Not yet defined | Initial market liquidity |
| P-12 | What is the offline catch-up cap? | Conservative cap to be set after tests | Prevents runaway simulation |
| P-13 | Which gambling game launches first? | Undecided; casino is later phase | Rules, odds and payout policy |
| P-14 | How are crate keys earned or purchased? | Undecided; must define money source/sink | Economy integrity |
| P-15 | What is market dominance? | Likely category market-share metric | Ranking formula |
| P-16 | Is Realms support a requirement? | Not promised; stable APIs preferred | API and hosting constraints |
| P-17 | Should Java Edition ever be supported? | Not in current scope | Would require a separate implementation |
| P-18 | Does the project need multiplayer compatibility later? | Single-player-first; avoid unnecessary assumptions | Account and concurrency model |

## B. Technical verification required

| ID | Item to verify | Acceptance evidence |
|---|---|---|
| T-01 | create-mc-bedrock scaffold | Reproducible clean build and generated pack |
| T-02 | Exact stable Script API version | Manifest and API docs for target game version |
| T-03 | Chest-UI compatibility | Pinned package, licence and working mobile UI test |
| T-04 | @tendrock/database compatibility | Pinned package, licence, persistence test and supported runtime |
| T-05 | Dynamic Property limits and types | Exact current documentation and measured tests |
| T-06 | Text/property byte limits | Confirm for the exact property type and target version |
| T-07 | Custom humanoid entity skin workflow | Working resource pack on iPhone |
| T-08 | Entity nametag and equipment presentation | In-client test |
| T-09 | Script scheduling and world-load lifecycle | In-client close/reopen test |
| T-10 | SimulatedPlayer availability | Separate experimental proof of concept only |
| T-11 | UI library callbacks and form limits | Mobile test, including repeated submissions |
| T-12 | Build/import process on iOS | Documented reproducible workflow |

## C. Important assumptions — not yet proven

1. A custom humanoid entity can provide a sufficiently convincing player-like appearance on the target Bedrock client.
2. A custom entity is more suitable for the stable first implementation than SimulatedPlayer.
3. Offline catch-up can be performed reliably using persisted timestamps and bounded deterministic steps.
4. The candidate scaffold and UI/database packages are compatible with the selected Bedrock and Script API versions.
5. A small simulated population can create enough liquidity for a satisfying single-player economy.
6. Physical and remote shop price differences can create a meaningful travel incentive without making remote access frustrating.

These assumptions must be tested; they are not guarantees.

## D. Risk register

| Risk | Impact | Mitigation |
|---|---|---|
| Script API breaking changes | Pack stops loading | Pin API versions; test target client after updates |
| Mobile performance | Crashes or poor frame rate | Start with five agents; profile before scaling |
| Entity appearance limitations | Bots look unlike players | Prototype skin/model early |
| Persistence corruption | Lost or duplicated value | Journal, snapshots, schema validation and recovery tests |
| Economy inflation | Progression becomes meaningless | Explicit sources/sinks and bounded production |
| UI library incompatibility | Rework or broken menus | Verify before adopting; isolate behind adapter |
| Beta SimulatedPlayer instability | World/API incompatibility | Keep out of stable dependency path |
| Third-party licensing | Cannot legally reuse code/assets | Reference policy and licence review |
| Scope expansion | Delayed playable build | Phase gates; no casino/crates before economy works |

## E. Decision log

| Date | Decision | Status |
|---|---|---|
| 2026-11-06 | Bedrock-first, single-player Economy SMP | Confirmed |
| 2026-11-06 | Player-style bot appearance | Confirmed |
| 2026-11-06 | Chat, physical interaction and menus | Confirmed |
| 2026-11-06 | Listings, negotiation and bot-vs-bot competition | Confirmed |
| 2026-11-06 | Physical spawn shop plus remote shop; physical pricing better | Confirmed |
| 2026-11-06 | Common, Rare and Legendary crates | Confirmed |
| 2026-11-06 | Risk-based wagers against simulated competitors | Confirmed |
| 2026-11-06 | Wealth and market-dominance competition | Confirmed |
| 2026-11-06 | Classic grind with competitive pressure | Confirmed |
| 2026-11-06 | Offline economic activity continues | Confirmed |
| 2026-11-06 | Custom player-style entity is stable first path; SimulatedPlayer experimental | Proposed implementation choice |
| 2026-11-06 | New standalone repository | Confirmed |

## F. How to resolve questions

Resolve in this order:
1. Target Bedrock version and actual test device.
2. Scaffold, Script API and pack import.
3. Entity skin/model proof of concept.
4. Persistence mechanism and size limits.
5. Currency unit, starting balance and MVP item allowlist.
6. Shop price model and bot starting capital.
7. Offline catch-up limits.
8. Remaining crate, casino and ranking design choices.

Do not block Phase 0 on decisions that only affect later phases.
