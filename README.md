# callsign-mint (Agent Skill)

An installable skill that lets an AI agent claim or forge a **CALLSIGN** codename
NFT on BNB Chain for its wallet.

**CALLSIGN** is the codename registry for AI agents: one free claim per
Agent-ID-holding wallet (4–15 chars), rare 2–3 char names forged by burning
regular ones, artwork rendered fully on-chain. Contract:
`0x4ebFd50398D9889A530F6DE326917BDEef4679f9` (BNB Chain). Source verified
(Sourcify exact match). Independent project — not affiliated with Binance or
SPACE ID; the Agent ID is only an eligibility check.

## Install

- **Skills-compatible agents** (Claude Code, Codex, OpenClaw-style runtimes):
  clone this repository into your skills directory under the name
  `callsign-mint`, e.g. `git clone <this-repo-url> <skills-dir>/callsign-mint`.
- **Any agent with web access:** point it at the raw `SKILL.md` URL of this
  repository and tell it to follow the instructions.
- **Humans:** paste the hand-off line below into your agent's chat.

## One-line hand-off (copy-paste into your agent)

> Read the CALLSIGN skill at <RAW_SKILL_URL> and claim a callsign for my
> wallet. Check my eligibility and name availability first, and confirm the
> exact name with me before sending any transaction.

(Repository owner: replace `<RAW_SKILL_URL>` with this file's raw URL, i.e.
`https://raw.githubusercontent.com/lutblip/callsign-mint/main/SKILL.md`.)

## Safety

The skill instructs agents to verify the contract address, check eligibility
and quota before transacting, and to obtain explicit human confirmation before
burning any name as forge fuel (burns are permanent "tombstones").
