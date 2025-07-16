# Database Analysis Queries

This file tracks all database queries used for analyzing transaction data and identifying transformation opportunities.

## Analysis Overview

**Goal**: Identify the most common event signatures and transaction types that are not currently handled by our ruleset or transformations.

**Approach**: 
1. Analyze current transformation coverage
2. Query database for unhandled patterns
3. Identify high-impact transformation opportunities

---

## Query Log

### 1. Database Overview
**Purpose**: Get basic understanding of available data
**Query**: 
```sql
SELECT COUNT(*) as count FROM transactions;
SELECT COUNT(*) as count FROM transaction_logs;
```
**Results**: 
- 3,979 transactions
- 0 transaction logs
- Data is primarily in the transactions table

### 2. Chain Distribution Analysis  
**Purpose**: Understand which chains have the most activity
**Query**:
```sql
SELECT chain_id, COUNT(*) as count 
FROM transactions 
GROUP BY chain_id 
ORDER BY count DESC;
```
**Results**:
- Base (8453): 2,295 transactions (57.7%)
- Ethereum (1): 590 transactions (14.8%) 
- Arbitrum (42161): 483 transactions (12.1%)
- Polygon (137): 355 transactions (8.9%)
- Optimism (10): 258 transactions (6.5%)

### 3. Sample Data Structure Analysis
**Purpose**: Understand how parsed data is stored
**Query**:
```sql
SELECT hash, chain_id, method_name, summary, contract_name, parsed_data 
FROM transactions 
WHERE parsed_data IS NOT NULL 
LIMIT 5;
```
**Findings**:
- Parsed data is stored as JSON objects with numeric keys (0, 1, etc.)
- Many transactions have unrecognized summaries
- Contract names are detected but methods often null

### 4. Most Common Unhandled Events Analysis
**Purpose**: Identify specific events that need transformation rules
**Query**:
```sql
SELECT 
  event_data->>'name' as event_name,
  event_data->>'contractName' as contract_name,
  COUNT(*) as count
FROM transactions,
jsonb_array_elements(parsed_data) as event_data
WHERE event_data->>'summary' = ''
GROUP BY event_data->>'name', event_data->>'contractName'
ORDER BY count DESC
LIMIT 15;
```
**Top Results**:
- WETH9.Transfer: 1,591 events
- PoolManager.ModifyLiquidity: 1,413 events  
- RelayRouter.SolverCallExecuted: 1,227 events
- UniswapV3Pool.Swap: 507 events
- WETH9.Deposit: 437 events
- WETH9.Withdrawal: 395 events
- Seaport.OrderFulfilled: 252 events
- SpaceStationV2.EventClaimCapped: 217 events

### 5. Event Argument Analysis
**Purpose**: Understand argument structures for transformation development
**Sample Queries**:
```sql
-- WETH9 Transfer events
SELECT event_data->'args' as args
FROM transactions, jsonb_array_elements(parsed_data) as event_data
WHERE event_data->>'name' = 'Transfer' 
  AND event_data->>'contractName' = 'WETH9'
  AND event_data->>'summary' = '';

-- Seaport OrderFulfilled events  
SELECT event_data->'args' as args
FROM transactions, jsonb_array_elements(parsed_data) as event_data
WHERE event_data->>'name' = 'OrderFulfilled' 
  AND event_data->>'contractName' = 'Seaport'
  AND event_data->>'summary' = '';
```

### 6. Chain-Specific Distribution
**Purpose**: Understand which events are most important per chain
**Query**:
```sql
SELECT 
  t.chain_id,
  event_data->>'contractName' as contract_name,
  event_data->>'name' as event_name,
  COUNT(*) as count
FROM transactions t,
jsonb_array_elements(t.parsed_data) as event_data
WHERE event_data->>'summary' = ''
  AND event_data->>'name' IS NOT NULL
  AND event_data->>'contractName' IS NOT NULL
GROUP BY t.chain_id, event_data->>'contractName', event_data->>'name'
ORDER BY count DESC;
```
