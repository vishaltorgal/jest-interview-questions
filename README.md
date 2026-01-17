# Jest Interview Questions (2025–2026)

### Table of Contents

1. [What is describe in Jest?](#1-what-is-describe-in-jest)
2. [What is expect in Jest?](#2-what-is-expect-in-jest)
3. [Difference between toBe and toEqual?](#3-difference-between-tobe-and-toequal)
4. [What is snapshot testing?](#4-what-is-snapshot-testing)
5. [What are lifecycle methods in Jest?](#5-what-are-lifecycle-methods-in-jest)
6. [How do you test async code in Jest?](#6-how-do-you-test-async-code-in-jest)
7. [What is code coverage in Jest?](#7-what-is-code-coverage-in-jest)
8. [What is test.only?](#8-what-is-testonly)
9. [whats difference describe.only vs test.only?](#9-whats-difference-describeonly-vs-testonly)
10. [What is test.skip?](#10-what-is-testskip)
11. [How to test API calls?](#11-how-to-test-api-calls)
12. [What is jest.spyOn()?](#12-what-is-jestspyon)
13. [Difference: jest.spyOn vs jest.mock?](#13-difference-jestspyon-vs-jestmock)
14. [Difference between mockClear, mockReset, mockRestore](#14-difference-between-mockclear-mockreset-mockrestore)
15. [How do you test React components with Jest?](#15-how-do-you-test-react-components-with-jest)


## 1. What is describe in Jest?

**describe** is used to group related test cases.
  
```jsx
describe('Math tests', () => {
  test('2 + 2 = 4', () => {
    expect(2 + 2).toBe(4);
  });
});
```

## 2. What is expect in Jest?

**expect** is used to create assertions to check the output of a test.
The basic syntax follows this pattern: *expect(actualValue).matcher(expectedValue);*
  
```jsx
test('two plus two is four', () => {
  const result = 2 + 2;
  expect(result).toBe(4);
});
```

## 3. Difference between toBe and toEqual?

**toBe** checks exact reference or primitive value (Similar to ===)

**toEqual** checks deep equality for objects or arrays
  
```jsx
test('primitives work with both', () => {
  const value = 10;
  expect(value).toBe(10);    // PASS
  expect(value).toEqual(10); // PASS
});
```

```jsx
test('difference in objects', () => {
  const carA = { brand: 'Tesla', model: 'S' };
  const carB = { brand: 'Tesla', model: 'S' };

  // This FAILS because carA and carB are different instances
  expect(carA).toBe(carB); 

  // This PASSES because the properties and values match exactly
  expect(carA).toEqual(carB);
});

```

```jsx
test('referential identity', () => {
  const fruits = ['apple', 'banana'];
  const sameFruits = fruits;

  // This PASSES because they point to the same array in memory
  expect(fruits).toBe(sameFruits);
});

```

## 4. **What is snapshot testing?**

**First Run:** Jest creates a __snapshots__ folder and saves the output into a .snap file.

**Subsequent Runs:** Jest compares the output of the code against that .snap file.

**If it fails:** You must decide:

**Is it a bug?** Fix the code so it matches the snapshot.

**Is it an intentional change?** Press u (update) to overwrite the old snapshot with the new version.


```jsx
import renderer from 'react-test-renderer';
import Link from '../Link';

test('renders correctly', () => {
  const tree = renderer
    .create(<Link page="http://www.google.com">Google</Link>)
    .toJSON();
    
  // This creates or checks the snapshot
  expect(tree).toMatchSnapshot();
});

```

## 5. **What are lifecycle methods in Jest?**

- beforeEach()
- afterEach()
- beforeAll()
- afterAll()


| `Hook`               | `When it runs`                       | `Best Used For`                   |
| --------------------- | ------------------------------- | ----------------------------------- |
| **beforeAll**           | Once before any tests in the file start.  | Setting up a database connection. |
| **beforeEach**           | Before every single test (it or test block).  | Resetting variables or global state. |
| **afterEach**           | After every single test finishes.  | Cleaning up temporary files or mocks. |
| **afterAll**           | Once after all tests in the file are done.  | Closing server connections. |


## 6. **How do you test async code in Jest?**
By using async/await or returning promises.

```jsx
test('async test', async () => {
  const data = await fetchData();
  expect(data).toBe('done');
});

```

## 7. **What is code coverage in Jest?**
Code coverage in Jest tells you how much of your code is actually executed (tested) during your test runs.

| Metric     | Meaning                   |
| ---------- | ------------------------- |
| Statements | % of statements executed  |
| Branches   | if/else conditions tested |
| Functions  | functions called          |
| Lines      | lines executed            |


`npm test -- --coverage`

```jsx
// sum.js
export const sum = (a, b) => a + b
```

***Coverage will be 100% because all lines ran.***
```jsx
// sum.test.js
import { sum } from './sum'

test('adds numbers', () => {
  expect(sum(2, 3)).toBe(5)
})
```

***Branch coverage is 50%***
```jsx
export const check = (x) => {
  if (x > 10) {
    return 'big'
  }
  return 'small'
}
```
If you test only x > 10, then:
Branch coverage is 50%. 
Because else was never tested

## 8. **What is test.only?**
test.only in Jest is used to run only one specific test and skip all other tests. Very useful while debugging.

```jsx
test.only('runs only this test', () => {
  expect(2 + 2).toBe(4)
})

test('this test will be skipped', () => {
  expect(1 + 1).toBe(2)
})
```
```jsx
describe.only('Math tests', () => {
  test('adds', () => {
    expect(2 + 3).toBe(5)
  })
})

describe('String tests', () => {
  test('length', () => {
    expect('hi'.length).toBe(2)
  })
})

```

## 9. **whats difference describe.only vs test.only?**
- `test.only` runs only one single test
- `describe.only` runs all tests inside one describe block

## 10. **What is test.skip?**
`test.skip` is a Jest feature used to temporarily skip a test so it does not run during the test execution.

```jsx
test('runs normally', () => {
  expect(true).toBe(true);
});

test.skip('skipped test', () => {
  expect(false).toBe(true);
});
```
***Skip multiple tests***

```jsx
describe.skip('User API tests', () => {
  test('should create user', () => {});
  test('should delete user', () => {});
});
```

## 11. **How to test API calls?**

***Example function that calls an API***
```jsx
// api.js
export const getUsers = async () => {
  const res = await fetch('https://api.example.com/users');
  return res.json();
};

```
***Test with mocked fetch***
```jsx
// api.test.js
import { getUsers } from './api';

global.fetch = jest.fn();

test('fetches users successfully', async () => {
  fetch.mockResolvedValueOnce({
    json: jest.fn().mockResolvedValueOnce([
      { id: 1, name: 'John' }
    ])
  });

  const data = await getUsers();

  expect(fetch).toHaveBeenCalledWith('https://api.example.com/users');
  expect(data[0].name).toBe('John');
});

```

## 12. **What is jest.spyOn()?**

`jest.spyOn()` creates a spy on an existing method to check how many times it was called, with what arguments, or to mock its behavior.

***File***
```jsx
// math.js
export const add = (a, b) => a + b;
```

***Test***
```jsx
import * as math from './math';

test('mock implementation using spyOn', () => {
  const spy = jest.spyOn(math, 'add').mockReturnValue(10);

  const result = math.add(2, 3);

  expect(result).toBe(10);
});

```
## 13. **Difference: jest.spyOn vs jest.mock?**

| Feature         | jest.spyOn()      | jest.mock()     |
| --------------- | ----------------- | --------------- |
| Scope           | Single function   | Whole module    |
| Original code   | Exists            | Replaced        |
| Partial mocking | Yes               | No              |
| Tracks calls    | Yes               | Yes             |
| Restore needed  | Yes (mockRestore) | No              |
| Use case        | Internal methods  | External module |


## 14. **Difference between mockClear, mockReset, mockRestore**

| Feature                    | mockClear() | mockReset() | mockRestore() |
| -------------------------- | ----------- | ----------- | ------------- |
| Clears call history        | Yes         | Yes         | Yes           |
| Clears mock implementation | No          | Yes         | Yes           |
| Clears mock return value   | No          | Yes         | Yes           |
| Restores original function | No          | No          | Yes           |
| Used with jest.fn()        | Yes         | Yes         | No            |
| Used with jest.spyOn()     | Yes         | Yes         | Yes           |

- `mockClear` clears history only
- `mockReset` clears history and implementation
- `mockRestore` brings back original function

## 15. **How do you test React components with Jest?**

***Simple component***
```jsx
function Hello() {
  return <h1>Hello World</h1>;
}

```

***Test***
```jsx
import { render, screen } from '@testing-library/react';
import Hello from './Hello';

test('shows Hello World text', () => {
  render(<Hello />);

  expect(screen.getByText('Hello World')).toBeInTheDocument();
});

```
***What is happening***
- Render the component
- Check if text appears on screen

[⬆ Back to Table of Contents](#table-of-contents)

