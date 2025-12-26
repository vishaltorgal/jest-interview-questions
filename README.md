# jest-interview-questions

## 1. **What is describe in Jest?**

**describe** is used to group related test cases.
  
```jsx
describe('Math tests', () => {
  test('2 + 2 = 4', () => {
    expect(2 + 2).toBe(4);
  });
});
```

## 2. **What is expect in Jest?**

**expect** is used to create assertions to check the output of a test.
The basic syntax follows this pattern: *expect(actualValue).matcher(expectedValue);*
  
```jsx
test('two plus two is four', () => {
  const result = 2 + 2;
  expect(result).toBe(4);
});
```

## 3. **Difference between toBe and toEqual?**

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

## 7. **How to test API calls?**
## 8. **What is jest.spyOn()?**
## 9. **What is code coverage in Jest?**
## 10. **What is test.only?**
## 11. **What is test.skip?**
## 12. **Difference between mockClear, mockReset, mockRestore**
## 13. **How do you test React components with Jest?**
