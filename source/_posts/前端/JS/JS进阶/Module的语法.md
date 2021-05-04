---
title: Module的语法
cover: false
categories:
  - 前端
  - JS
  - JS进阶
tags:
  - 模块化
abbrlink: ed015086
date: 2020-09-13 10:53:58
updated:
---
ES6 在语言标准的层面上，实现了模块功能，而且实现得相当简单，完全可以取代 `CommonJS` 和 `AMD` 规范，成为浏览器和服务器通用的模块解决方案。

- ES6 模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系（`CommonJS` 和 `AMD`运行时确定），以及输入和输出的变量。
- ES6 模块不是对象，这导致没法引用 ES6 模块本身
- ES6 通过`export`命令显式指定输出的代码，再通过import命令输入。
- ES6 的模块自动采用严格模式，

## 基本命令
### export 命令
一个模块就是一个独立的文件。该文件内部的所有变量、函数、类，外部无法获取。如果你希望外部能够读取模块内部的某个变量，就必须使用`export`关键字输出该变量。
```js
export var firstName = 'Michael';

var lastName = 'Jackson';
export { lastName };

export function multiply(x, y) {
  return x * y;
};
```

- 可以直接输出，或者将它们的名字用`{}`包裹再输出（推荐），用`as`可以重命名接口名字
- `export`语句输出的接口，与其对应的值是动态绑定关系，即通过该接口，可以取到模块内部实时的值。
- `export`命令可以出现在模块的任何位置，只要处于模块顶层就可以，代码块中无法静态优化，会报错。

### `import` 命令
`import`命令接受一对大括号，里面指定要从其他模块导入的变量名。大括号里面的变量名，必须与被导入模块（profile.js）对外接口的名称相同。

```js
import { firstName, lastName, year } from './profile.js';
```

- 输入的变量、函数、类是只读的（属性值可以改写，并作用到其他地方，不建议这样做），但同样可以用`as`重命名
- `import`后面的`from`指定模块文件的位置，可以是相对路径，也可以是绝对路径，.js后缀可以省略。
- `import`命令具有提升效果，会提升到整个模块的头部，首先执行，本质：`import`命令是编译阶段执行的，在代码运行之前
- 用星号（*）可以整体加载

### `export default` 命令
使用`import`命令的时候，用户需要知道所要加载的变量名或函数名，否则无法加载，`export default`命令，为模块指定默认输出。

- `export default`命令用在非匿名函数前，也是可以的，只不过默认匿名，非匿名的函数名在模块外部是无效的。
- 其他模块加载该模块时，import命令可以为该匿名函数指定任意名字。
- `import`命令后面，不使用大括号，本质：一个模块只能有一个默认输出，因此`export default`命令只能使用一次.

	```js
	// 错误
	export default var a = 1;
	// 正确
	export default 1;
	```
	> 它等效于输出一个叫做default的变量或方法，然后系统允许你为它取任意名字，所以它后面不能跟变量声明语句。

- `import`命令可以同时输入默认方法和其他接口

### `import()` 
`import`函数的参数为指定所要加载的模块的位置。`import`命令能够接受什么参数，`import()`函数就能接受什么参数，两者区别主要是后者为动态加载。
- `import()`可以按需加载，条件加载，动态的模块路径。
- `import()`返回一个 `Promise` 对象，模块会作为一个对象，当作`then`方法的参数
	> 可以使用对象解构赋值的语法，获取输出接口。

- `import()`类似于 Node 的`require`方法，区别主要是前者是异步加载，后者是同步加载。
- 同时加载多个模块
	```js
	Promise.all([
	  import('./module1.js'),
	  import('./module2.js'),
	  import('./module3.js'),
	])
	.then(([module1, module2, module3]) => {
	   ···
	});
	```


## 其他
### `export` 与 `import` 的复合写法
如果在一个模块之中，先输入后输出同一个模块，`import`语句可以与`export`语句写在一起。
```js
export { foo, bar } from 'my_module';

// 可以简单理解为
import { foo, bar } from 'my_module';
export { foo, bar };
```
- 写成一行以后，foo和bar实际上并没有被导入当前模块，只是相当于对外转发了这两个接口，导致当前模块不能直接使用foo和bar。
- 模块之间也可以继承。

### 跨模块常量
`const`声明的常量只在当前代码块有效，模块化可以使这些常量跨多个文件，或者说一个值要被多个模块共享。

如果要使用的常量非常多，可以建一个专门的`constants`目录，将各种常量写在不同的文件里面，保存在该目录下。
```js
// constants/db.js
export const db = {
  url: 'http://my.couchdbserver.local:5984',
  admin_username: 'admin',
  admin_password: 'admin password'
};

// constants/user.js
export const users = ['root', 'admin', 'staff', 'ceo', 'chief', 'moderator'];
然后，将这些文件输出的常量，合并在index.js里面。

// constants/index.js
export {db} from './db';
export {users} from './users';
使用的时候，直接加载index.js就可以了。

// script.js
import {db, users} from './constants/index';
```
### 加载规则
- 浏览器加载 ES6 模块，也使用`<script>`标签，但是要加入`type="module"`属性
- 过程是异步加载，不会造成堵塞浏览器，即等到整个页面渲染完，再执行模块脚本，等同于打开了`<script>`标签的defer属性。
- 同一个模块如果加载多次，将只执行一次。
- ES6 模块也允许内嵌在网页中，语法行为与加载外部脚本完全一致。
	```js
	<script type="module">
	  import utils from "./utils.js";
	
	  // other code
	</script>
	```