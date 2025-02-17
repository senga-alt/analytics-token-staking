# AnalytiX: Decentralized Analytics Protocol

A next-generation DeFi protocol enabling decentralized analytics governance through innovative token staking mechanics and tiered participation.

## Overview

AnalytiX is a sophisticated smart contract platform built on Stacks that combines staking mechanics with governance capabilities. It enables users to participate in protocol governance through a tiered system, where staking STX tokens grants voting power and rewards.

## Features

### Staking System

- **Flexible Staking Options**
  - No lock period
  - 1-month lock (25% bonus)
  - 2-month lock (50% bonus)
- **Minimum Stake**: 1,000,000 microSTX
- **Base Reward Rate**: 5% APR
- **Cooldown Period**: 24 hours (1440 blocks)

### Tier System

1. **Tier 1**

   - Minimum Stake: 1,000,000 microSTX
   - Reward Multiplier: 1x
   - Basic feature access

2. **Tier 2**

   - Minimum Stake: 5,000,000 microSTX
   - Reward Multiplier: 1.5x
   - Enhanced feature access

3. **Tier 3**
   - Minimum Stake: 10,000,000 microSTX
   - Reward Multiplier: 2x
   - Premium feature access

### Governance

- **Proposal Creation**

  - Minimum voting power required: 1,000,000
  - Description length: 10-256 characters
  - Configurable voting period (100-2880 blocks)

- **Voting**
  - Weighted voting based on staking position
  - Vote tracking for/against proposals
  - Automatic vote counting system

## Contract Functions

### Staking Operations

```clarity
(define-public (stake-stx (amount uint) (lock-period uint)))
(define-public (initiate-unstake (amount uint)))
(define-public (complete-unstake))
```

### Governance Operations

```clarity
(define-public (create-proposal (description (string-utf8 256)) (voting-period uint)))
(define-public (vote-on-proposal (proposal-id uint) (vote-for bool)))
```

### Administrative Functions

```clarity
(define-public (pause-contract))
(define-public (resume-contract))
```

### Read-Only Functions

```clarity
(define-read-only (get-contract-owner))
(define-read-only (get-stx-pool))
(define-read-only (get-proposal-count))
```

## Security Features

### Access Control

- Contract owner privileges for critical operations
- Tiered access system based on stake amount
- Cooldown periods for unstaking operations

### Safety Measures

- Emergency pause functionality
- Minimum stake requirements
- Valid period checks for proposals
- Proper error handling with specific error codes

## Error Codes

| Code | Description                 |
| ---- | --------------------------- |
| 1000 | Not authorized              |
| 1001 | Invalid protocol parameters |
| 1002 | Invalid amount              |
| 1003 | Insufficient STX            |
| 1004 | Cooldown period active      |
| 1005 | No stake found              |
| 1006 | Below minimum stake         |
| 1007 | Contract paused             |

## Data Structures

### User Positions

```clarity
{
    total-collateral: uint,
    total-debt: uint,
    health-factor: uint,
    last-updated: uint,
    stx-staked: uint,
    analytics-tokens: uint,
    voting-power: uint,
    tier-level: uint,
    rewards-multiplier: uint
}
```

### Staking Positions

```clarity
{
    amount: uint,
    start-block: uint,
    last-claim: uint,
    lock-period: uint,
    cooldown-start: (optional uint),
    accumulated-rewards: uint
}
```

### Proposals

```clarity
{
    creator: principal,
    description: (string-utf8 256),
    start-block: uint,
    end-block: uint,
    executed: bool,
    votes-for: uint,
    votes-against: uint,
    minimum-votes: uint
}
```

## Reward Calculation

Rewards are calculated based on:

- Base reward rate (5% APR)
- Tier multiplier (1x - 2x)
- Lock period bonus (up to 1.5x)
- Staked amount
- Number of blocks since last claim

Formula:

```clarity
(stake-amount * base-rate * multiplier * blocks) / 14400000
```
