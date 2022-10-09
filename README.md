# Unit testing - A brief introduction to jest #

## How does a test look?

- Jest globals include `test`, `describe`, `it`, and `expect`.

### test
- `test` takes in a description and a callback function. The testing logic goes inside the callback.

```javascript
// utils.js
function add(a, b) {
  return a + b;
}

// utils.test.js
test("Add should add two number together", () => {
  const result = add(2, 2);
  expect(result).toBe(4);
})
```
The output in the terminal will look like this

![image](https://user-images.githubusercontent.com/25080049/194770619-2321ab03-921f-4d30-b53b-ece39bb5db28.png)

- In the above code example, `toBe` is a _matcher function_ which we use to compare our output to the expected result.
- Other commonly used matcher functions include `toEqual` (used for deep equality between objects) and `toHaveBeenCalled` (used for checking if a particular function has been called)

### describe and it

- If we want to structure our tests more, we can use `describe` and `it`.
- `describe` allows us to group a set of tests, e.g., tests that belong to the same function.
- `it` is an alias of `test` and both can be used in the same way. However, `it` provides a nice readability when used with `describe`.

Consider a scenario where a function has several outcomes you would like to test.

```javascript
// utils.js
function add(a, b) {
  if (!a || !b) return 0;

  return a + b;
}

// utils.test.js
describe("add", () => {
  it("should add two numbers together", () => {
    const result = add(2, 2);
    expect(result).toBe(4);
  });
  
  it("should return 0 if either number is falsy", () => {
    const result1 = add(null, 2);
    const result2 = add(2, null);
    
    expect(result1).toBe(0);
    expect(result2).toBe(0);
  });
});
```

Here, we have two different test scenarios related to the same function, so it makes sense to group them together.

The output will look like this:

![image](https://user-images.githubusercontent.com/25080049/194771110-e7ed74d6-e956-4222-b9a2-a31b8d1f3513.png)

Describe can also be nested. This is useful when testing for example reducers, where you have one function with several cases, and each case may need several tests.

An example output of a nested describe would look like this:

![image](https://user-images.githubusercontent.com/25080049/194771725-94451dad-44ae-46f1-9869-4d224a838c65.png)


