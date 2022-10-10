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

## Async tests and mocking modules

- What if there is a function being called inside `add` which is not an input parameter?
- How do we control what happens to that function call?
- We can mock the module/file we import the function from.

Consider we have two files.

A file containing an async function.
```javascript
// apiFunctions.js
export async function getNumberAsync() {
  const response = await fetch("www.getanumber.com"); // Get a number from some URL
  const responseText = response.text();
  return Number(responseText); // Return it as a number
}
```

And a file containing our `add` function.
```javascript
// utils.js
import getNumberAsync from "./testAsync";

export async function add(a) {
  const b = await getNumberAsync();
  return a + b;
}
```

Our test file could look like this.
```javascript
// utils.test.ts
import { add } from "./test";
import { getNumberAsync } from "./testAsync";

// Mock "./testAsync"
jest.mock("./testAsync");

describe("add", () => {
  it("should add two numbers together", async () => {
    // given
    const firstNumber = 2;
    getNumberAsync.mockResolvedValue(2); // Mock the resolved value of getNumberAsync

    // when
    const result = await add(firstNumber);

    // then
    expect(result).toBe(4);
  });
});
```

- We use `jest.mock()` to mock modules.

## Using `spyOn`

- A spy allows you to keep the original implementation of the function you want to mock, while still being able to track, e.g., if it was called.

```javascript
import { add } from "./test";
const asyncFunctions = require('./testAsync');

// Function to test
const add = a => a + additionalFns.getNumber();

// Using jest.spyOn() for Mocking
test('adding function with spyOn', () => {
  const getNumberAsyncSpy = jest.spyOn(asyncFunctions, 'getNumberAsync');

  expect(getNumberAsyncSpy).toHaveBeenCalled();

  spyOn.mockRestore();
});
```

- `spyOn` takes in an object as the first parameter, and the property it is supposed to spy on as the second parameter.
- You can also mock the implementation or return value using this method. For example: `jest.spyOn(asyncFunctions, 'getNumberAsync').mockResolvedValue(2)`
- A drawback is that spyOn requires and object, so you have to import your module using `require` instead of just importing the function you want to mock.

[Next page](testingHooks.md)
