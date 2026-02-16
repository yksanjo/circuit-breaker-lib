# Circuit Breaker Library

Circuit breaker pattern implementation for Node.js to prevent cascading failures.

## Features

- **Three States**: Closed, Open, Half-Open
- **Automatic Recovery**: Transitions to half-open after recovery timeout
- **Event Emission**: Built-in events for state changes
- **Manager**: CircuitBreakerManager for multiple breakers

## Installation

```bash
npm install circuit-breaker-lib
```

## Usage

```typescript
import { CircuitBreaker } from 'circuit-breaker-lib';

const breaker = new CircuitBreaker('my-service', {
  failureThreshold: 5,
  recoveryTimeout: 60000,
  halfOpenAttempts: 3
});

async function callService() {
  if (!breaker.canExecute()) {
    throw new Error('Circuit is open');
  }
  
  try {
    const result = await doSomething();
    breaker.recordSuccess();
    return result;
  } catch (error) {
    breaker.recordFailure();
    throw error;
  }
}
```

## States

- **Closed**: Normal operation, requests pass through
- **Open**: Too many failures, requests are blocked
- **Half-Open**: Testing recovery, limited requests allowed

## License

MIT
