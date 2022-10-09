# What should we test? #

- We only test logic (including hooks), not components.
- Ideally we would test any exported function that contains some logic.
- Since we're on a deadline we need to be a bit more picky with what we test.

```javascript
export function incrementCounterState(dispatch, type) {
  if(type === "increment_counter") {
    dispatch(type);
  }
}
```

- The above function call dispatch if the provided type is "increment_counter".
- Is it necessary to test that dispatch is called here?

```typescript
export const calcNetDealtAmount = (
  legs: Leg[],
  baseCurrency: string
): string => {
  const netDealtAmount = legs.reduce((prev, current) => {
    if (current.takerBuysBase) {
      if (current.dealtCurrency === baseCurrency) {
        return prev + Number(current.dealtAmount);
      }
      return prev - Number(current.dealtAmount);
    } else {
      if (current.dealtCurrency === baseCurrency) {
        return prev - Number(current.dealtAmount);
      }
      return prev + Number(current.dealtAmount);
    }
  }, 0);
  return `${netDealtAmount}`;
};
```

- The above function presents some complex logic and several conditions.
- It could be hard to realize if the output here is correct, since the correct number is not always obvious.
- A unit test could ensure that this calculation is correct, while providing some documentation of the expected behaviour as well.

