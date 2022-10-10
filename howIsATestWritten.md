# How is a test written? #

## Basic building blocks of a test

- Jest globals include `test`, `describe`, `it`, and `expect`. You do not need to import these.
- These are the basic functions you will use to write your tests.

### `expect`
- `expect` is used to compare a generated result to an expected result, for example: `expect(1 + 1).toBe(2)`.
- In this example, `toBe` is a _matcher function_ which will make sure that the expected value is the same as the given input value.
- `toBe` can be used to compare primitives such as numbers and strings, for comparing anything else you can use `toEqual`.


```javascript
const a = { prop: 1 };
const b = { prop: 1 };
expect(a).toBe(b); // Fails
expect(a).toEqual(b); // Passes
```

- Other useful matcher functions include `toHaveBeenCalled`.
- Use this to test if a certain function was ever called.
- This is useful if you want to test a certain condition in your function that does not change the output but only calls another function.

```javascript
someFunction(); // This function calls innerFunction() 
expect(innerFunction).toHaveBeenCalled();
```

- You can use the `not` keyword to test the opposite of something.

```javascript
const notEmptyString = "Hello";
expect(notEmptyString.length).not.toBe(0);
```

There are many matcher functions. You can find them [here](https://jestjs.io/docs/expect) in the documentation.

### `test`
- `test` takes in a description and a callback function. The testing logic goes inside the callback.

```javascript
// utils.js
function add(a, b) {
  return a + b;
}

// utils.test.js
test("should add two number together", () => {
  const result = add(2, 2);
  expect(result).toBe(4);
})
```
The output in the terminal will look like this

![image](https://user-images.githubusercontent.com/25080049/194770619-2321ab03-921f-4d30-b53b-ece39bb5db28.png)

### `describe` and `it`

- If we want to structure our tests more, we can use `describe` and `it`.
- `describe` allows us to group a set of tests, e.g., tests that belong to the same function.
- `it` is an alias of `test` and both can be used in the same way. However, `it` provides a nice readability when used with `describe`.

Consider a scenario where a function has several outcomes you would like to test.

```javascript
// utils.js
function add(a, b) {
  if (a === null || b === null) return 0;

  return a + b;
}

// utils.test.js
describe("add", () => {
  it("should add two numbers together", () => {
    const result = add(2, 2);
    expect(result).toBe(4);
  });
  
  it("should return 0 if either number is null", () => {
    const result1 = add(null, 2);
    const result2 = add(2, null);
    
    expect(result1).toBe(0);
    expect(result2).toBe(0);
  });
});
```

Here we have two different test scenarios related to the same function, so it makes sense to group them together.

The output will look like this:

![image](https://user-images.githubusercontent.com/25080049/194773004-2845a6d3-5132-4edf-a93a-4e9d652d723e.png)

Putting the name of the function you are testing as the name of your `describe` block, and phrasing your `it` blocks as "should..." gives a very readable output.

Describe can also be nested. This is useful when testing for example reducers, where you have one function with several cases, and each case may need several tests.

An example output of a nested describe would look like this:

![image](https://user-images.githubusercontent.com/25080049/194771725-94451dad-44ae-46f1-9869-4d224a838c65.png)


### Example test structure

- Writing your tests with a "given" => "when" => "then" structure can help to produce a tidy and readable test. 
- Under "given" you can put any input parameter or mocks you will need to run the function you are testing.
- Under "when" you can run the function.
- Under "then" you do your comparisons using `expect` and matcher functions.
- Note: This is just a preference.

```javascript
// utils.js
function add(a, b) {
  if (a === null || b === null) return 0;

  return a + b;
}

// utils.test.js
describe("add", () => {
  it("should add two numbers together", () => {
    // given
    const numberA = 2;
    const numberB = 2;
    
    // when
    const result = add(numberA, numberB);
    
    // then
    expect(result).toBe(4);
  });
});
```
