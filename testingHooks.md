# Testing hooks #

- If we try to test a hook like we would a normal function, we would get the error: `Invariant Violation: Hooks can only be called inside the body of a function component.`.
- To avoid creating components for testing hooks, we have react testing library to help us.
- To call a hook, we simply use the `renderHook` function.

## Using `renderHook`
Suppose we have a `useCounter` hook, which returns the current count and a function to increment it.
```javascript
// useCounter.js
import { useState, useCallback } from 'react'

export default function useCounter() {
  const [count, setCount] = useState(0)
  const increment = useCallback(() => setCount((x) => x + 1), [])
  return { count, increment }
}
```

To run this hook, we wrap the call in the `renderHook` function.
```javascript
// useCounter.test.js
import { renderHook } from '@testing-library/react-hooks'
import useCounter from './useCounter'

test('count should be 0', () => {
  const { result } = renderHook(() => useCounter())

  expect(result.current.count).toBe(0)
})
```

## The `result` object
The `renderHook` function returns a `result` object which looks like this:
```typescript
{
  all: Array<any>
  current: any,
  error: Error
}
```

- `current` reflects the latest of what your hook returned.
- `error` contains any error that may have been thrown during your hook's execution. (This will not show up in `current`)
- `all` contains every return your hook has done including the latest. These could be results and errors.


## Using `act`
Considering the same hook, what if we want to run and test the `increment` function? This will change the state in the hook, so we need to wrap the call in `act`.

```javascript
// useCounter.test.js
import { renderHook, act } from '@testing-library/react-hooks'
import useCounter from './useCounter'

test('count should be 1 after incrementing', () => {
  const { result } = renderHook(() => useCounter())

  act(() => {
    result.current.increment()
  })

  expect(result.current.count).toBe(1)
})
```

Act simulates how our hook will behave in a browser, allowing us to update the values within it. For more info on `act` go to the [React docs](https://reactjs.org/docs/test-utils.html#act).

## Props and rerenders
- Sometimes props will change some behaviour inside your hook.
- For example, if we add a `reset` function to our `useCounter` hook which will reset the counter to an initial value, it could look something like this:

```javascript
// useCounter.js
import { useState, useCallback } from 'react'

export default function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue)
  const increment = useCallback(() => setCount((x) => x + 1), [])
  const reset = useCallback(() => setCount(initialValue), [initialValue])
  return { count, increment, reset }
}
```

- The only time `reset` is updated here is when `initialValue` changes.
- To trigger this in our test we can provide the initial value, change it, and rerender the hook using the `rerender` function.

```javascript
// useCounter.test.js
import { renderHook, act } from '@testing-library/react-hooks'
import useCounter from './useCounter'

test('should reset counter to updated initial value', () => {
  let initialValue = 0
  const { result, rerender } = renderHook(() => useCounter(initialValue))

  initialValue = 10
  rerender()

  act(() => {
    result.current.reset()
  })

  expect(result.current.count).toBe(10)
})
```

Examples are from [here](https://react-hooks-testing-library.com/usage/basic-hooks).
