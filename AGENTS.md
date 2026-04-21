# Agent.md

## 编码规范

- 尽可能少的写防御性代码、fallback代码、try catch

- 代码中尽可能少的用any、as

- 不要用硬编码做逻辑判断，改用enum

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

- 规范注释，good case：
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

## 文件结构规范

- 编码前，搜索代码仓库中是否有可复用的类似实现，默认值、常量等，搜索代码库中是否有config文件，写在单独的config文件中，而不是代码文件中

- 如果一个变量/函数在不同地方出现2次及以上，将其抽离为公共配置/函数

## 测试规范

- TDD驱动，完成功能后，需要编写单元测试、集成测试、e2e测试

- 单元测试描述使用中文

## 技术架构规范

- 对于新项目，优先使用bun + Typescript + Node22
