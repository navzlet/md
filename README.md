Модуль - файл, содержащий `import` или `export` верхнего уровня.
Файл, не содержащий указанных ключевых слов, является глобальным скриптом.

У модулей есть своя область видимости.

Все модули имеют определенный формат и относятся к определенном типу.
Популярные - 
- AMD
- CommonJS
- ES
 
#  Cинтаксис: 

## Es syntax

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

# Модули в TS

Типы в TS могут экспортироваться и импортироваться с помощью такого же синтаксиса, что и значения в JS:

`export type Cat = { breed: string, yearOfBirth: number }`

`import { Cat } from './animal.js'`

TypeScript имеет ключевые слова `module` и `namespace`. Они называются внутренними модулями:

```
    module Counter {
        let count = 0
        export const increase = () => ++count
        export const reset = () => {
            count = 0
            console.log('Счетчик сброшен.')
        }
    }

    namespace Counter {
        let count = 0
        export const increase = () => ++count
        export const reset = () => {
            count = 0
            console.log('Счетчик сброшен.')
        }
    }
```


# Синтаксис Common-JS, AMD.

## CommonJs

`CommonJS` — шаблон для определения и использования модулей. Он встроен в Node.js. По умолчанию каждый JS файл — это CJS.

Переменные `module` и `exports` обеспечивают экспорт модуля (файла). Функция `require` обеспечивает загрузку и использование модуля. Следующий код демонстрирует определение модуля счетчика на синтаксисе CommonJS:  
  

```
    // определяем CommonJS модуль: commonJSCounterModule.js
    const dependencyModule1 = require('./dependencyModule1')
    const dependencyModule2 = require('./dependencyModule2')

    let count = 0
    const increase = () => ++count
    const reset = () => {
        count = 0
        console.log('Счетчик сброшен.')
    }

    module.exports = {
        increase,
        reset
    }
```

  
Вот как этот модуль используется:  
  

```
    // используем CommonJS модуль
    const {
        increase,
        reset
    } = require('./commonJSCounterModule')
    
    const commonJSCounterModule = require('./commonJSCounterModule')
    commonJSCounterModule.increase()
    commonJSCounterModule.reset()
```

## AMD

`AMD (Асинхронное определение модуля)` -  из самых старых модульных систем, изначально реализована библиотекой require.js


```
    // определяем AMD модуль
    define('amdCounterModule', ['dependencyModule1', 'dependencyModule2'], (dependencyModule1, dependencyModule2) => {
        let count = 0
        const increase = () => ++count
        const reset = () => {
            count = 0
            console.log('Счетчик сброшен.')
        }

        return {
            increase,
            reset
        }
    })
```

  
Он также содержит функцию `require` для использования модуля:  
  

```
    // используем AMD модуль
    require(['amdCounterModule'], amdCounterModule => {
        amdCounterModule.increase()
        amdCounterModule.reset()
    })

```


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
