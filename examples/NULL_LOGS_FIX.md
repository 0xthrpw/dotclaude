# Null Logs Fix

## Issue
Analytics recording was failing with the error:
```
TypeError: undefined is not an object (evaluating 'log.name')
```

## Root Cause
The `parsedLogs` array from the transaction parser contains `null` values when log parsing fails (which is expected behavior). However, several places in the codebase were iterating over this array without checking for null values before accessing log properties.

## Affected Code Locations

### 1. `src/history.ts` (Line 132-136)
**Issue**: Mapping over `parsedLogs` without filtering nulls
**Fix**: Added `.filter(log => log !== null)` before `.map()`

```typescript
// Before
parsedLogs: parsedTx.parsedLogs?.map(log => ({
    name: log.name,
    contractName: log.contractName,
    summary: log.summary
}))

// After  
parsedLogs: parsedTx.parsedLogs?.filter(log => log !== null)?.map(log => ({
    name: log.name,
    contractName: log.contractName,
    summary: log.summary
}))
```

### 2. `src/analytics/collectors/transaction.ts` (Line 54)
**Issue**: Looping over `parsedLogs` without null check
**Fix**: Added `if (log)` check inside loop

```typescript
// Before
for (const log of data.parsedLogs) {
    events.push(this.createEvent(
        'log_event',
        {
            event_name: log.name,
            // ...
        }
    ))
}

// After
for (const log of data.parsedLogs) {
    if (log) { // Skip null entries
        events.push(this.createEvent(
            'log_event',
            {
                event_name: log.name,
                // ...
            }
        ))
    }
}
```

### 3. `src/analytics/collectors/transaction.ts` (Line 146)
**Issue**: Looping over `parsedLogs` for event grouping without null check  
**Fix**: Added `if (log)` check inside loop

```typescript
// Before
for (const log of data.parsedLogs) {
    const key = `${log.contractName || 'unknown'}:${log.name}`
    eventGroups.set(key, (eventGroups.get(key) || 0) + 1)
}

// After
for (const log of data.parsedLogs) {
    if (log) { // Skip null entries
        const key = `${log.contractName || 'unknown'}:${log.name}`
        eventGroups.set(key, (eventGroups.get(key) || 0) + 1)
    }
}
```

## Why This Happens
The transaction parser (`src/parse/index.ts`) returns `null` for logs that cannot be parsed:

```typescript
const parsedLogs = (logs || []).map(async (log: Log) => {
    try {
        // ... parsing logic
        return { address: log.address, contractName, name: decodedLog.eventName, args: decodedLog.args, summary, icon };
    } catch {
        // Parsing failed - return null
        return null;
    }
})
```

This is expected behavior - not all logs can be successfully parsed, especially for contracts without known ABIs.

## Solution Benefits
- **Robust Error Handling**: System continues operating even when some logs fail to parse
- **Clean Analytics**: Only successfully parsed logs are included in analytics
- **No Data Loss**: Failed log parsing doesn't prevent transaction processing
- **Performance**: Filtering nulls is more efficient than try/catch in analytics code

## Testing
Verified the fix with test data containing mixed valid logs and nulls:
- ✅ Filter correctly removes null values (5 → 3 entries)
- ✅ Mapping works without errors after filtering
- ✅ Analytics collection succeeds without throwing errors

## Related Files
- `src/history.ts` - Historical transaction analytics recording
- `src/analytics/collectors/transaction.ts` - Real-time analytics collection
- `src/parse/index.ts` - Transaction parser (source of nulls - no changes needed)