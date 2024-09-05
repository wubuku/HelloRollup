# README


这是一个示例性的 JS 库，使用 TypeScript 编写，并使用 Rollup 进行构建。
支持多种模块格式，包括 CommonJS、ES Module 和 UMD。


## package.json 文件解释

```json
{
  "name": "math-lib",
  "version": "1.0.0",
  "type": "module",
  "main": "dist/index.cjs.js",
  "module": "dist/index.esm.js",
  "browser": "dist/index.umd.js",
  "types": "dist/index.d.ts",
  "exports": {
    "import": "./dist/index.esm.js",
    "require": "./dist/index.cjs.js"
  },
  "scripts": {
    "build": "rollup -c"
  },
  "files": [
    "dist"
  ],
  "keywords": [
    "math",
    "calculator",
    "typescript"
  ],
  "author": "Your Name",
  "license": "MIT",
  "devDependencies": {
    "@rollup/plugin-commonjs": "^25.0.8",
    "@rollup/plugin-node-resolve": "^15.2.3",
    "@rollup/plugin-typescript": "^11.1.6",
    "rollup": "^4.21.2",
    "rollup-plugin-dts": "^6.1.1",
    "typescript": "^5.0.0"
  }
}
```

- `name`: 包名，这里是 "math-lib"
- `version`: 版本号，当前为 1.0.0
- `type`: 指定为 ES 模块
- `main`: CommonJS 入口文件路径
- `module`: ES 模块入口文件路径
- `browser`: 浏览器使用的 UMD 文件路径
- `types`: TypeScript 类型声明文件路径
- `exports`: 模块导出配置
  - `import`: ES 模块导入路径
  - `require`: CommonJS 导入路径
- `scripts`: 
  - `build`: 使用 Rollup 构建项目的命令
- `files`: npm 包中包含的文件，这里只包含 dist 目录
- `keywords`: 包的关键词，用于 npm 搜索
- `author`: 包的作者
- `license`: 使用的许可证，这里是 MIT
- `devDependencies`: 开发依赖列表
  - 包含 Rollup 及其插件
  - TypeScript 编译器

这个配置文件定义了一个支持多种模块格式的 npm 包，使用 Rollup 进行构建，并包含了必要的开发依赖。


## TypeScript 配置解释

### tsconfig.json 文件解释

```json
{
    "compilerOptions": {
        "target": "es2015",
        "module": "esnext",
        "declaration": true,
        "outDir": "./dist",
        "strict": true,
        "esModuleInterop": true
    },
    "include": ["src/**/*"],
    "exclude": ["node_modules", "dist"]
}
```


1. `"compilerOptions"`: 这个对象包含了 TypeScript 编译器的配置选项。

2. `"target": "es2015"`: 指定编译后的 JavaScript 版本为 ES2015（ES6）。这是一个较现代的版本，支持大多数现代浏览器。

3. `"module": "esnext"`: 指定生成的模块代码。`"esnext"` 表示使用最新的 ECMAScript 模块语法。

4. `"declaration": true`: 生成相应的 `.d.ts` 文件，这些文件包含了类型声明。

5. `"outDir": "./dist"`: 指定编译后文件的输出目录为 `./dist`。

6. `"strict": true`: 启用所有严格类型检查选项，有助于捕获更多潜在的错误。

7. `"esModuleInterop": true`: 允许使用 `import` 语法导入 CommonJS 模块，增强了与非 ES 模块的互操作性。

8. `"include": ["src/**/*"]`: 指定哪些文件应该被 TypeScript 编译器处理。这里包含了 `src` 目录下的所有文件。

9. `"exclude": ["node_modules", "dist"]`: 指定哪些文件或目录应该被排除在编译过程之外。

这个配置适合于开发一个现代的 TypeScript 库或应用，它生成 ES2015 兼容的代码，使用最新的模块语法，并启用了严格的类型检查。如果你需要支持更老的环境，可能需要将 `target` 调整为 `"es5"`。如果你主要针对最新的浏览器或 Node.js 环境，可以考虑使用更高的 target，如 `"es2018"` 或更新版本。


### 关于 target 选项

`target` 的主要选项包括：

1. `ES3`：最旧的 ECMAScript 标准
2. `ES5`：广泛支持的版本，兼容 IE11
3. `ES6`/`ES2015`：引入了许多现代特性
4. `ES2016` 到 `ES2022`：每年发布的新版本
5. `ESNext`：最新的 ECMAScript 版本
6. `Node12`、`Node14`、`Node16` 等：针对特定 Node.js 版本

选择 `target` 时，考虑以下因素：
- 目标环境（浏览器支持、Node.js 版本）
- 需要的 JavaScript 特性
- 性能要求
- 代码体积


## Rollup 配置解释


对 rollup.config.js 文件逐行注释解释：

```js
import resolve from '@rollup/plugin-node-resolve';
// 导入 @rollup/plugin-node-resolve 插件，用于解析第三方模块

import commonjs from '@rollup/plugin-commonjs';
// 导入 @rollup/plugin-commonjs 插件，用于将 CommonJS 模块转换为 ES6

import typescript from '@rollup/plugin-typescript';
// 导入 @rollup/plugin-typescript 插件，用于编译 TypeScript

import dts from 'rollup-plugin-dts';
// 导入 rollup-plugin-dts 插件，用于生成 TypeScript 声明文件

export default [
  // 导出一个数组，包含两个配置对象
  {
    input: 'src/index.ts',
    // 指定入口文件为 src/index.ts

    output: [
      // 定义输出配置，这里有三种不同的输出格式
      {
        file: 'dist/index.cjs.js',
        format: 'cjs',
        // 输出 CommonJS 格式的文件
      },
      {
        file: 'dist/index.esm.js',
        format: 'es',
        // 输出 ES Module 格式的文件
      },
      {
        file: 'dist/index.umd.js',
        format: 'umd',
        name: 'MathLib',
        // 输出 UMD 格式的文件，全局变量名为 MathLib
      },
    ],
    plugins: [
      // 使用的插件列表
      resolve(),
      // 解析第三方模块
      commonjs(),
      // 将 CommonJS 模块转换为 ES6
      typescript({ tsconfig: './tsconfig.json' }),
      // 编译 TypeScript，使用项目根目录下的 tsconfig.json 配置
    ],
  },
  {
    // 第二个配置对象，用于生成类型声明文件
    input: 'src/index.ts',
    // 入口文件仍然是 src/index.ts
    output: [{ file: 'dist/index.d.ts', format: 'es' }],
    // 输出 ES Module 格式的类型声明文件
    plugins: [dts()],
    // 使用 rollup-plugin-dts 插件生成类型声明
  },
];
```

## 源代码文件

文件 src/index.ts 内容如下：

```typescript
export class Calculator {
    add(a: number, b: number): number {
        return a + b;
    }

    subtract(a: number, b: number): number {
        return a - b;
    }

    multiply(a: number, b: number): number {
        return a * b;
    }

    divide(a: number, b: number): number {
        if (b === 0) {
            throw new Error("Division by zero is not allowed.");
        }
        return a / b;
    }
}
```

## 在其他项目中使用

```js
import * as mathLib from 'math-lib';
const { Calculator } = mathLib;

// 现在你可以同时使用 mathLib 和直接使用 Calculator
const calc1 = new Calculator();
const calc2 = new mathLib.Calculator();

// Log the structure of mathLib to see what's available
console.log('mathLib structure:', mathLib);

const calculator = new Calculator();
console.log(calculator.add(2, 3));
console.log(calculator.subtract(5, 2));
```

## Tips

### 模块系统

在 Node.js 和 TypeScript 项目中，主要有两种模块系统：CommonJS (CJS) 和 ECMAScript Modules (ESM)。让我们详细比较这两种系统：

#### CommonJS (CJS)

优点：
- 广泛支持，特别是在较旧的 Node.js 版本中。
- 动态导入：可以在运行时导入模块。
- 简单直接：require() 和 module.exports 使用起来很直观。

缺点：
- 不支持静态分析，这可能影响树摇（tree-shaking）和某些优化。
- 在浏览器中使用需要额外的工具（如 Browserify 或 Webpack）。
- 不支持顶级 await。

#### ECMAScript Modules (ESM)

优点：

- 现代标准，是 JavaScript 的官方模块系统。
- 支持静态分析，有利于树摇和代码优化。
- 在浏览器中原生支持。
- 支持顶级 await。
- 更好的循环依赖处理。

缺点：

- 在较旧的 Node.js 版本中支持有限。
- 某些 Node.js 特定的功能（如 __dirname）在 ESM 中不可用。
- 动态导入需要使用异步语法。

选择建议：

1. 如果您的项目是新项目，并且主要面向现代 Node.js 环境（v12.0.0 及以上），推荐使用 ESM。
2. 如果您的项目需要兼容旧版 Node.js 或大量使用 CommonJS 模块的第三方库，可能更适合使用 CommonJS。
3. 对于 TypeScript 项目，您可以在 tsconfig.json 中设置 "module": "ESNext" 来使用 ESM，或 "module": "CommonJS" 来使用 CommonJS。
4. 考虑您的目标环境：如果您的代码需要在浏览器中运行，ESM 可能是更好的选择。
5. 如果您计划发布一个库，考虑同时提供 CJS 和 ESM 版本以最大化兼容性。

#### 包装器

当使用 CommonJS 模块系统时，Node.js 会自动为每个 .js 文件添加一个包装器函数，这是 Node.js 模块系统的一个重要特性。

Node.js 使用这个包装器函数来实现以下目的：
- 保持顶级变量（在文件中定义的变量）的作用域限制在模块内部。
- 提供看似全局的特定变量，如 module, exports, require, __filename, __dirname。

当你创建一个 .js 文件时，Node.js 会在执行它之前，将其内容包装在一个函数中。这个函数大致如下：

```js
(function(exports, require, module, __filename, __dirname) {
    // 你的模块代码实际上在这里
});
```

##### 注意事项

这个包装器是 CommonJS 模块系统的一部分，不适用于 ES modules。

您可以通过以下方式检查您的环境是否使用 CommonJS：

```js
console.log(typeof require !== 'undefined');
console.log(typeof module !== 'undefined');
console.log(typeof __filename !== 'undefined');
```

尽管这个包装器函数存在，你通常不需要直接与它交互。理解这个概念有助于更好地理解 Node.js 的模块系统和作用域。

如果您的项目使用 ESM（通常通过在 package.json 中设置 `"type": "module"` 或使用 `.mjs` 文件扩展名），那么 Node.js 不会使用我上面描述的 CommonJS 包装器函数。

在 ESM 中：

- 不会自动添加包装器函数。
- 每个文件都被视为独立的模块，有自己的作用域。
- `require`, `module.exports`, `__filename`, `__dirname` 等 CommonJS 特定的变量不可用。
- 使用 `import` 和 `export` 语句来处理模块。

即使不使用 CommonJS，Node.js 仍然提供了一些全局变量和对象（如 process），但它们的提供方式不同于 CommonJS 模块系统。


##### 实际影响

你可以在模块中直接使用 require, module, exports 等，而不需要显式声明它们。
模块内的顶级 this 指向 module.exports，而不是全局对象。

---

##### 使用其他打包工具（如 Webpack, Rollup 等）

如果您使用这些工具：
- 它们通常有自己的模块处理机制。
- Node.js 的包装器函数不会直接应用。
- 这些工具可能会应用自己的包装或转换，但方式可能不同。

##### 在浏览器环境中

- 浏览器原生不支持 Node.js 的模块系统。
- 如果直接在浏览器中运行，不会有 Node.js 的包装器。
- 使用 `<script type="module">` 可以在浏览器中使用 ESM。

##### 使用 TypeScript

TypeScript 本身不使用 Node.js 的包装器。

编译后的 JavaScript 代码的行为取决于您的 TypeScript 配置和目标环境。

