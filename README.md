# Модули

Модуль - файл, содержащий `import` или `export` верхнего уровня.
Файл, не содержащий указанных ключевых слов, является глобальным скриптом.

У модулей есть своя область видимости.

Все модули имеют определенный формат и относятся к определенном типу.
Популярные - 
- AMD
- CommonJS
- ES
 
##  синтаксис: 
```

export default function helloWorld() {
console.log('Привет, народ!') 
}

```

`import hello from './hello.js' hello()`

из файла может экспортироваться несколько переменных и функций с помощью `export` (без `default`):

```

export var pi = 3.14
export class RandomNumberGenerator {}
export function....

```


Указанные сущности импортируются так:

`import { pi, phi, absolute } from './file.js'`

можно изменить название

`import {pi as p} from './file.js'`

или всё сразу поместить в пространство имён

```

import * as math from './maths.js'

const positivePhi = math.absolute(math.phi)

```

# внутренние модули в тс

Типы в TS могут экспортироваться и импортироваться с помощью такого же синтаксиса, что и значения в JS:

`export type Cat = { breed: string, yearOfBirth: number }`

`import { Cat } from './animal.js'`

# Синтаксис Common-JS, AMD - внешние модули

`CommonJS` — шаблон для определения и использования модулей. Он встроен в Node.js. По умолчанию каждый JS файл — это CJS.

`AMD (Асинхронное определение модуля)` -  из самых старых модульных систем, изначально реализована библиотекой require.js

Оба имеют концепт одного объекта, содержащего всего "экспорты" модуля.

TypeScript поддерживает `export =` синтаксис, характерный для воркфлоу CommonJS и AMD для экспорта одного объекта. 

   

```
let numberRegexp = /^[0-9]+$/;

class ZipCodeValidator {
	isAcceptable(s: string) {
	return s.length === 5 && numberRegexp.test(s);
  }
}
 export = ZipCodeValidator; 
```

Импорт 
`import moduleName = require("./ZipCodeValidator")`

Команда, используемая для компиляции главного TypeScript файла для систем AMD или CommonJS, следующая:

`tsc --module amd file.ts`
`tsc --module commonjs file.ts`
