# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A Node.js/TypeScript machine learning library implementing Decision Tree (ID3/CART), Random Forest, and XGBoost algorithms with comprehensive continuous variable support.

## Common Commands

```bash
# Install dependencies
npm install        # or: bun install

# Build TypeScript to JavaScript
npm run build      # or: bun run build

# Run all tests
npm test           # or: bun run test:bun

# Watch mode for development
npm run build:watch
npm run test:watch

# Run a single test file
npm run build && npx mocha --require ts-node/register tst/decision-tree.ts

# Run tests matching a pattern
npm run build && npx mocha --require ts-node/register --grep "Random Forest" tst/*.ts

# Run examples
npm run example:ts         # Decision Tree TypeScript
npm run example:rf-ts      # Random Forest TypeScript
npm run example:xgb-ts     # XGBoost TypeScript
```

## Architecture

### Main Classes (src/)

- **DecisionTree** (`src/decision-tree.ts`): Core classifier supporting both ID3 (discrete) and CART (continuous) algorithms with automatic algorithm selection
- **RandomForest** (`src/random-forest.ts`): Ensemble of DecisionTrees with bootstrap sampling and random feature selection; uses majority voting
- **XGBoost** (`src/xgboost.ts`): Gradient boosting implementation with early stopping, regularization (L1/L2), and multiple loss functions

### Shared Modules (src/shared/)

- `types.ts`: All TypeScript interfaces and type definitions
- `id3-algorithm.ts`: ID3 algorithm implementation (entropy, information gain)
- `cart-algorithm.ts`: CART algorithm for continuous variables (binary splits, Gini/MSE criteria)
- `data-type-detection.ts`: Automatic discrete/continuous feature detection
- `gradient-boosting.ts`: XGBoost-specific gradient boosting utilities
- `loss-functions.ts`: MSE, logistic, and cross-entropy loss functions
- `caching-system.ts`: Multi-level prediction caching
- `memory-optimization.ts`: Efficient data structures for large datasets
- `utils.ts`: Seeded random, bootstrap sampling, feature selection, majority voting

### Key Patterns

- All classes support multiple constructor signatures: `new Class(target, features)`, `new Class(target, features, config)`, or `new Class(jsonModel)` for import
- Automatic algorithm selection: `algorithm: 'auto'` detects data types and chooses ID3 (pure discrete), CART (continuous), or hybrid
- Model persistence: `toJSON()` to export, constructor or `import()` to restore
- Tests are in `tst/` directory using Mocha with TypeScript

### Build Output

TypeScript source in `src/` compiles to JavaScript in `lib/`. The package exports ES modules only (no CommonJS).

## Test Structure

Tests in `tst/*.ts` cover:
- Core functionality: `decision-tree.ts`, `random-forest.ts`, `xgboost.ts`
- Algorithm implementations: `id3-algorithm.ts`, `cart-algorithm.ts`, `data-type-detection.ts`
- Edge cases: `edge-cases.ts`, `prediction-edge-cases.ts`, `random-forest-edge-cases.ts`, `xgboost-edge-cases.ts`
- Utilities: `random-forest-utils.ts`, `xgboost-gradient-boosting.ts`, `xgboost-loss-functions.ts`

Sample datasets in `data/` (JSON): `sample.json`, `tic-tac-toe.json`, `voting.json`, `object-evaluation.json`
