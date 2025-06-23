# sBTC-Staxion - STX Staking Pool Contract

The STX Staking Pool Contract is a smart contract designed for managing a staking pool with tiered rewards, governance, referrals, and advanced security features. It allows users to stake STX tokens, earn rewards based on their staking tier, participate in governance, and benefit from a referral system.

---

## Features

1. **Tiered Staking Rewards**  
    - Users are categorized into tiers based on their staking amount.
    - Higher tiers receive greater reward multipliers.

2. **Governance System**  
    - Stakers can create and vote on proposals.
    - Voting power is proportional to the staked amount and tier.

3. **Referral System**  
    - Users can refer others to the staking pool and earn bonuses.

4. **Advanced Security**  
    - Includes cooldown periods for unstaking.
    - Emergency withdrawal functionality with a timelock.

5. **Price Oracle Integration**  
    - Allows the contract owner to update the price for external integrations.

---

## Constants

### Staking Tiers
| Tier | Minimum STX | Multiplier |
|------|-------------|------------|
| 1    | 100 STX     | 1x         |
| 2    | 1,000 STX   | 1.25x      |
| 3    | 10,000 STX  | 1.5x       |

### Timelocks and Cooldowns
- **Unstake Cooldown**: 100 blocks (~10 minutes).
- **Governance Voting Period**: 1,440 blocks (~10 days).
- **Emergency Withdrawal Timelock**: 144 blocks (~24 hours).

---

## Data Structures

### Data Variables
- `total-staked`: Total amount of STX staked in the pool.
- `pool-active`: Boolean indicating if the pool is active.
- `reward-cycle`: Current reward cycle.
- `total-rewards`: Total rewards distributed.
- `governance-proposal-id`: ID for the next governance proposal.
- `last-price`: Last updated price from the oracle.

### Data Maps
- `staker-balances`: Tracks each staker's balance.
- `staker-rewards`: Tracks rewards for each staker.
- `staker-tiers`: Tracks the tier of each staker.
- `reward-distribution`: Tracks rewards per cycle.
- `referral-rewards`: Tracks referral bonuses.
- `last-unstake-block`: Tracks the last unstake block for each staker.
- `governance-proposals`: Stores governance proposals.
- `governance-votes`: Tracks votes for proposals.
- `governance-vote-counts`: Tracks vote counts for proposals.

---

## Functions

### Public Functions

#### Staking
- **`stake(amount uint)`**  
  Stake STX tokens and update the user's tier.

- **`unstake(amount uint)`**  
  Unstake STX tokens with a cooldown period.

#### Governance
- **`create-proposal(description (string-utf8 256))`**  
  Create a governance proposal (requires Tier 2 or higher).

- **`vote(proposal-id uint, vote-bool bool)`**  
  Vote on a governance proposal.

#### Referrals
- **`register-referral(referrer principal)`**  
  Register a referral and calculate the bonus.

#### Rewards
- **`calculate-rewards(staker principal)`**  
  Calculate rewards for a staker based on their tier and balance.

#### Emergency
- **`initiate-emergency-withdrawal()`**  
  Initiate an emergency withdrawal with a timelock.

- **`execute-emergency-withdrawal()`**  
  Execute the emergency withdrawal after the timelock.

#### Price Oracle
- **`update-price(new-price uint)`**  
  Update the price (only callable by the contract owner).

---

### Read-Only Functions

- **`get-staker-balance(staker principal)`**  
  Retrieve the balance of a staker.

- **`get-referral-bonus(referrer principal, referee principal)`**  
  Retrieve the referral bonus for a referrer and referee.

---

## Security Features

1. **Owner-Only Functions**  
    - Certain functions, like `update-price` and emergency withdrawals, are restricted to the contract owner.

2. **Cooldown Periods**  
    - Prevents rapid unstaking to ensure stability.

3. **Emergency Withdrawal**  
    - Allows the owner to withdraw all funds in case of an emergency, with a 24-hour timelock.

4. **Governance Voting**  
    - Ensures only active stakers can participate in governance.

---

## How to Use

1. **Stake STX**  
    Call the `stake` function with the desired amount of STX.

2. **Earn Rewards**  
    Rewards are calculated based on your tier and staking amount.

3. **Participate in Governance**  
    Create or vote on proposals if you meet the tier requirements.

4. **Refer Others**  
    Use the `register-referral` function to earn bonuses for referrals.

5. **Unstake STX**  
    Call the `unstake` function to withdraw your tokens after the cooldown period.

---

## Error Codes

| Code | Description                     |
|------|---------------------------------|
| 100  | Owner-only function             |
| 101  | Insufficient funds              |
| 102  | Not an active staker            |
| 103  | No rewards available            |
| 104  | Invalid staking tier            |
| 105  | Proposal not active             |
| 106  | Already voted                   |
| 107  | Cooldown period active          |
| 108  | Amount below Tier 1 minimum     |
| 109  | Insufficient balance for Tier 2 |
| 110  | Emergency timelock not expired  |

---

## License

This project is licensed under the [MIT License](LICENSE).
