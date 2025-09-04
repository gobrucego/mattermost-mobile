# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Mattermost Mobile is a React Native application for the Mattermost team collaboration platform. It supports iOS 15.1+ and Android 7.0+, connecting to Mattermost Server versions 10.5.0+.

## Development Commands

### Essential Commands
- `npm run check` - Run linting and type checking
- `npm run check-test` - Run linting, type checking, and tests
- `npm run test` - Run Jest tests
- `npm run test:watch` - Run tests in watch mode
- `npm run lint` - Run ESLint
- `npm run fix` - Auto-fix ESLint issues
- `npm run tsc` - Run TypeScript type checking

### Platform-Specific
- `npm run ios` - Run on iOS simulator
- `npm run android` - Run on Android emulator
- `npm run pod-install` - Install iOS CocoaPods dependencies
- `npm run pod-install-m1` - Install iOS dependencies on M1 Macs

### Build Commands
- `./scripts/build.sh ipa` - Build iOS
- `./scripts/build.sh apk` - Build Android
- `./scripts/clean.sh` - Clean build artifacts

## Architecture

### Database Layer
- **WatermelonDB**: Local SQLite database with two types:
  - **App Database**: Stores global app state and server connections
  - **Server Databases**: One per connected server, stores server-specific data
- **Database Manager** (`app/database/manager/index.ts`): Singleton managing all database connections
- **Models**: Database models are split between `app/database/models/app/` and `app/database/models/server/`
- **Operators**: Data access layer with `AppDataOperator` and `ServerDataOperator`

### Client Layer
- **REST Client** (`app/client/rest/`): HTTP API client for Mattermost server
- **WebSocket Client** (`app/client/websocket/`): Real-time communication with reliable WebSocket support
- **Network Manager**: Handles connectivity and request management

### State Management
- **Actions**: Redux-style actions in `app/actions/` divided by:
  - `local/`: Local database operations
  - `remote/`: Server API calls
  - `websocket/`: WebSocket event handlers
- **Queries** (`app/queries/`): Database query functions
- **Stores**: Ephemeral state management

### Navigation
- **React Native Navigation v7**: Native navigation library
- **Screen Architecture**: Each screen in `app/screens/` contains component and business logic

### Path Aliases
The project uses TypeScript path aliases:
- `@actions/*` → `app/actions/*`
- `@components/*` → `app/components/*`
- `@database/*` → `app/database/*`
- `@hooks/*` → `app/hooks/*`
- `@screens/*` → `app/screens/*`
- `@utils/*` → `app/utils/*`
- And many others (see `tsconfig.json`)

### Key Patterns
- **Multi-server support**: App can connect to multiple Mattermost servers simultaneously
- **Offline-first**: Local database enables offline functionality
- **Real-time sync**: WebSocket maintains data synchronization
- **Feature-based organization**: Code organized by feature areas (channels, posts, teams, etc.)

## Testing
- **Jest** with React Native preset
- **Testing Library**: For component testing
- Test files use `.test.ts/tsx` suffix
- Coverage reports available via `npm run test:coverage`

## Platform-Specific Notes
- iOS builds require Xcode and CocoaPods
- Android builds use Gradle
- Expo modules are used for certain native functionality
- Platform-specific code uses `.ios.ts` and `.android.ts` extensions when needed