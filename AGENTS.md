# AGENTS.md

## 编码规范

- 尽可能减少过度设计与不必要的代码

- 尽可能少的写防御性代码、fallback代码、try catch。所有报错需要有对应的日志输出，不接受静默失败

- 代码中尽可能少的用any、as，减少对于lambda表达式的使用

- 不需要做过多的兼容、最小改动、兜底。你的变更需要以代码简洁、逻辑明确为目标

- 不要用硬编码做逻辑判断，改用enum

- 代码逻辑中，不能根据字符串硬编码判断，如对于error，不能判断error字符串包含哪些然后做下一步处理，得看error的message、code之类的直接归类。看llm stream返回也是一样，避免硬编码判断返回

- 函数、class的注释尽可能简洁，只需说明模块的作用，不需要说明变更的来龙去脉

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

- 规范注释，且所有注释用中文，good case：
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


### **clean code**

> https://github.com/ryanmcdermott/clean-code-javascript
> 
> 
> 好的编程远不止是掌握语言的机制。最重要的是，需要记住程序员创建程序是为其他人将来阅读的。好的程序反映了问题陈述及其中的重要概念，它带有简洁的自我描述。示例说明了此描述，并将其与需要解决的问题联系起来。这些示例还确保未来的读者知道代码的工作原理和方式。
> 

这一部分是对于代码格式的一些建议,包括但不仅限于变量命名,函数命名,注释格式等

### **变量**

1. 使用meaningful的变量名

bad: `const yyyymmdstr = moment().format("YYYY/MM/DD");`

good: `const currentDate = moment().format("YYYY/MM/DD");`

1. 不要使用magic number

bad:

```
// What the heck is 86400000 for?
setTimeout(blastOff, 86400000);
```

good:

```
// Declare them as capitalized named constants.
const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000; //86400000;

setTimeout(blastOff, MILLISECONDS_PER_DAY);
```

1. 使用易于理解的变量名(在forEach等中)

bad:

```
[1,2,3,4].forEach(i => {
    //...
})
```

good:

```
[1,2,3,4].forEach(item => {
    // replace item with other meaningful name related to origin array
})
```

1. 如果你的class/object名表明了一些事情,不要在变量名中重复

bad:

```
const Car = {
    carColor:"blue"
}
```

good:

```
const Car = {
    color:"blue"
}
```

1. 在函数中使用default parameters

bad:

```
function tutorialExample(param) {
        const tmpParam = param || "default param";
}
```

good:

```
function tutorialExample(param = "default param") {
        //...
}
```

### **函数**

1. 限制函数的参数在两个及以下，当参数数量过多时，将他们写在一个对象里
2. 一个函数只能做一件事情，在函数中尽可能避免if等条件语句
3. 函数的名字应该说明这个函数的用处
4. 尽可能减少重复的代码，提高函数的可复用性
5. 尽可能避免Side Effects，一个函数应当尽可能**只做到接受一个值并返回一个值**

Bad:

```
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

```
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

Good:

```
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

```
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```


## 设计规范

### 单一职责原则

一个类或模块应该只有一个引起它变化的原因

### 依赖倒置原则

高层模块不应该依赖低层模块，而二者都应该依赖于抽象接口或抽象类

## 文件结构规范

- 编码前，搜索代码仓库中是否有可复用的类似实现，默认值、常量等，搜索代码库中是否有config文件，写在单独的config文件中，而不是代码文件中

- 如果一个变量/函数在不同地方出现2次及以上，将其抽离为公共配置/函数。对于公共变量，需要有Js-Doc格式的注释

## 技术架构规范

- 对于新项目，优先使用Typescript + Node22
