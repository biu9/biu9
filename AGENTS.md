# Agent 编码规范

1. 尽可能少的写防御性代码、fallback代码、try catch

2. 代码中尽可能少的用any、as

3. 不要用硬编码做逻辑判断，改用enum

```typescript
// bad case
const result = params?.decision === '保留' ? '保留' : '丢弃';

// good case
enum ModelAction {
  SAVE = '保留',
  REJECT = '丢弃',
}

const result = params?.decision === ModelAction.SAVE ? ModelAction.SAVE : ModelAction.REJECT;
```

4. 编码前，搜索代码仓库中是否有可复用的类似实现

5. 规范注释，good case：
```typescript
/** example count */
const EXAMPLE_COUNT = 10;

/**
 * example object
 * @property name - the name of the person
 * @property age - the age of the person
 * @property city - the city of the person
 */
const EXAMPLE_OBJECT = {
    name: 'John',
    age: 30,
    city: 'New York',
}

/**
 * example function, add two numbers
 * @param num1 - the first number
 * @param num2 - the second number
 * @returns number - the sum of num1 and num2
 */
const exampleFunc = (num1: number, num2: number): number => {
  return num1 + num2;
};
```
