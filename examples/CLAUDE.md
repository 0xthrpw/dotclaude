# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

- **Run the application**: `bun src/index.ts` or `bun run src/index.ts`
- **Test functionality**: `bun src/test.ts` (runs custom test script)
- **Run all tests**: `bun test` (runs comprehensive test suite)
- **Run specific tests**: `bun test src/parse/transformations/transformations.test.ts` or `bun test src/parse/ruleset.test.ts`
- **Install dependencies**: `bun install`
- **TypeScript compilation check**: `bun --check src/index.ts`

### Database Commands (Analytics)
- **Setup database**: `bun run db:setup` (creates database and runs migrations)
- **Run migrations**: `bun run db:migrate` (runs pending migrations only)
- **Check database health**: `bun run db:health` (verifies connectivity and tables)
- **Reset database**: `bun run db:reset` (destructive - drops all analytics tables)

## Project Architecture

This is a real-time blockchain data feed service that processes Ethereum transactions and streams them via WebSocket. The architecture consists of:

### Core Components

- **WebSocket Server** (`src/index.ts`): Main entry point serving WebSocket connections on port 8080 with stream-based subscriptions
- **Redis Stream Integration** (`src/stream.ts`, `src/redis.ts`): Uses Redis streams for real-time data broadcasting and message queuing
- **Transaction Parser** (`src/parse/index.ts`): Decodes blockchain transactions and events using viem library
- **Blockchain Clients** (`src/clients.ts`): EVM client connections for multiple networks
- **Data Transformations** (`src/parse/transformations/`): Converts raw blockchain data into human-readable formats

### Key Modules

- **Parsing System**: Located in `src/parse/` with ruleset-based event interpretation
  - `ruleset.ts`: Defines parsing rules for different contracts and events
  - `transformations/`: Network-specific transformations (ENS, ERC20, Uniswap, EFP, ECP)
  - `abis/`: Contract ABIs for event decoding
- **Stream Management**: Redis-based pub/sub system with consumer groups
- **ENS Resolution**: Built-in ENS name resolution for addresses

### Technology Stack

- **Runtime**: Bun (JavaScript/TypeScript runtime)
- **Blockchain**: viem library for Ethereum interactions
- **Database**: Redis for stream processing and caching
- **WebSocket**: Built-in Bun WebSocket server
- **Deployment**: Docker containerized, Railway.app deployment configuration

### Data Flow

1. WebSocket clients connect with stream parameters (e.g., `?stream=addr:0x...`)
2. ENS names are resolved to addresses automatically
3. Historical data is replayed from Redis streams
4. New blockchain events are parsed and transformed
5. Processed data is broadcast to subscribed clients

### Path Alias

The project uses `#/*` as an alias for `src/*` in imports, configured in `tsconfig.json`.

### Contract Integration

The system recognizes specific contracts and events through the ruleset system, allowing for human-readable transaction summaries with appropriate transformations for different protocols.