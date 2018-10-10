# How-to-import-non-type-JS-package-to-Angular

## 如何在Angular中导入和使用非@type类型的JS包

> 通常情况下，开发Angular默认会使用NPM或者YARN其中一个作为项目的包管理器，并从`node_modules`中导入相应的`@type模块`应用于组件之中

> 但是总有一些状况导致项目开发需要的包并不能由npm或yarn导入，比如未发布的包或者私人开发的js模块等

> 更常见的状况是需要在Angular项目中使用非`@type类型`的JS包

### 如何向Angular中导入不存在于npm或yarn库中的包或模块

> 使用Angular-cli启动的项目可以通过`Angular.json`文件进行配置外源性包或模块

> 假设外源性包`customPkg`放置在于src目录下的`assets/packages`目录下,可以通过如下配置

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

> Angular-Cli会将`Angular.json`中指定的脚本`"scripts": ["../src/assets/customPkg/dist/index.js"]`包含在`scripts.bundle.js`中并自动添加到Angular程序中

> 同样,设置于`"styles": ["src/styles.css"]`属性中的样式也会被包含在`styles.bundle.js`中并自动添加到Angular程序中

> 在`src`文件目录下创建`typings.d.ts`文件并申明你所希望引入的包

```javascript
declare module 'customPkg'
```

> 现在就可以在项目的组件中引入包并使用了

```typescript
import * as yourPreferedName from 'customPkg';
yourPreferedName.method();
```

