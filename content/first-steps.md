### First steps

在本系列文章中，您将了解 Nest 的**核心基础知识**。为了熟悉 Nest 应用进程的基本构建块，我们将构建一个基本的 CRUD 应用进程，该应用进程的功能涵盖了入门级的很多内容。

#### Language

我们喜欢 [TypeScript](https://www.typescriptlang.org/)，但我们更喜欢 [Node.js](https://nodejs.org/en/)。这就是 Nest 与 TypeScript 和纯 JavaScript 兼容的原因。Nest用了最新的语言功能，因此要将其与原生JavaScript一起使用，我们需要一个[Babel](https://babeljs.io/) 编译器。

我们在提供的示例中主要使用 TypeScript，但您始终可以**将代码片段**切换为原始JavaScript语法（只需单击每个代码片段右上角的语言按钮即可切换）。

#### Prerequisites

请确保你的操作系统上安装了 [Node.js](https://nodejs.org) (版本 >= 16)。

#### Setup

使用 [Nest CLI](/cli/overview) 设置新项目非常简单。安装 [npm](https://www.npmjs.com/) 后，您可以在 OS 终端中使用以下命令创建一个新的 Nest 项目：

```bash
npm i -g @nestjs/cli
nest new project-name
```

> info **提示** 要使用 TypeScript 的 [stricter](https://www.typescriptlang.org/tsconfig#strict) 功能集创建新项目，请将 `--strict` 标志传递给 `nest new` 命令。

我们将创建`project-name`目录，安装node模块和一些其他模板文件，并创建`src/`目录并写入几个核心文件。

<div class="file-tree">
  <div class="item">src</div>
  <div class="children">
    <div class="item">app.controller.spec.ts</div>
    <div class="item">app.controller.ts</div>
    <div class="item">app.module.ts</div>
    <div class="item">app.service.ts</div>
    <div class="item">main.ts</div>
  </div>
</div>

以下是这些核心的简要概述：

| | |
|-|-|
| `app.controller.ts`      | 具有单一路由的基本控制器。|
| `app.controller.spec.ts` | 控制器的单元测试|
| `app.module.ts`          | 应用的根模块|
| `app.service.ts`         | 有单一方法的一个基础服务|
| `main.ts`                | 应用进程的入口文件，使用核心函数`NestFactory`创建 Nest 应用进程实例。|

`main.ts`文件包含一个异步函数来**启动**我们的应用。

```typescript
@@filename(main)

import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(process.env.PORT ?? 3000);
}
bootstrap();
@@switch
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(process.env.PORT ?? 3000);
}
bootstrap();
```

To create a Nest application instance, we use the core `NestFactory` class. `NestFactory` exposes a few static methods that allow creating an application instance. The `create()` method returns an application object, which fulfills the `INestApplication` interface. This object provides a set of methods which are described in the coming chapters. In the `main.ts` example above, we simply start up our HTTP listener, which lets the application await inbound HTTP requests.要创建 Nest 应用进程实例，我们使用核心的`NestFactory`类。`NestFactory`公开了一些允许创建应用进程实例的静态方法。`create()`方法返回一个应用进程对象，该对象实现`INestApplication`接口。此对象提供了一组方法，将在接下来的章节中介绍。在上面的`main.ts`示例中，我们只需启动 HTTP 监听器，它就可以让应用进程等待即将进入 HTTP 请求。

Note that a project scaffolded with the Nest CLI creates an initial project structure that encourages developers to follow the convention of keeping each module in its own dedicated directory.

> info **Hint** By default, if any error happens while creating the application your app will exit with the code `1`. If you want to make it throw an error instead disable the option `abortOnError` (e.g., `NestFactory.create(AppModule, {{ '{' }} abortOnError: false {{ '}' }})`).

<app-banner-courses></app-banner-courses>

#### Platform

Nest aims to be a platform-agnostic framework. Platform independence makes it possible to create reusable logical parts that developers can take advantage of across several different types of applications. Technically, Nest is able to work with any Node HTTP framework once an adapter is created. There are two HTTP platforms supported out-of-the-box: [express](https://expressjs.com/) and [fastify](https://www.fastify.io). You can choose the one that best suits your needs.

|                    |                                                                                                                                                                                                                                                                                                                                    |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `platform-express` | [Express](https://expressjs.com/) is a well-known minimalist web framework for node. It's a battle tested, production-ready library with lots of resources implemented by the community. The `@nestjs/platform-express` package is used by default. Many users are well served with Express, and need take no action to enable it. |
| `platform-fastify` | [Fastify](https://www.fastify.io/) is a high performance and low overhead framework highly focused on providing maximum efficiency and speed. Read how to use it [here](/techniques/performance).                                                                                                                                  |

Whichever platform is used, it exposes its own application interface. These are seen respectively as `NestExpressApplication` and `NestFastifyApplication`.

When you pass a type to the `NestFactory.create()` method, as in the example below, the `app` object will have methods available exclusively for that specific platform. Note, however, you don't **need** to specify a type **unless** you actually want to access the underlying platform API.

```typescript
const app = await NestFactory.create<NestExpressApplication>(AppModule);
```

#### Running the application

Once the installation process is complete, you can run the following command at your OS command prompt to start the application listening for inbound HTTP requests:

```bash
npm run start
```

> info **Hint** To speed up the development process (x20 times faster builds), you can use the [SWC builder](/recipes/swc) by passing the `-b swc` flag to the `start` script, as follows `npm run start -- -b swc`.

This command starts the app with the HTTP server listening on the port defined in the `src/main.ts` file. Once the application is running, open your browser and navigate to `http://localhost:3000/`. You should see the `Hello World!` message.

To watch for changes in your files, you can run the following command to start the application:

```bash
npm run start:dev
```

This command will watch your files, automatically recompiling and reloading the server.

#### Linting and formatting

[CLI](/cli/overview) provides best effort to scaffold a reliable development workflow at scale. Thus, a generated Nest project comes with both a code **linter** and **formatter** preinstalled (respectively [eslint](https://eslint.org/) and [prettier](https://prettier.io/)).

> info **Hint** Not sure about the role of formatters vs linters? Learn the difference [here](https://prettier.io/docs/en/comparison.html).

To ensure maximum stability and extensibility, we use the base [`eslint`](https://www.npmjs.com/package/eslint) and [`prettier`](https://www.npmjs.com/package/prettier) cli packages. This setup allows neat IDE integration with official extensions by design.

For headless environments where an IDE is not relevant (Continuous Integration, Git hooks, etc.) a Nest project comes with ready-to-use `npm` scripts.

```bash
# Lint and autofix with eslint
$ npm run lint

# Format with prettier
$ npm run format
```
