# 📚 StakeStream: Bitcoin-Aligned Staking & Governance Protocol

**StakeStream** is a next-generation, Bitcoin-secured staking and on-chain governance protocol built on **Stacks L2**. It combines capital-efficient staking mechanisms with tiered rewards and enterprise-grade compliance features.

This Clarity smart contract enables users to stake STX tokens with time-lock options, earn yield via a dynamic reward engine, and participate in decentralized governance by creating and voting on proposals. Designed to be non-custodial and transparent, StakeStream upholds Bitcoin’s security through the **Proof of Transfer (PoX)** consensus.

---

## 🚀 Key Features

* **Tiered Staking System**: Three staking tiers (Bronze, Silver, Gold) with escalating benefits based on stake size and duration.
* **Lock-Based Multipliers**: Optional 1–2 month lock periods offer increased yield (up to 2x).
* **On-Chain Governance**: Users can submit and vote on proposals using quadratic voting powered by stake-based voting weight.
* **Emergency Protections**: Cooldown periods and emergency mode toggles help secure user funds.
* **Enterprise Compliance Layer**: Transaction hooks and segregation support institutional use cases.
* **Security-Oriented Design**: Withdrawal cooldowns and feature gating per tier add additional security and risk controls.

---

## 🧩 System Overview

StakeStream consists of three primary modules:

| Module                      | Description                                                                                                    |
| --------------------------- | -------------------------------------------------------------------------------------------------------------- |
| **Staking Engine**          | Manages user staking positions, lock periods, reward accrual, and cooldown-based unstakes.                     |
| **Governance Module**       | Enables proposal creation and on-chain voting using stake-weighted power.                                      |
| **Tiered Incentive System** | Assigns users to Bronze/Silver/Gold tiers with associated multipliers and feature access based on their stake. |

---

## 📐 Contract Architecture

### 🏗 Smart Contract Components

| Component          | Type  | Description                                                                               |
| ------------------ | ----- | ----------------------------------------------------------------------------------------- |
| `UserPositions`    | `map` | Tracks user financial positions including stake, voting power, and rewards multiplier.    |
| `StakingPositions` | `map` | Tracks individual staking data such as lock period, claim history, and cooldown.          |
| `Proposals`        | `map` | Governance proposals including voting periods and execution status.                       |
| `TierLevels`       | `map` | Configuration for staking tiers: minimum stake, reward multipliers, and enabled features. |

### 📊 Constants

* `minimum-stake`: 1M STX (u1000000)
* `base-reward-rate`: 5% annualized (u500)
* `bonus-rate`: 1% for time locks (u100)
* `cooldown-period`: 1440 blocks (~24 hours)

### 🛠 Functions

#### 🔹 Public

* `initialize-contract`: Initializes the contract and configures tier levels.
* `stake-stx`: Stake STX with optional lock-up.
* `initiate-unstake`: Begins unstaking via cooldown mechanism.
* `complete-unstake`: Completes withdrawal post-cooldown.
* `create-proposal`: Governance proposal creation.
* `vote-on-proposal`: Vote on proposals using voting power.
* `pause-contract` / `resume-contract`: Owner-only toggles for halting/restarting protocol actions.

#### 🔸 Read-only

* `get-stx-pool`: Total STX currently staked in the contract.
* `get-proposal-count`: Total number of governance proposals.
* `get-contract-owner`: Returns the contract admin.

#### 🔐 Private

* `calculate-rewards`: Computes rewards based on stake, block duration, and multipliers.
* `get-tier-info`: Returns tier-level info based on stake.
* `calculate-lock-multiplier`: Lock period → bonus multiplier logic.
* `is-valid-lock-period`, `is-valid-description`, `is-valid-voting-period`: Internal validation helpers.

---

## 🧬 Data Flow: Staking & Rewards (Simplified)

```text
[User stakes STX] 
     ↓
[Contract assigns tier level]
     ↓
[Rewards multiplier calculated based on tier + lock period]
     ↓
[Staking position recorded]
     ↓
[User accrues rewards per block]
     ↓
[User initiates unstake → cooldown starts]
     ↓ (after 24hr)
[User completes unstake → STX returned]
```

---

## 🗳 Governance Flow

```text
[User with ≥1M voting power]
     ↓
[Creates proposal (description + voting period)]
     ↓
[Community votes using quadratic voting logic]
     ↓
[After expiry, outcome determined]
     ↓
[Proposal executed or rejected based on vote results]
```

---

## 🧾 Tier Structure

| Tier   | Min Stake | Multiplier | Features Enabled                       |
| ------ | --------- | ---------- | -------------------------------------- |
| Bronze | 1M STX    | 1.0x       | Basic staking                          |
| Silver | 5M STX    | 1.5x       | + Voting, Bonus Rewards                |
| Gold   | 10M STX   | 2.0x       | + Proposal Creation, Governance Access |

---

## 🛡 Security & Compliance

* **Non-custodial**: STX is held in contract; users retain control.
* **Cooldown Unstake**: 24-hour cooldown prevents instant withdrawals (anti-exploit).
* **Emergency Mode**: Contract owner can pause operations in critical scenarios.
* **Feature Flags**: Each tier controls access to protocol features.
* **Regulatory-Ready**: Designed with hooks for transaction monitoring and enterprise compliance.

---

## ✅ Deployment & Testing

> ⚠️ *Only the contract logic is included in this repository. Integration with front-end or CLI tools for proposal creation and staking is outside the scope of this README.*

To test locally:

```bash
clarinet test
```

Ensure Clarinet is installed: [https://docs.hiro.so/clarity/clarinet/get-started](https://docs.hiro.so/clarity/clarinet/get-started)

---

## 📄 License

MIT License © 2025 StakeStream Protocol Contributors
