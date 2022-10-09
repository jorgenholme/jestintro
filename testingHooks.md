# Testing hooks #

- If we try to test a hook like we would a normal function, we would get the error: `Invariant Violation: Hooks can only be called inside the body of a function component.`.
- To avoid creating components for testing hooks, we have react testing library to help us.
- To call a hook, we simply use the `renderHook` function.

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

However, if we want to call the `increment` function, thus changing the state in the hook, we need to wrap that call in `act`.

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

