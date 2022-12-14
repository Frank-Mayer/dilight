# dilight

Lightweight ES5 compatible Dependency Injection Library

[![](https://img.shields.io/badge/Types-included-blue?logo=typescript&style=plastic)](https://www.typescriptlang.org)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

[![Test](https://github.com/Frank-Mayer/dilight/actions/workflows/test.yml/badge.svg)](https://github.com/Frank-Mayer/dilight/actions/workflows/test.yml)

[![Lint](https://github.com/Frank-Mayer/dilight/actions/workflows/lint.yml/badge.svg)](https://github.com/Frank-Mayer/dilight/actions/workflows/lint.yml)

## Installation

```bash
npm install @frank-mayer/dilight
```

## Usage

Construct a new `DIContainer` and register your services.

Keep in mind that the add methods return the same container instance (no new instance is created), but the type is updated with the new registered service.

**Don't do this**

```TypeScript
const container = new DIContainer();
container.addTransient(Foo, "Foo");
container.addTransient(Bar, "Bar");
```

**Do this**

```TypeScript
const container = new DIContainer()
  .addTransient(Foo, "Foo")
  .addTransient(Bar, "Bar");
```

### Interface

You can also provide a different type for the service.

```TypeScript
const container = new DIContainer()
  .addTransient<Logger>(FileLogger, "Logger");
```

### Decorators

```TypeScript
const container = new DIContainer()
  .addTransient(Foo, "Foo");

const inject = makeDecorator(container);

class Foo {
  @inject("Logger")
  private logger: Logger;
}
```

## Full Example

```TypeScript
import { DIContainer } from "@frank-mayer/dilight";

// Demo classes

class Foo { }

class Bar { }

class Baz {
  constructor(param: number) { }
}

interface ILogger {
  log(message: string): void;
}

class ConsoleLogger implements ILogger {
  log(message: string): void {
    console.log(message);
  }
}

// register services

export const container = new DIContainer()
  .addTransient(Foo, "Foo")
  .addSingleton(Bar, "Bar")
  .addFactory(() => new Baz(Math.random()), "Baz")
  .addSingleton<ILogger>(ConsoleLogger, "ILogger");

// resolve services

const foo: Foo = container.resolve("Foo");
const bar: Bar = container.resolve("Bar");
const baz: Baz = container.resolve("Baz");
const baz: ILogger = container.resolve("ILogger");
```
