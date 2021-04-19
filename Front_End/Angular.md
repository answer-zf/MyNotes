# Angular

## 起步

### Step 0. 安装依赖环境

1.  安装 Node.js
2.  安装 npm
3.  安装 Python（ 使用 Python 2 ）
    -   <https://www.python.org/downloads/release/python-2717/>
        -   Windows x86-64 MSI installer
    -   确认 Python 环境 python --version
4.  安装 C++ 编译工具

-   Angular CLi 在 Windows 上同时依赖 C++ 编译工具。
-   执行下面的命名安装 C++ 编译工具：
    -   npm install --global --production windows-build-tools

### Step1. 安装脚手架工具 Angular CLI

#### 安装

```shell
  cnpm i -g @angular/cli
```

#### 确认

```shell
  ng --version
```

### Setp 2. 使用脚手架工具初始化项目

```shell
  ng new my-app
```

> 请特别注意：Angular CLI 在自动生成好项目骨架之后，会立即自动使用 npm 来安装所依赖的 Node 模块，自己手动 Ctrl + C 终止掉，然后进入初始化好的项目根目录使用 cnpm 来安装。

### Step 3. Serve the application

项目目录下：

```shell
  # 或者 npm start
  ng serve
```

> 该命令默认会开启一个服务占用 4200 端口，如果想要修改可以通过 --port 参数来指定，例如 ng serve --port 3000

## 简介

### 目录结构

> <https://www.cnblogs.com/nightnight/p/11186387.html>

### angular.json 部分配置

```json
{
  "projects": {
    "ng-text": {
      "projectType": "application",
      "schematics": {
        "@schematics/angular:component": {
          "style": "scss"
        }
      },
      "root": "", // 项目根目录
      "sourceRoot": "src", // 源码根目录
      "prefix": "app", // 使用脚手架工具创建组件的自动命名前缀
      "architect": {
        // 自动化命令配置
        "build": {
          // 项目默认生成的配置
          "builder": "@angular-devkit/build-angular:browser",
          "options": {
            "outputPath": "dist/ng-text", // 打包编译结果目录
            "index": "src/index.html", // 单页面
            "main": "src/main.ts", // 模块启动入口
            "polyfills": "src/polyfills.ts", // 用以兼容低版本浏览器不支持的 JavaScript 语法特性
            "tsConfig": "tsconfig.app.json",
            "aot": true,
            "assets": ["src/favicon.ico", "src/assets"], // 存放静态资源目录
            "styles": ["src/styles.scss"], // 全局样式文件
            "scripts": [] // 全局脚本文件
          }
        }
      }
    }
  },
  "defaultProject": "ng-text"
}
```

## TypeScript

### 环境

1.  在线测试编译环境 compiler
    -   <https://www.typescriptlang.org/play/index.html>
2.  本地开发编译环境

```shell
  npm i -g typescript
  # 使用 tsc 文件路径 编译成 js
```

### 语法：

#### 数据类型：

```typescript
// 布尔类型
let isDone: boolean = false

// 数字
let amount: number = 6

// 字符串
let nickname: string = '张三'

// 模板字符串同样兼容
let nickname: string = `Gene`
let age: number = 37
let sentence: string = `Hello, my nickname is ${nickname}.I'll be ${age +
  1} years old next month.`

// 数组 在元素类型后面接上[]
let list: number[] = [1, 2, 3]
let list: string[] = ['z', 'f']
// 使用数组泛型，Array<元素类型>
let list: Array<number> = [1, 2, 3]

// 元组：有不同数据类型的数组
let arr: [number, string] = [1, 'zf'] // 一一对应的关系，不能多，不能少，不能交错

// 对象：
let obj: object = { name: 'zf' } // 一般使用这种方式

let user: {
  // 使用这种方式
  name: string
  age: number
} = {
  name: 'zf',
  age: 8
}
```

##### 特殊情况

**任意类型：**

```typescript
// Any: 任意类型 （少用）
let zfs: any = 'sadf'
zfs = 111
```

**函数中应用**

-   用于函数的形参

```typescript
function add(x: number, y: number): number {
  return x + y
}
let ret: number = add(10, 20)
// viod：表示没有任何类型 -> 空（ 只能用于函数的返回值 ）
function fn(): void {
  console.log('zf')
}
```

-   可选参数

```typescript
function add(x: number, y?: number): number {
  return x + 10
}
```

-   默认参数

```typescript
function add(x: number, y: number = 20): number {
  return x + y
}
```

-   剩余参数

```typescript
function sum(...args: number[]): number {
  let ret: number = 0
  args.forEach((item: number): void => {
    ret += item
  })
  return ret
}

sum(1, 2, 3)
```

-   箭头函数

```typescript
let add = (x: number, y: number): number => x + y
```

**Null 和 Undefined**

-   和 void 相似，它们的本身的类型用处不是很大：

```typescript
// 几乎不用
let u: undefined = undefined
let n: null = null
```

#### 接口

-   重用

```typescript
interface Person {
  name: string
  age: number
}
let xyz: Person = {
  name: 'zddf',
  age: 888
}
```

#### 解构赋值

**数组**

-   数组按照 顺序 解构

```typescript
let arr: number[] = [10, 20]
let [num1, num2, num3] = arr
```

**对象**

-   对象按照 键名 解构

```typescript
let obj: {
  name: string
  age: number
} = {
  name: 'zf',
  age: 18
}
let { name: zf, age } = obj
```

**剩余参数**

```typescript
function sum(...args: number[]): number {
  let ret = 0
  args.forEach(item => {
    ret += item
  })
  return ret
}
sum(1, 2, 3)
```

#### 展开操作符

```typescript
// 数组
let arr1 = [1, 2, 3]
let arr2 = [4, 5, 6]
let arr3 = [...arr1, ...arr2]

// 对象
let obj1 = {
  foo: 'bar'
}
let obj2 = {
  ...obj1,
  name: 'jack'
}
```

#### 类

```typescript
// 非 es6 语法
function Person(name: string, age: number) {
  this.name = name
  this.age = age
}
Person.prototype.sayHello = function(): void {
  console.log(this.name, this.age)
}
let p1 = new Person('zhans', 18)
p1.sayHello()
```

```typescript
class Person {
  // ts 要求声明类成员
  name: string
  age: number
  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }
  sayHello(): void {
    console.log(this.age, this.name)
  }
}
let p1 = new Person('zhans', 18)
p1.sayHello()

// 继承
class Student extends Person {
  constructor(name: string, age: number) {
    // super 就是 父类 构造函数
    super(name, age)
  }
}
```

##### 实例成员访问修饰符

-   `public` 默认值 公开的
    -   类的成员默认是对外公开的
-   `private` 私有的
    -   用来声明私有成员，只能在类的内部访问，外部访问不到
    -   私有成员 无法继承
-   `protected` 受保护的
    -   与 private 类似 私有成员，外部无法直接访问，但可以被继承
-   `readonly` 只读的，不允许修改，与 const 定义的常量类似

```typescript
// 类的成员默认是对外公开的
class Person {
  public name: string
  public age: number
  // 可以在声明类成员的同时赋值
  private readonly type: string = 'zf'
  protected foo: string = 'bar'
  public constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }
  public getType() {
    // 在类的内部访问私有成员
    // 在外部无法访问
    return this.type
  }
}

// protected 的成员只能在子类内部访问，不能在子类外部访问
class Student extends Person {
  getFoo() {
    console.log(this.foo)
  }
}

new Student('z', 1).getFoo()
```

**利用时间修饰符做简化**

> 可混写

```typescript
class Person {
  public foo: string
  constructor(public name: string, public age: number, foo: string) {
    this.name = name
    this.age = age
    this.foo = foo
  }
}
```

##### 属性的存值，储值器(get，set)

```typescript
class Person {
  private _age: number = 1 // 不在 constructor 中引入的成员 必须赋值
  // 当访问 实例.age 的时候会调用 get 方法
  get age() {
    return this._age
  }
  // 当对 实例.age = xxx 赋值的时候会调用 set 方法
  set age(val) {
    if (val < 0) {
      throw new Error('不能为复数')
    }
    this._age = val
  }
}
let p1 = new Person()
p1.age = -10 // 运行代码报错，ts 内编译不报错
```

##### 实例成员与静态成员

-   实例成员：只能通过 new 出来的实例访问
-   静态成员：即类本身的成员，只能通过类本身访问

> 所有的成员默认是实例成员 ，加上 static 关键字，变为静态成员

```typescript
class Person {
  static type: string = 'zf'
  static sayHello() {
    console.log('zf..zf')
  }
}
console.log(Person.type) // 不能使用 new Person().type
Person.sayHello()
```

#### 循环

-   forEach
    -   不支持 break
-   for in
    -   会把数组当作对象来遍历
-   for of
    -   支持 break

```typescript
let arr: number[] = [1, 2, 3]

for (let key in arr) {
  // key => 索引
  console.log(key) // => 0 1 2
}

for (let val of arr) {
  // val => 值
  // if (val === 2) {
  //   break
  // } => 1
  console.log(val) // => 1 2 3
}
```

#### 模块导入、导出

##### 导出

```typescript
export default xxx

export const foo: string = 'bar'
export const bar: string = 'foo'
```

##### 导入

```typescript
// 加载默认成员
import xxx from '模块标识'

// 按需加载模块成员
import { foo, bar } from '模块'
```

## 组件

```typescript
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
  title = 'ng-text111'
}
```

-   @Component：这是一个 Decorator（装饰器），其作用类似于 Java 里面的注解。Decorator 这个语言特性目前（2017-10）处于 Stage 2（草稿）状态，还不是 ECMA 的正式规范。
-   selector：组件的标签名，外部使用者可以这样来使用这个组件：。默认情况下，ng 命令生成出来的组件都会带上一个 app 前缀，如果你不喜欢，可以在 angular.json 里面修改 prefix 配置项，设置为空字符串将会不带任何前缀。
-   templateUrl：引用外部的 HTML 模板。如果你想直接编写内联模板，可以使用 template，支持 ES6 引入的“模板字符串”写法。
-   styleUrls：引用外部 CSS 样式文件，这是一个数组，也就意味着可以引用多份 CSS 文件。
-   export class AppComponent：这是 ES6 里面引入的模块和 class 定义方式。

## Project start

```shell
# 创建
ng new projectname
# 运行
npm start
```

## 模板语法

### 循环

可以使用 let 声明索引

```html
<div *ngFor="let item of arr; let i = index"></div>
```

### 条件

```html
<div *ngIf="condition"></div>

<!-- 多个重复的条件可用 -->
<ng-template [ngIf]="condition"></ng-template>
```

### 事件

```html
<div (keyup.enter)="addTodo($event)"></div>
```

```javascript
...
addTodo(e):void {
  // 控制台输出键盘码
  console.log(e.keyCode);
}
```

### NgClass 指令

```html
<div [ngClass]="{css类名: 布尔值,css类名: 布尔值}">测试文本</div>
```

> NgClass 指令接收一个对象，对象的 key 指定 css 类名，value 给定一个布尔值，当布尔值为真则作用该类名，当布尔值为假则移除给类名。

### 表单

#### 双向绑定

##### 配置项

```typescript
// └─ app.module.ts
// 载入 表单处理模块
import { FormsModule } from "@angular/forms";
// 声明 该模块
@NgModule({
// ...
  imports: [BrowserModule, AppRoutingModule, FormsModule],
// ...
})
```

##### 使用

```html
<div [(ngModule)]="data"></div>
```

## 路由

### 配置

```typescript
// ---------------app-routing.module.ts

import { NgModule } from '@angular/core'
import { Routes, RouterModule } from '@angular/router'

// 引入模块
import { LayoutComponent } from './layout/layout.component'
// 子模块
import { ContactListComponent } from './contact-list/contact-list.component'
import { ContactEditComponent } from './contact-edit/contact-edit.component'
import { ContactNewComponent } from './contact-new/contact-new.component'
// 配置路由
const routes: Routes = [
  // pathMatch, 必须完全匹配 路径 时，才做重定向，重定向的必备条件
  {
    path: '',
    redirectTo: 'contacts',
    pathMatch: 'full'
  },
  { path: 'layout', component: LayoutComponent },
  {
    path: 'contacts',
    component: LayoutComponent,
    canActivate: [AuthGuard], // 路由守卫
    // 子模块
    children: [
      { path: '', component: ContactListComponent },
      { path: 'edit/:id', component: ContactEditComponent }, // 路由传参
      { path: 'new', component: ContactNewComponent }
    ]
  }
]

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
```

### API 跳转

```typescript
import { Router } from '@angular/router'

export class SignupComponent implements OnInit {
  constructor(private router: Router) {}
  signupHandle() {
    this.router.navigate(['/'])
  }
}
```

### 模板使用路由跳转

```html
<a routerLink="/router/:id"></a>
```

**获取路由传递的参数**

```typescript
// 使用 ActivatedRoute 模块
import { Component, OnInit } from '@angular/core'
import { Router, ActivatedRoute } from '@angular/router'
import { HttpClient } from '@angular/common/http'
import { GlobalVariable } from '../globals'

@Component({
  selector: 'app-contact-edit',
  templateUrl: './contact-edit.component.html',
  styleUrls: ['./contact-edit.component.scss']
})
export class ContactEditComponent implements OnInit {
  formData = {
    name: '',
    email: '',
    phone: ''
  }
  constructor(
    private http: HttpClient,
    private router: Router,
    private route: ActivatedRoute
  ) {}

  ngOnInit(): void {
    this.getEditContactHandle()
  }

  getEditContactHandle() {
    const id = this.route.snapshot.params.id
    console.log(id)
  }
  editContactByIdHandle() {}
}
```

### 设置响应头

```typescript
import { HttpClient, HttpHeaders } from '@angular/common/http'
export class ContactListComponent implements OnInit {
  constructor(private http: HttpClient) {}
  ngOnInit(): void {
    this.http
      .get('http://.../contacts', {
        headers: new HttpHeaders().set(
          'X-Access-Token',
          window.localStorage.getItem('auth_token')
        )
      })
      .toPromise()
      .then(data => {
        console.log(data)
      })
      .catch(err => {
        console.log(err)
      })
  }
}
```

### 设置拦截器

1.  添加模块

```typescript
// ------------ ./global.interceptor.ts
import { Injectable } from '@angular/core'
import {
  HttpEvent,
  HttpInterceptor,
  HttpHandler,
  HttpRequest
} from '@angular/common/http'

import { Observable } from 'rxjs'

/** Pass untouched request through to the next request handler. */
@Injectable()
export class GlobalInterceptor implements HttpInterceptor {
  intercept(
    req: HttpRequest<any>,
    next: HttpHandler
  ): Observable<HttpEvent<any>> {
    const token = window.localStorage.getItem('auth_token')
    const authReq = req.clone({
      // 自定义响应头
      headers: req.headers.set('X-Access-Token', token)
    })
    return next.handle(authReq)
  }
}
```

2.  模块配置

```typescript
// ------------- app.module.ts

// http配置
import { HTTP_INTERCEPTORS } from '@angular/common/http'
// 拦截器配置
import { GlobalInterceptor } from './global.interceptor'

@NgModule({
  // ...
  providers: [
    { provide: HTTP_INTERCEPTORS, useClass: GlobalInterceptor, multi: true }
  ]
})
```

### 路由守卫（登录验证）

1.  创建路由验证文件

```typescript
// auth-guard.ts
import { Injectable } from '@angular/core'
import { CanActivate, Router } from '@angular/router'

@Injectable()
export class AuthGuard implements CanActivate {
  constructor(private router: Router) {}
  canActivate(): boolean {
    const token = window.localStorage.getItem('auth_token')
    if (!token) {
      this.router.navigate(['/signup'])
      return false
    }
    return true
  }
}
```

2.  配置

```typescript
// admin-routing.module.ts
import { AuthGuard } from './auth-guard'
@NgModule({
  // ...
  providers: [AuthGuard]
})
```

3.  使用

```typescript
// 在需要使用守卫的路由添加
const routes: Routes = [
  {
    path: '...',
    canActivate: [AuthGuard]
  }
]
```

## 请求

1.  模块配置

```typescript
// ------------------- app.module.ts
// http 模块配置
import { HttpClientModule } from '@angular/common/http'

@NgModule({
  // ...
  imports: [HttpClientModule]
})
```

2.  使用配置

```typescript
import { HttpClient } from '@angular/common/http'

export class SignupComponent implements OnInit {
  constructor(private http: HttpClient) {}
}
```

3.  使用

```typescript
  signupHandle() {
    const formData = this.signupForm
    this.http
      .post('http://localhost:3000/users', formData)
      .toPromise()
      .then((data: any) => {
        this.email_err_msg = ''
        window.localStorage.setItem('auth_token', data.token)
        this.router.navigate(['/'])
      })
      .catch(err => {
        if (err.status === 409) {
          this.email_err_msg = 'Email Conflict'
        }
      })
  }
```
