---
name: callsign-mint
description: Claim or forge a CALLSIGN codename NFT on BNB Chain for this agent's wallet. Use when the user asks their agent to get a callsign, codename, agent name, or @handle as an NFT.
---

# CALLSIGN — claim or forge your codename

CALLSIGN is a codename registry for AI agents on BNB Chain. A callsign is a unique
name (shown as `@name`) minted as an ERC-721 NFT, with the artwork generated fully
on-chain. CALLSIGN is an independent project; it is not affiliated with Binance or
SPACE ID — a SPACE ID Agent ID is used only as an eligibility check.

## Addresses (BNB Chain, chainId 56 — verify before every use)

- CALLSIGN contract: `0x4ebFd50398D9889A530F6DE326917BDEef4679f9`
- Agent ID registry (eligibility gate): `0x60b9262456de011f7d15a54cdd8c48baaa8b022f`
- Public RPC examples: `https://bsc-dataseed.binance.org`, `https://bsc-rpc.publicnode.com`

Only ever interact with the contract address above. The source is verified
(Sourcify exact match; BscScan) — if in doubt, read it before transacting.

## Rules (enforced by the contract)

- Names: lowercase `a-z` and `0-9` only, 1–15 characters, globally unique,
  first come first served. There is no reserve list — not even for the team.
- Eligibility: the wallet must hold a SPACE ID Agent ID. Check:
  `AgentIdRegistry.handleOf(wallet)` must return a non-empty string.
- Regular claim: names of 4–15 chars are claimed with `mint(name)`, free
  (gas only). **One regular claim per address, ever.** Burning your regular
  callsign does NOT restore the claim.
- Forging: names of 2–3 chars cannot be claimed, only forged with
  `refineMint(name, fuelIds)` paying exactly **0.001 BNB**:
  a 3-char name burns 2 regular callsigns as fuel; a 2-char burns 3.
  Fuel must be regular callsigns (4–15 chars) owned by the caller.
  Forging is unlimited per address, but a global quota applies:
  `quotaUsed(len) < quotaCap(len)` must hold (3-char quota starts at 1,000 and
  grows with total mints; 2-char quota = total regular mints / 40).
- **WARNING — tombstones:** burned names are gone forever and can never be
  re-registered by anyone. Never burn a name the user wants to keep.
- Single characters (a–z, 0–9) are locked in a vault as season prizes and
  cannot be claimed or forged.
- Your wallet's primary callsign is its most recent mint/forge:
  `Callsign.handleOf(wallet)`. Transferring the primary away clears it;
  receiving a token does not set it.

## Procedure — claim a regular callsign

1. Pick a name with the user (4–15 chars, lowercase a–z / 0–9).
2. Check availability: `Callsign.nameState(keccak256(utf8(name)))` must be `0`
   (free), and `Callsign.ownerOfHandle(name)` must be the zero address.
3. Check eligibility: `AgentIdRegistry.handleOf(wallet)` non-empty, and
   `Callsign.hasMintedRegular(wallet)` is `false`.
4. Check the registry is open: `Callsign.mintOpen()` is `true`.
5. Send `Callsign.mint(name)`. Confirm, then report `Callsign.handleOf(wallet)`
   and the token id to the user.

## Procedure — forge a short callsign (2–3 chars)

1. Pick the target name with the user; check it is free (step 2 above) and the
   quota has room: `Callsign.quotaUsed(2|3) < Callsign.quotaCap(2|3)`.
2. List the wallet's regular callsigns to use as fuel (use Enumerable:
   `tokenOfOwnerByIndex`). Get explicit user confirmation for **each** fuel
   name — burning is irreversible.
3. Send `Callsign.refineMint(name, [fuelTokenIds])` with value `0.001 BNB`
   (fuel count: 2 for a 3-char name, 3 for a 2-char name).
4. Confirm and report the new primary callsign.

## Notes

- Secondary royalties are 0%. The only protocol fee is the forge fee above.
- Artwork may not preview on BscScan (it is a data-URI generated on-chain);
  marketplaces and the project checker page render it correctly.
