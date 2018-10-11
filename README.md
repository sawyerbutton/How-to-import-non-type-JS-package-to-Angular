# How-to-import-non-type-JS-package-to-Angular

## 如何在Angular中导入和使用非@type类型的JS库

> TypeScript是JavaScript的超集,可编译为纯JavaScript

> 但是TypeScript有自己的语法,函数和规定类型的变量,但是当要使用外部库(比如underscore)时,需要声明TypeScript的类型定义

> 在JavaScript中参数类型并不重要,因为在编写代码时不会出错(虽然可能在编译阶段报错),但TypeScript不允许向一个接收Number类型的数组传入String类型的变量

> 但是这并不意味着需要将JavaScript类库重写为TypeScript类库,TypeScript提供声明文件`(* .d.ts)`用于定义类型并标准化既存的JavaScript文件/库

> 随着TypeScript的发展,越来越多的JavaScript文件/库已经提供了TypeScript类型声明文件,只需要在[Type Search](https://microsoft.github.io/TypeSearch/)中搜索所期待的库并通过npm或yarn下载即可

### 如何向Angular中导入包含TypeScript类型声明的库以及如何导入不包含类型声明的库

> 以unders举例来说,先通过包管理器安装`types/underscore`

```bash
npm install --save @types/underscore
```

> 在组件中引入underscore的模块

```typescript
import * as _ from 'underscore';
/**
* OR simply:
* import 'underscore';
*/

let array = [1,2,3,4,5];
let lastNumber = _.last(array);
console.log(lastNumber);
// 5
```
> 假设库`your-library`无法下载到types版本,亦即没有`(*.d.ts)`文件,

> 那么检测是否存在文件`src/typings.d.ts`,如若不存在则创建,在文件存在的条件下添加

```
declare module 'your-library'
```

> 之后希望在TypeScript文件中使用时只需要

```typescript
import * as yourPreferedName from 'your-library';
yourPreferedName.method();
```

### 如何向Angular中导入不存在于npm或yarn库中的库或模块

> 情况下,开发Angular默认会使用NPM或者YARN其中一个作为项目的库管理器,并从`node_modules`中导入相应的`@type模块`应用于组件之中

> 但是总有一些状况导致项目开发需要的库并不能由npm或yarn导入,比如未发布的库或者私人开发的js模块等

> 更常见的状况是需要在Angular项目中使用非`@type类型`的JS库

> 使用Angular-cli启动的项目可以通过`Angular.json`文件进行配置外源性库或模块

> 假设外源性库`customPkg`放置在于src目录下的`assets/packages`目录下,则可以进行如下配置(位于angular.json)文件

```json
 "build": {
          "builder": "@angular-devkit/build-angular:browser",
          "options": {
            "outputPath": "dist/subscribe-observables-inangular",
            "index": "src/index.html",
            "main": "src/main.ts",
            "polyfills": "src/polyfills.ts",
            "tsConfig": "src/tsconfig.app.json",
            "assets": [
              "src/favicon.ico",
              "src/assets"
            ],
            "styles": [
              "src/styles.css"
            ],
            "scripts": ["../src/assets/customPkg/dist/index.js"]
          },
```

> Angular-Cli会将`Angular.json`中指定的脚本`"scripts": ["../src/assets/customPkg/dist/index.js"]`库含在`scripts.bundle.js`中并自动添加到Angular程序中

> 同样,设置于`"styles": ["src/styles.css"]`属性中的样式也会被库含在`styles.bundle.js`中并自动添加到Angular程序中

> 在`src`文件目录下创建`typings.d.ts`文件并申明你所希望引入的库

```JavaScript
declare module 'customPkg'
```

> 现在就可以在项目的组件中引入库并使用了

```TypeScript
import * as yourPreferedName from 'customPkg';
yourPreferedName.method();
```

