Мост — это структурный паттерн проектирования, который разделяет один или несколько классов
на две отдельные иерархии — абстракцию и реализацию,
позволяя изменять их независимо друг от друга.

Абстракция — это образный слой управления чем-либо. Он не делает работу самостоятельно, а делегирует её слою реализации.

Простой пример для понимания:
У вас есть класс геометрических Фигур, который имеет подклассы Круг и Квадрат.
Вы хотите расширить иерархию фигур по цвету, то есть иметь Красные и Синие фигуры.
Но чтобы всё это объединить, вам придётся создать 4 комбинации подклассов, вроде СиниеКруги и КрасныеКвадраты.

![[изображение_2022-11-27_180717507.png]]

Решение:
Паттерн Мост предлагает заменить наследование агрегацией или композицией. Для этого нужно выделить одну из таких «плоскостей»
в отдельную иерархию и ссылаться на объект этой иерархии, вместо хранения его состояния и поведения внутри одного класса.

![[изображение_2022-11-27_181123415.png]]

==============

![[изображение_2022-11-27_204541686.png]]

Пример псевдокода

// Класс пультов имеет ссылку на устройство, которым управляет.
// Методы этого класса делегируют работу методам связанного
// устройства.
class Remote is
protected field device: Device
constructor Remote(device: Device) is
this.device = device
method togglePower() is
if (device.isEnabled()) then
device.disable()
else
device.enable()
method volumeDown() is
device.setVolume(device.getVolume() - 10)
method volumeUp() is
device.setVolume(device.getVolume() + 10)
method channelDown() is
device.setChannel(device.getChannel() - 1)
method channelUp() is
device.setChannel(device.getChannel() + 1)

// Вы можете расширять класс пультов, не трогая код устройств.
class AdvancedRemote extends Remote is
method mute() is
device.setVolume(0)

// Все устройства имеют общий интерфейс. Поэтому с ними может
// работать любой пульт.
interface Device is
method isEnabled()
method enable()
method disable()
method getVolume()
method setVolume(percent)
method getChannel()
method setChannel(channel)

// Но каждое устройство имеет особую реализацию.
class Tv implements Device is
// ...

class Radio implements Device is
// ...

// Где-то в клиентском коде.
tv = new Tv()
remote = new Remote(tv)
remote.togglePower()

radio = new Radio()
remote = new AdvancedRemote(radio)

============

пример кода:

/\*\*

- Абстракция устанавливает интерфейс для «управляющей» части двух иерархий
- классов. Она содержит ссылку на объект из иерархии Реализации и делегирует
- ему всю настоящую работу.
  \*/
  class Abstraction {
  protected implementation: Implementation;

      constructor(implementation: Implementation) {
          this.implementation = implementation;
      }

      public operation(): string {
          const result = this.implementation.operationImplementation();
          return `Abstraction: Base operation with:\n${result}`;
      }

  }

/\*\*

- Можно расширить Абстракцию без изменения классов Реализации.
  \*/
  class ExtendedAbstraction extends Abstraction {
  public operation(): string {
  const result = this.implementation.operationImplementation();
  return `ExtendedAbstraction: Extended operation with:\n${result}`;
  }
  }

/\*\*

- Реализация устанавливает интерфейс для всех классов реализации. Он не должен
- соответствовать интерфейсу Абстракции. На практике оба интерфейса могут быть
- совершенно разными. Как правило, интерфейс Реализации предоставляет только
- примитивные операции, в то время как Абстракция определяет операции более
- высокого уровня, основанные на этих примитивах.
  \*/
  interface Implementation {
  operationImplementation(): string;
  }

/\*\*

- Каждая Конкретная Реализация соответствует определённой платформе и реализует
- интерфейс Реализации с использованием API этой платформы.
  \*/
  class ConcreteImplementationA implements Implementation {
  public operationImplementation(): string {
  return 'ConcreteImplementationA: Here\'s the result on the platform A.';
  }
  }

class ConcreteImplementationB implements Implementation {
public operationImplementation(): string {
return 'ConcreteImplementationB: Here\'s the result on the platform B.';
}
}

/\*\*

- За исключением этапа инициализации, когда объект Абстракции связывается с
- определённым объектом Реализации, клиентский код должен зависеть только от
- класса Абстракции. Таким образом, клиентский код может поддерживать любую
- комбинацию абстракции и реализации.
  \*/
  function clientCode(abstraction: Abstraction) {
  // ..

      console.log(abstraction.operation());

      // ..

  }

/\*\*

- Клиентский код должен работать с любой предварительно сконфигурированной
- комбинацией абстракции и реализации.
  \*/
  let implementation = new ConcreteImplementationA();
  let abstraction = new Abstraction(implementation);
  clientCode(abstraction);

console.log('');

implementation = new ConcreteImplementationB();
abstraction = new ExtendedAbstraction(implementation);
clientCode(abstraction);

Выполнение программы:
Abstraction: Base operation with:
ConcreteImplementationA: Here's the result on the platform A.

ExtendedAbstraction: Extended operation with:
ConcreteImplementationB: Here's the result on the platform B.

Данный структурный паттерн полезен когда нужно расширять класс в независимых плоскостях. Он особенно полезен когда нужно делать кросс-платформенные приложения,
поддерживать несколько типов бд.