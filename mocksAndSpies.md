# Mocks and spies #

## What is a mock

- When we abstract an external part of a function so it can be tested **in isolation**, we are _mocking_ that part of the function.
- You can think of mocking as simulating an outside influence of the function you are testing.
- This is useful in any scenario where a value can change inside your function. A testable function should always return the same thing given the same input.


## Why do we need mocks
- When we are testing a function (a unit), that is the only thing that should be tested. Any other functions it depends on should be mocked.
- Consider we are testing our `add` function, which now takes another function as a parameter. We now are relying on another function to be consistent and predictable in order for our test to work. Remember, we only want to test our `add` function, and we want to test it **in isolation**.

```javascript
// Function to test
function add(a, randomNumberGenerator) {
  return a + randomNumberGenerator();
}

// Create a Mock for 'getRandomNumber'
test('add should add two numbers together', ()=>{
  // given
  const firstNumber = 2;
  const mockedNumberGenerator = jest.fn(() => 3); // A mock function which will always return 3
    
  // when
  const result = add(2, mockedNumberGenerator);

  // then
  expect(result).toBe(5);
});
```

- Mocking a function is done using `jest.fn()`. 
- Inside `jest.fn()` you can define an optional mock implementation of the function.
- In this case, the mocked function will always return 3, which gives our `add` function a predictable output.

## Async tests and mocks

- What if there is a function being called inside `add` which is not an input parameter?
- How do we control what happens to that function call?
- We can mock the module where we import the function.

```javascript
// apiFunctions.js
export async function getNumberAsync() {
  const response = await fetch("www.getanumber.com");
  return response.json();
}
```



```javascript
// utils.js
import getNumberAsync from "./testAsync";

export async function add(a) {
  const b = await getNumberAsync();
  return a + b;
}
```


