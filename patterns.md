1. **Синглтон**

Одиночка (Singleton, Синглтон) - порождающий паттерн, который гарантирует, что для определенного класса будет создан только один объект, а также предоставит к этому объекту точку доступа.

Когда надо использовать Синглтон? Когда необходимо, чтобы для класса существовал только один экземпляр

Синглтон позволяет создать объект только при его необходимости. Если объект не нужен, то он не будет создан. В этом отличие синглтона от глобальных переменных.

class Singleton

{

&nbsp;   private static Singleton instance;

&nbsp;

&nbsp;   private Singleton()

&nbsp;   {}

&nbsp;

&nbsp;   public static Singleton getInstance()

&nbsp;   {

&nbsp;       if (instance == null)

&nbsp;           instance = new Singleton();

&nbsp;       return instance;

&nbsp;   }

}

В классе определяется статическая переменная - ссылка на конкретный экземпляр данного объекта и приватный конструктор. В статическом методе getInstance() этот конструктор вызывается для создания объекта, если, конечно, объект отсутствует и равен null.

1. **Builder**

Строитель (Builder) - шаблон проектирования, который инкапсулирует создание объекта и позволяет разделить его на различные этапы.

**Когда использовать паттерн Строитель?**

- Когда процесс создания нового объекта не должен зависеть от того, из каких частей этот объект состоит и как эти части связаны между собой
- Когда необходимо обеспечить получение различных вариаций объекта в процессе его создания

class Client

{

&nbsp;   void Main()

&nbsp;   {

&nbsp;       Builder builder = new ConcreteBuilder();

&nbsp;       Director director = new Director(builder);

&nbsp;       director.Construct();

&nbsp;       Product product = builder.GetResult();

&nbsp;   }

}

class Director

{

&nbsp;   Builder builder;

&nbsp;   public Director(Builder builder)

&nbsp;   {

&nbsp;       this.builder = builder;

&nbsp;   }

&nbsp;   public void Construct()

&nbsp;   {

&nbsp;       builder.BuildPartA();

&nbsp;       builder.BuildPartB();

&nbsp;       builder.BuildPartC();

&nbsp;   }

}

&nbsp;

abstract class Builder

{

&nbsp;   public abstract void BuildPartA();

&nbsp;   public abstract void BuildPartB();

&nbsp;   public abstract void BuildPartC();

&nbsp;   public abstract Product GetResult();

}

&nbsp;

class Product

{

&nbsp;   List&lt;object&gt; parts = new List&lt;object&gt;();

&nbsp;   public void Add(string part)

&nbsp;   {

&nbsp;       parts.Add(part);

&nbsp;   }

}

&nbsp;

class ConcreteBuilder : Builder

{

&nbsp;   Product product = new Product();

&nbsp;   public override void BuildPartA()

&nbsp;   {

&nbsp;       product.Add("Part A");

&nbsp;   }

&nbsp;   public override void BuildPartB()

&nbsp;   {

&nbsp;       product.Add("Part B");

&nbsp;   }

&nbsp;   public override void BuildPartC()

&nbsp;   {

&nbsp;       product.Add("Part C");

&nbsp;   }

&nbsp;   public override Product GetResult()

&nbsp;   {

&nbsp;       return product;

&nbsp;   }

}

- **Product**: представляет объект, который должен быть создан. В данном случае все части объекта заключены в списке parts.
- **Builder**: определяет интерфейс для создания различных частей объекта Product
- **ConcreteBuilder**: конкретная реализация Buildera. Создает объект Product и определяет интерфейс для доступа к нему
- **Director**: распорядитель - создает объект, используя объекты Builder

1. **Фабрика**

Фабричный метод (Factory Method) - это паттерн, который определяет интерфейс для создания объектов некоторого класса, но непосредственное решение о том, объект какого класса создавать происходит в подклассах. То есть паттерн предполагает, что базовый класс делегирует создание объектов классам-наследникам.

### Когда надо применять паттерн

- Когда заранее неизвестно, объекты каких типов необходимо создавать
- Когда система должна быть независимой от процесса создания новых объектов и расширяемой: в нее можно легко вводить новые классы, объекты которых система должна создавать.
- Когда создание новых объектов необходимо делегировать из базового класса классам наследникам

abstract class Product

{}

&nbsp;

class ConcreteProductA : Product

{}

&nbsp;

class ConcreteProductB : Product

{}

&nbsp;

abstract class Creator

{

&nbsp;   public abstract Product FactoryMethod();

}

&nbsp;

class ConcreteCreatorA : Creator

{

&nbsp;   public override Product FactoryMethod() { return new ConcreteProductA(); }

}

&nbsp;

class ConcreteCreatorB : Creator

{

&nbsp;   public override Product FactoryMethod() { return new ConcreteProductB(); }

}

- Абстрактный класс **Product** определяет интерфейс класса, объекты которого надо создавать.
- Конкретные классы **ConcreteProductA** и **ConcreteProductB** представляют реализацию класса Product. Таких классов может быть множество
- Абстрактный класс **Creator** определяет абстрактный фабричный метод FactoryMethod(), который возвращает объект Product.
- Конкретные классы **ConcreteCreatorA** и **ConcreteCreatorB** - наследники класса Creator, определяющие свою реализацию метода FactoryMethod(). Причем метод FactoryMethod() каждого отдельного класса-создателя возвращает определенный конкретный тип продукта. Для каждого конкретного класса продукта определяется свой конкретный класс создателя.

Таким образом, класс Creator делегирует создание объекта Product своим наследникам. А классы ConcreteCreatorA и ConcreteCreatorB могут самостоятельно выбирать какой конкретный тип продукта им создавать.

1. Абстрактна фабрика

Паттерн "Абстрактная фабрика" (Abstract Factory) предоставляет интерфейс для создания семейств взаимосвязанных объектов с определенными интерфейсами без указания конкретных типов данных объектов.

### Когда использовать абстрактную фабрику

- Когда система не должна зависеть от способа создания и компоновки новых объектов
- Когда создаваемые объекты должны использоваться вместе и являются взаимосвязанными

abstract class AbstractFactory

{

&nbsp;   public abstract AbstractProductA CreateProductA();

&nbsp;   public abstract AbstractProductB CreateProductB();

}

class ConcreteFactory1: AbstractFactory

{

&nbsp;   public override AbstractProductA CreateProductA()

&nbsp;   {

&nbsp;       return new ProductA1();

&nbsp;   }

&nbsp;

&nbsp;   public override AbstractProductB CreateProductB()  

&nbsp;   {

&nbsp;       return new ProductB1();

&nbsp;   }

}

class ConcreteFactory2: AbstractFactory

{

&nbsp;   public override AbstractProductA CreateProductA()

&nbsp;   {

&nbsp;       return new ProductA2();

&nbsp;   }

&nbsp;

&nbsp;   public override AbstractProductB CreateProductB()

&nbsp;   {

&nbsp;       return new ProductB2();

&nbsp;   }

}

&nbsp;

abstract class AbstractProductA

{}

&nbsp;

abstract class AbstractProductB

{}

&nbsp;

class ProductA1: AbstractProductA  

{}

&nbsp;

class ProductB1: AbstractProductB  

{}

&nbsp;

class ProductA2: AbstractProductA  

{}

&nbsp;

class ProductB2: AbstractProductB

{}

&nbsp;

class Client

{

&nbsp;   private AbstractProductA abstractProductA;

&nbsp;   private AbstractProductB abstractProductB;

&nbsp;

&nbsp;   public Client(AbstractFactory factory)

&nbsp;   {

&nbsp;       abstractProductB = factory.CreateProductB();

&nbsp;       abstractProductA = factory.CreateProductA();

&nbsp;   }

&nbsp;   public void Run()

&nbsp;   { }

}

- Абстрактные классы **AbstractProductA** и **AbstractProductB** определяют интерфейс для классов, объекты которых будут создаваться в программе.
- Конкретные классы **ProductA1 / ProductA2** и **ProductB1 / ProductB2** представляют конкретную реализацию абстрактных классов
- Абстрактный класс фабрики **AbstractFactory** определяет методы для создания объектов. Причем методы возвращают абстрактные продукты, а не их конкретные реализации.
- Конкретные классы фабрик **ConcreteFactory1** и **ConcreteFactory2** реализуют абстрактные методы базового класса и непосредственно определяют какие конкретные продукты использовать
- Класс клиента **Client** использует класс фабрики для создания объектов. При этом он использует исключительно абстрактный класс фабрики AbstractFactory и абстрактные классы продуктов AbstractProductA и AbstractProductB и никак не зависит от их конкретных реализаций

1. **Прототип**

Паттерн Прототип (Prototype) позволяет создавать объекты на основе уже ранее созданных объектов-прототипов. То есть по сути данный паттерн предлагает технику клонирования объектов.

Когда использовать Прототип?

- Когда конкретный тип создаваемого объекта должен определяться динамически во время выполнения
- Когда нежелательно создание отдельной иерархии классов фабрик для создания объектов-продуктов из параллельной иерархии классов (как это делается, например, при использовании паттерна Абстрактная фабрика)
- Когда клонирование объекта является более предпочтительным вариантом нежели его создание и инициализация с помощью конструктора. Особенно когда известно, что объект может принимать небольшое ограниченное число возможных состояний.

class Client

{

&nbsp;   void Operation()

&nbsp;   {

&nbsp;       Prototype prototype = new ConcretePrototype1(1);

&nbsp;       Prototype clone = prototype.Clone();

&nbsp;       prototype = new ConcretePrototype2(2);

&nbsp;       clone = prototype.Clone();

&nbsp;   }

}

&nbsp;

abstract class Prototype

{

&nbsp;   public int Id { get; private set; }

&nbsp;   public Prototype(int id)

&nbsp;   {

&nbsp;       this.Id = id;

&nbsp;   }

&nbsp;   public abstract Prototype Clone();

}

&nbsp;

class ConcretePrototype1 : Prototype

{

&nbsp;   public ConcretePrototype1(int id)

&nbsp;       : base(id)

&nbsp;   { }

&nbsp;   public override Prototype Clone()

&nbsp;   {

&nbsp;       return new ConcretePrototype1(Id);

&nbsp;   }

}

&nbsp;

class ConcretePrototype2 : Prototype

{

&nbsp;   public ConcretePrototype2(int id)

&nbsp;       : base(id)

&nbsp;   { }

&nbsp;   public override Prototype Clone()

&nbsp;   {

&nbsp;       return new ConcretePrototype2(Id);

&nbsp;   }

}

- **Prototype**: определяет интерфейс для клонирования самого себя, который, как правило, представляет метод Clone()
- **ConcretePrototype1** и **ConcretePrototype2**: конкретные реализации прототипа. Реализуют метод Clone()
- **Client**: создает объекты прототипов с помощью метода Clone()

**ПАТТЕРНИ ПОВЕДІНКИ**

1. **Стратегія**

Паттерн Стратегия (Strategy) представляет шаблон проектирования, который определяет набор алгоритмов, инкапсулирует каждый из них и обеспечивает их взаимозаменяемость. В зависимости от ситуации мы можем легко заменить один используемый алгоритм другим. При этом замена алгоритма происходит независимо от объекта, который использует данный алгоритм.

### Когда использовать стратегию?

- Когда есть несколько родственных классов, которые отличаются поведением. Можно задать один основной класс, а разные варианты поведения вынести в отдельные классы и при необходимости их применять
- Когда необходимо обеспечить выбор из нескольких вариантов алгоритмов, которые можно легко менять в зависимости от условий
- Когда необходимо менять поведение объектов на стадии выполнения программы
- Когда класс, применяющий определенную функциональность, ничего не должен знать о ее реализации

public interface IStrategy

{

&nbsp;   void Algorithm();

}

&nbsp;

public class ConcreteStrategy1 : IStrategy

{

&nbsp;   public void Algorithm()

&nbsp;   {}

}

&nbsp;

public class ConcreteStrategy2 : IStrategy

{

&nbsp;   public void Algorithm()

&nbsp;   {}

}

&nbsp;

public class Context

{

&nbsp;   public IStrategy ContextStrategy { get; set; }

&nbsp;

&nbsp;   public Context(IStrategy \_strategy)

&nbsp;   {

&nbsp;       ContextStrategy = \_strategy;

&nbsp;   }

&nbsp;

&nbsp;   public void ExecuteAlgorithm()

&nbsp;   {

&nbsp;       ContextStrategy.Algorithm();

&nbsp;   }

}

- Интерфейс IStrategy, который определяет метод Algorithm(). Это общий интерфейс для всех реализующих его алгоритмов. Вместо интерфейса здесь также можно было бы использовать абстрактный класс.
- Классы ConcreteStrategy1 и ConcreteStrategy, которые реализуют интерфейс IStrategy, предоставляя свою версию метода Algorithm(). Подобных классов-реализаций может быть множество.
- Класс Context хранит ссылку на объект IStrategy и связан с интерфейсом IStrategy отношением агрегации.

В данном случае объект IStrategy заключена в свойстве ContextStrategy, хотя также для нее можно было бы определить приватную переменную, а для динамической установки использовать специальный метод.

1. **Спостерігач**

Паттерн "Наблюдатель" (Observer) представляет поведенческий шаблон проектирования, который использует отношение "один ко многим". В этом отношении есть один наблюдаемый объект и множество наблюдателей. И при изменении наблюдаемого объекта автоматически происходит оповещение всех наблюдателей.

Данный паттерн еще называют Publisher-Subscriber (издатель-подписчик), поскольку отношения издателя и подписчиков характеризуют действие данного паттерна: подписчики подписываются email-рассылку определенного сайта. Сайт-издатель с помощью email-рассылки уведомляет всех подписчиков о изменениях. А подписчики получают изменения и производят определенные действия: могут зайти на сайт, могут проигнорировать уведомления и т.д.

### Когда использовать паттерн Наблюдатель?

- Когда система состоит из множества классов, объекты которых должны находиться в согласованных состояниях
- Когда общая схема взаимодействия объектов предполагает две стороны: одна рассылает сообщения и является главным, другая получает сообщения и реагирует на них. Отделение логики обеих сторон позволяет их рассматривать независимо и использовать отдельно друга от друга.
- Когда существует один объект, рассылающий сообщения, и множество подписчиков, которые получают сообщения. При этом точное число подписчиков заранее неизвестно и процессе работы программы может изменяться.

interface IObservable

{

&nbsp;   void AddObserver(IObserver o);

&nbsp;   void RemoveObserver(IObserver o);

&nbsp;   void NotifyObservers();

}

class ConcreteObservable : IObservable

{

&nbsp;   private List&lt;IObserver&gt; observers;

&nbsp;   public ConcreteObservable()

&nbsp;   {

&nbsp;       observers = new List&lt;IObserver&gt;();

&nbsp;   }

&nbsp;   public void AddObserver(IObserver o)

&nbsp;   {

&nbsp;       observers.Add(o);

&nbsp;   }

&nbsp;

&nbsp;   public void RemoveObserver(IObserver o)

&nbsp;   {

&nbsp;       observers.Remove(o);

&nbsp;   }

&nbsp;

&nbsp;   public void NotifyObservers()

&nbsp;   {

&nbsp;       foreach (IObserver observer in observers)

&nbsp;           observer.Update();

&nbsp;   }

}

&nbsp;

interface IObserver

{

&nbsp;   void Update();

}

class ConcreteObserver :IObserver

{

&nbsp;   public void Update()

&nbsp;   {

&nbsp;   }

}

- **IObservable**: представляет наблюдаемый объект. Определяет три метода: AddObserver() (для добавления наблюдателя), RemoveObserver() (удаление набюдателя) и NotifyObservers() (уведомление наблюдателей)
- **ConcreteObservable**: конкретная реализация интерфейса IObservable. Определяет коллекцию объектов наблюдателей.
- **IObserver**: представляет наблюдателя, который подписывается на все уведомления наблюдаемого объекта. Определяет метод Update(), который вызывается наблюдаемым объектом для уведомления наблюдателя.
- **ConcreteObserver**: конкретная реализация интерфейса IObserver.

При этом наблюдаемому объекту не надо ничего знать о наблюдателе кроме того, что тот реализует метод Update(). С помощью отношения агрегации реализуется слабосвязанность обоих компонентов. Изменения в наблюдаемом объекте не виляют на наблюдателя и наоборот.

В определенный момент наблюдатель может прекратить наблюдение. И после этого оба объекта - наблюдатель и наблюдаемый могут продолжать существовать в системе независимо друг от друга.

1. **Команда**

Паттерн "Команда" (Command) позволяет инкапсулировать запрос на выполнение определенного действия в виде отдельного объекта. Этот объект запроса на действие и называется командой. При этом объекты, инициирующие запросы на выполнение действия, отделяются от объектов, которые выполняют это действие.

Команды могут использовать параметры, которые передают ассоциированную с командой информацию. Кроме того, команды могут ставиться в очередь и также могут быть отменены.

### Когда использовать команды?

Когда надо передавать в качестве параметров определенные действия, вызываемые в ответ на другие действия. То есть когда необходимы функции обратного действия в ответ на определенные действия.

Когда необходимо обеспечить выполнение очереди запросов, а также их возможную отмену.

Когда надо поддерживать логгирование изменений в результате запросов. Использование логов может помочь восстановить состояние системы - для этого необходимо будет использовать последовательность запротоколированных команд.

abstract class Command

{

&nbsp;   public abstract void Execute();

&nbsp;   public abstract void Undo();

}

// конкретная команда

class ConcreteCommand : Command

{

&nbsp;   Receiver receiver;

&nbsp;   public ConcreteCommand(Receiver r)

&nbsp;   {

&nbsp;       receiver = r;

&nbsp;   }

&nbsp;   public override void Execute()

&nbsp;   {

&nbsp;       receiver.Operation();

&nbsp;   }

&nbsp;

&nbsp;   public override void Undo()

&nbsp;   {}

}

&nbsp;

// получатель команды

class Receiver

{

&nbsp;   public void Operation()

&nbsp;   { }

}

// инициатор команды

class Invoker

{

&nbsp;   Command command;

&nbsp;   public void SetCommand(Command c)

&nbsp;   {

&nbsp;       command = c;

&nbsp;   }

&nbsp;   public void Run()

&nbsp;   {

&nbsp;       command.Execute();

&nbsp;   }

&nbsp;   public void Cancel()

&nbsp;   {

&nbsp;       command.Undo();

&nbsp;   }

}

class Client

{

&nbsp;   void Main()

&nbsp;   {

&nbsp;       Invoker invoker = new Invoker();

&nbsp;       Receiver receiver = new Receiver();

&nbsp;       ConcreteCommand command=new ConcreteCommand(receiver);

&nbsp;       invoker.SetCommand(command);

&nbsp;       invoker.Run();

&nbsp;   }

}

- **Command**: интерфейс, представляющий команду. Обычно определяет метод Execute() для выполнения действия, а также нередко включает метод Undo(), реализация которого должна заключаться в отмене действия команды
- **ConcreteCommand**: конкретная реализация команды, реализует метод Execute(), в котором вызывается определенный метод, определенный в классе Receiver
- **Receiver**: получатель команды. Определяет действия, которые должны выполняться в результате запроса.
- **Invoker**: инициатор команды - вызывает команду для выполнения определенного запроса
- **Client**: клиент - создает команду и устанавливает ее получателя с помощью метода SetCommand()

Таким образом, инициатор, отправляющий запрос, ничего не знает о получателе, который и будет выполнять команду. Кроме того, если нам потребуется применить какие-то новые команды, мы можем просто унаследовать классы от абстрактного класса Command и реализовать его методы Execute и Undo.

В программах на C# команды находят довольно широкое применение. Так, в технологии WPF и других технологиях, которые используют XAML и подход MVVM, на командах во многом базируется взаимодействие с пользователем. В некоторых архитектурах, например, в архитектуре CQRS, команды являются одним из ключевых компонентов.

1. **Шаблонний метод**

Шаблонный метод (Template Method) определяет общий алгоритм поведения подклассов, позволяя им переопределить отдельные шаги этого алгоритма без изменения его структуры.

### Когда использовать шаблонный метод?

- Когда планируется, что в будущем подклассы должны будут переопределять различные этапы алгоритма без изменения его структуры
- Когда в классах, реализующим схожий алгоритм, происходит дублирование кода. Вынесение общего кода в шаблонный метод уменьшит его дублирование в подклассах.

abstract class AbstractClass

{

&nbsp;   public void TemplateMethod()

&nbsp;   {

&nbsp;       Operation1();

&nbsp;       Operation2();

&nbsp;   }

&nbsp;   public abstract void Operation1();

&nbsp;   public abstract void Operation2();

}

&nbsp;

class ConcreteClass : AbstractClass

{

&nbsp;   public override void Operation1()

&nbsp;   {

&nbsp;   }

&nbsp;

&nbsp;   public override void Operation2()

&nbsp;   {

&nbsp;   }

}

- **AbstractClass**: определяет шаблонный метод TemplateMethod(), который реализует алгоритм. Алгоритм может состоять из последовательности вызовов других методов, часть из которых может быть абстрактными и должны быть переопределены в классах-наследниках. При этом сам метод TemplateMethod(), представляющий структуру алгоритма, переопределяться не должен.
- **ConcreteClass**: подкласс, который может переопределять различные методы родительского класса.

1. **Ітератор**

Паттерн Итератор (Iterator) предоставляет абстрактный интерфейс для последовательного доступа ко всем элементам составного объекта без раскрытия его внутренней структуры.

Наверное, всем программистам, работающим с языком C#, приходилось иметь дело с циклом foreach, который перебирает объекты в массиве или коллекции. При этом встроенных классов коллекций существует множество, и каждая из них отличается по своей структуре и поведению.

Ключевым моментом, который позволяет осуществить перебор коллекций с помощью foreach, является применения этими классами коллекций паттерна итератор, или проще говоря пары интерфейсов IEnumerable / IEnumerator. Интерфейс IEnumerator определяет функционал для перебора внутренних объектов в контейнере

Используя данные интерфейсы, мы можем свести к одному шаблону - с помощью цикла foreach - любые составные объекты.

### Когда использовать итераторы?

- Когда необходимо осуществить обход объекта без раскрытия его внутренней структуры
- Когда имеется набор составных объектов, и надо обеспечить единый интерфейс для их перебора
- Когда необходимо предоставить несколько альтернативных вариантов перебора одного и того же объекта

class Client

{

&nbsp;   public void Main()

&nbsp;   {

&nbsp;       Aggregate a = new ConcreteAggregate();

&nbsp;

&nbsp;       Iterator i = a.CreateIterator();

&nbsp;

&nbsp;       object item = i.First();

&nbsp;       while (!i.IsDone())

&nbsp;       {

&nbsp;           item = i.Next();

&nbsp;       }

&nbsp;   }

}

&nbsp;

abstract class Aggregate

{

&nbsp;   public abstract Iterator CreateIterator();

&nbsp;   public abstract int Count { get; protected set; }

&nbsp;   public abstract object this\[int index\] { get; set; }

}

&nbsp;

class ConcreteAggregate : Aggregate

{

&nbsp;   private readonly ArrayList \_items = new ArrayList();

&nbsp;

&nbsp;   public override Iterator CreateIterator()

&nbsp;   {

&nbsp;       return new ConcreteIterator(this);

&nbsp;   }

&nbsp;

&nbsp;   public override int Count

&nbsp;   {

&nbsp;       get { return \_items.Count; }

&nbsp;       protected set { }

&nbsp;   }

&nbsp;

&nbsp;   public override object this\[int index\]

&nbsp;   {

&nbsp;       get { return \_items\[index\]; }

&nbsp;       set { \_items.Insert(index, value); }

&nbsp;   }

}

abstract class Iterator

{

&nbsp;   public abstract object First();

&nbsp;   public abstract object Next();

&nbsp;   public abstract bool IsDone();

&nbsp;   public abstract object CurrentItem();

}

&nbsp;

class ConcreteIterator : Iterator

{

&nbsp;   private readonly Aggregate \_aggregate;

&nbsp;   private int \_current;

&nbsp;

&nbsp;   public ConcreteIterator(Aggregate aggregate)

&nbsp;   {

&nbsp;       this.\_aggregate = aggregate;

&nbsp;   }

&nbsp;

&nbsp;   public override object First()

&nbsp;   {

&nbsp;       return \_aggregate\[0\];

&nbsp;   }

&nbsp;

&nbsp;   public override object Next()

&nbsp;   {

&nbsp;       object ret = null;

&nbsp;

&nbsp;       \_current++;

&nbsp;

&nbsp;       if (\_current < \_aggregate.Count)

&nbsp;       {

&nbsp;           ret = \_aggregate\[\_current\];

&nbsp;       }

&nbsp;

&nbsp;       return ret;

&nbsp;   }

&nbsp;

&nbsp;   public override object CurrentItem()

&nbsp;   {

&nbsp;       return \_aggregate\[\_current\];

&nbsp;   }

&nbsp;

&nbsp;   public override bool IsDone()

&nbsp;   {

&nbsp;       return \_current >= \_aggregate.Count;

&nbsp;   }

}

- **Iterator**: определяет интерфейс для обхода составных объектов
- **Aggregate**: определяет интерфейс для создания объекта-итератора
- **ConcreteIterator**: конкретная реализация итератора для обхода объекта Aggregate. Для фиксации индекса текущего перебираемого элемента использует целочисленную переменную \_current
- **ConcreteAggregate**: конкретная реализация Aggregate. Хранит элементы, которые надо будет перебирать
- **Client**: использует объект Aggregate и итератор для его обхода

1. **Стан (State)**

Состояние (State) - шаблон проектирования, который позволяет объекту изменять свое поведение в зависимости от внутреннего состояния.

### Когда применяется данный паттерн?

- Когда поведение объекта должно зависеть от его состояния и может изменяться динамически во время выполнения
- Когда в коде методов объекта используются многочисленные условные конструкции, выбор которых зависит от текущего состояния объекта

class Program

{

&nbsp;   static void Main()

&nbsp;   {

&nbsp;       Context context = new Context(new StateA());

&nbsp;       context.Request(); // Переход в состояние StateB

&nbsp;       context.Request();  // Переход в состояние StateA

&nbsp;   }

}

abstract class State

{

&nbsp;   public abstract void Handle(Context context);

}

class StateA : State

{

&nbsp;   public override void Handle(Context context)

&nbsp;   {

&nbsp;       context.State = new StateB();

&nbsp;   }

}

class StateB : State

{

&nbsp;   public override void Handle(Context context)

&nbsp;   {

&nbsp;       context.State = new StateA();

&nbsp;   }

}

&nbsp;

class Context

{

&nbsp;   public State State { get; set; }

&nbsp;   public Context(State state)

&nbsp;   {

&nbsp;       this.State = state;

&nbsp;   }

&nbsp;   public void Request()

&nbsp;   {

&nbsp;       this.State.Handle(this);

&nbsp;   }

}

- **State**: определяет интерфейс состояния
- Классы **StateA** и **StateB** - конкретные реализации состояний
- **Context**: представляет объект, поведение которого должно динамически изменяться в соответствии с состоянием. Выполнение же конкретных действий делегируется объекту состояния

1. **Ланцюг обовязків (Chain of responsibility)**

Цепочка Обязанностей (Chain of responsibility) - поведенческий шаблон проектирования, который позволяет избежать жесткой привязки отправителя запроса к получателю. Все возможные обработчики запроса образуют цепочку, а сам запрос перемещается по этой цепочке. Каждый объект в этой цепочке при получении запроса выбирает, либо закончить обработку запроса, либо передать запрос на обработку следующему по цепочке объекту.

### Когда применяется цепочка обязанностей?

- Когда имеется более одного объекта, который может обработать определенный запрос
- Когда надо передать запрос на выполнение одному из нескольких объект, точно не определяя, какому именно объекту
- Когда набор объектов задается динамически

class Client

{

&nbsp;   void Main()

&nbsp;   {

&nbsp;       Handler h1 = new ConcreteHandler1();

&nbsp;       Handler h2 = new ConcreteHandler2();

&nbsp;       h1.Successor = h2;

&nbsp;       h1.HandleRequest(2);

&nbsp;   }

}

abstract class Handler

{

&nbsp;   public Handler Successor { get; set; }

&nbsp;   public abstract void HandleRequest(int condition);

}

&nbsp;

class ConcreteHandler1 : Handler

{

&nbsp;   public override void HandleRequest(int condition)

&nbsp;   {

&nbsp;       // некоторая обработка запроса

&nbsp;

&nbsp;       if (condition==1)

&nbsp;       {

&nbsp;           // завершение выполнения запроса;

&nbsp;       }

&nbsp;       // передача запроса дальше по цепи при наличии в ней обработчиков

&nbsp;       else if (Successor != null)

&nbsp;       {

&nbsp;           Successor.HandleRequest(condition);

&nbsp;       }

&nbsp;   }

}

&nbsp;

class ConcreteHandler2 : Handler

{

&nbsp;   public override void HandleRequest(int condition)

&nbsp;   {

&nbsp;       // некоторая обработка запроса

&nbsp;

&nbsp;       if (condition==2)

&nbsp;       {

&nbsp;           // завершение выполнения запроса;

&nbsp;       }

&nbsp;       // передача запроса дальше по цепи при наличии в ней обработчиков

&nbsp;       else if (Successor != null)

&nbsp;       {

&nbsp;           Successor.HandleRequest(condition);

&nbsp;       }

&nbsp;   }

}

- **Handler**: определяет интерфейс для обработки запроса. Также может определять ссылку на следующий обработчик запроса
- **ConcreteHandler1** и **ConcreteHandler2**: конкретные обработчики, которые реализуют функционал для обработки запроса. При невозможности обработки и наличия ссылки на следующий обработчик, передают запрос этому обработчику

В данном случае для простоты примера в качестве параметра передается некоторое число, сначала обработчик обрабатывает запрос и, если передано соответствующее число, завершает его обработку. Иначе передает запрос на обработку следующем в цепи обработчику при его наличии.

- **Client**: отправляет запрос объекту Handler

Использование цепочки обязанностей дает нам следующие преимущества:

- Ослабление связанности между объектами. Отправителю и получателю запроса ничего не известно друг о друге. Клиенту неизветна цепочка объектов, какие именно объекты составляют ее, как запрос в ней передается.
- В цепочку с легкостью можно добавлять новые типы объектов, которые реализуют общий интерфейс.

В то же время у паттерна есть недостаток: никто не гарантирует, что запрос в конечном счете будет обработан. Если необходимого обработчика в цепочки не оказалось, то запрос просто выходит из цепочки и остается необработанным.

Использование паттерна довольно часто встречается в нашей жизни. Достаточно распространена ситуация, когда чиновники перекладывают друг на друга по цепочке выполнения какого-нибудь дела, и оно в конце концов оказывается не выполненным. Или когда мы идем в поликлинику, но при этом точно не знаем характер заболевания. В этом случае мы идем к терапевту, а он в зависимости от заболевания уже может либо сам лечить, либо отправить на лечение другим специализированным врачам.

1. **Інтерпретатор**

Паттерн Интерпретатор (Interpreter) определяет представление грамматики для заданного языка и интерпретатор предложений этого языка. Как правило, данный шаблон проектирования применяется для часто повторяющихся операций.

Хотя паттерн требует понимания теории формальных языков и грамматик, на самом деле он не так сложен в понимании.

class Client

{

&nbsp;   void Main()

&nbsp;   {

&nbsp;       Context context = new Context();

&nbsp;

&nbsp;       var expression = new NonterminalExpression();

&nbsp;       expression.Interpret(context);

&nbsp;

&nbsp;   }

}

&nbsp;

class Context

{

}

&nbsp;

abstract class AbstractExpression

{

&nbsp;   public abstract void Interpret(Context context);

}

&nbsp;

class TerminalExpression : AbstractExpression

{

&nbsp;   public override void Interpret(Context context)

&nbsp;   {

&nbsp;   }

}

&nbsp;

class NonterminalExpression : AbstractExpression

{

&nbsp;   AbstractExpression expression1;

&nbsp;   AbstractExpression expression2;

&nbsp;   public override void Interpret(Context context)

&nbsp;   {

&nbsp;

&nbsp;   }

}

- **AbstractExpression**: определяет интерфейс выражения, объявляет метод Interpret()
- **TerminalExpression**: терминальное выражение, реализует метод Interpret() для терминальных символов грамматики. Для каждого символа грамматики создается свой объект TerminalExpression
- **NonterminalExpression**: нетерминальное выражение, представляет правило грамматики. Для каждого отдельного правила грамматики создается свой объект NonterminalExpression.
- **Context**: содержит общую для интерпретатора информацию. Может использоваться объектами терминальных и нетерминальных выражений для сохранения состояния операций и последующего доступа к сохраненному состоянию
- **Client**: строит предложения языка с данной грамматикой в виде абстрактного синтаксического дерева, узлами которого являются объекты TerminalExpression и NonterminalExpression

Методы Interpret в нетерминальных выражениях позволяют реализовать правила грамматики. При этом мы легко может добавить новые правила грамматики, определив новые объекты NonterminalExpression со своей реализацией метода Interpret. Однако данный паттерн подходит только для тех случаев, когда правила грамматики относительно простые. В более сложных случаях следует выбирать другие способы проектирования приложения.

1. **Посередник (Mediator)**

Паттерн Посредник (Mediator) представляет такой шаблон проектирования, который обеспечивает взаимодействие множества объектов без необходимости ссылаться друг на друга. Тем самым достигается слабосвязанность взаимодействующих объектов.

Когда используется паттерн Посредник?

- Когда имеется множество взаимосвязаных объектов, связи между которыми сложны и запутаны.
- Когда необходимо повторно использовать объект, однако повторное использование затруднено в силу сильных связей с другими объектами.

abstract class Mediator

{

&nbsp;   public abstract void Send(string msg, Colleague colleague);

}

&nbsp;

abstract class Colleague

{

&nbsp;   protected Mediator mediator;

&nbsp;

&nbsp;   public Colleague(Mediator mediator)

&nbsp;   {

&nbsp;       this.mediator = mediator;

&nbsp;   }

}

&nbsp;

class ConcreteColleague1 : Colleague

{

&nbsp;   public ConcreteColleague1(Mediator mediator)

&nbsp;       : base(mediator)

&nbsp;   { }

&nbsp;

&nbsp;   public void Send(string message)

&nbsp;   {

&nbsp;       mediator.Send(message, this);

&nbsp;   }

&nbsp;

&nbsp;   public void Notify(string message)

&nbsp;   { }

}

&nbsp;

class ConcreteColleague2 : Colleague

{

&nbsp;   public ConcreteColleague2(Mediator mediator)

&nbsp;       : base(mediator)

&nbsp;   { }

&nbsp;

&nbsp;   public void Send(string message)

&nbsp;   {

&nbsp;       mediator.Send(message, this);

&nbsp;   }

&nbsp;

&nbsp;   public void Notify(string message)

&nbsp;   { }

}

&nbsp;

class ConcreteMediator : Mediator

{

&nbsp;   public ConcreteColleague1 Colleague1 { get; set; }

&nbsp;   public ConcreteColleague2 Colleague2 { get; set; }

&nbsp;   public override void Send(string msg, Colleague colleague)

&nbsp;   {

&nbsp;       if (Colleague1 == colleague)

&nbsp;           Colleague2.Notify(msg);

&nbsp;       else

&nbsp;           Colleague1.Notify(msg);

&nbsp;   }

}

- **Mediator**: представляет интерфейс для взаимодействия с объектами Colleague
- **Colleague**: представляет интерфейс для взаимодействия с объектом Mediator
- **ConcreteColleague1** и **ConcreteColleague2**: конкретные классы коллег, которые обмениваются друг с другом через объект Mediator
- **ConcreteMediator**: конкретный посредник, реализующий интерфейс типа Mediator

1. **Зберігач (Memento)**

Паттерн Хранитель (Memento) позволяет выносить внутреннее состояние объекта за его пределы для последующего возможного восстановления объекта без нарушения принципа инкапсуляции.

Когда использовать Memento?

- Когда нужно сохранить состояние объекта для возможного последующего восстановления
- Когда сохранение состояния должно проходить без нарушения принципа инкапсуляции

То есть ключевыми понятиями для данного паттерна являются **сохранение внутреннего состояния** и **инкапсуляция**, и важно соблюсти баланс между ними. Ведь, как правило, если мы не нарушаем инкапсуляцию, то состояние объекта хранится в объекте в приватных переменных. И не всегда для доступа к этим переменным есть методы или свойства с сеттерами и геттерами. Например, в игре происходит управление героем, все состояние которого заключено в нем самом - оружие героя, показатель жизней, силы, какие-то другие показатели. И нередко может возникнуть ситуация, сохранить все эти показатели во вне, чтобы в будущем можно было откатиться к предыдущему уровню и начать игру заново. В этом случае как раз и может помочь паттерн Хранитель.

class Memento

{

&nbsp;   public string State { get; private set;}

&nbsp;   public Memento(string state)

&nbsp;   {

&nbsp;       this.State = state;

&nbsp;   }

}

&nbsp;

class Caretaker

{

&nbsp;   public Memento Memento { get; set; }

}

&nbsp;

class Originator

{

&nbsp;   public string State { get; set; }

&nbsp;   public void SetMemento(Memento memento)

&nbsp;   {

&nbsp;       State = memento.State;

&nbsp;   }

&nbsp;   public Memento CreateMemento()

&nbsp;   {

&nbsp;       return new Memento(State);

&nbsp;   }

}

- **Memento**: хранитель, который сохраняет состояние объекта Originator и предоставляет полный доступ только этому объекту Originator
- **Originator**: создает объект хранителя для сохранения своего состояния
- **Caretaker**: выполняет только функцию хранения объекта Memento, в то же время у него нет полного доступа к хранителю и никаких других операций над хранителем, кроме собственно сохранения, он не производит

1. **Візитор**

Паттерн Посетитель (Visitor) позволяет определить операцию для объектов других классов без изменения этих классов.

При использовании паттерна Посетитель определяются две иерархии классов: одна для элементов, для которых надо определить новую операцию, и вторая иерархия для посетителей, описывающих данную операцию.

Когда использовать данный паттерн?

- Когда имеется много объектов разнородных классов с разными интерфейсами, и требуется выполнить ряд операций над каждым из этих объектов
- Когда классам необходимо добавить одинаковый набор операций без изменения этих классов
- Когда часто добавляются новые операции к классам, при этом общая структура классов стабильна и практически не изменяется

class Client

{

&nbsp;   void Main()

&nbsp;   {

&nbsp;       var structure = new ObjectStructure();

&nbsp;       structure.Add(new ElementA());

&nbsp;       structure.Add(new ElementB());

&nbsp;       structure.Accept(new ConcreteVisitor1());

&nbsp;       structure.Accept(new ConcreteVisitor2());

&nbsp;   }

}

&nbsp;

abstract class Visitor

{

&nbsp;   public abstract void VisitElementA(ElementA elemA);

&nbsp;   public abstract void VisitElementB(ElementB elemB);

}

&nbsp;

class ConcreteVisitor1 : Visitor

{

&nbsp;   public override void VisitElementA(ElementA elementA)

&nbsp;   {

&nbsp;       elementA.OperationA();

&nbsp;   }

&nbsp;   public override void VisitElementB(ElementB elementB)

&nbsp;   {

&nbsp;           elementB.OperationB();

&nbsp;   }

}

class ConcreteVisitor2 : Visitor

{

&nbsp;   public override void VisitElementA(ElementA elementA)

&nbsp;   {

&nbsp;       elementA.OperationA();

&nbsp;   }

&nbsp;   public override void VisitElementB(ElementB elementB)

&nbsp;   {

&nbsp;       elementB.OperationB();

&nbsp;   }

}

&nbsp;

class ObjectStructure

{

&nbsp;   List&lt;Element&gt; elements = new List&lt;Element&gt;();

&nbsp;   public void Add(Element element)

&nbsp;   {

&nbsp;       elements.Add(element);

&nbsp;   }

&nbsp;   public void Remove(Element element)

&nbsp;   {

&nbsp;       elements.Remove(element);

&nbsp;   }

&nbsp;   public void Accept(Visitor visitor)

&nbsp;   {

&nbsp;       foreach (Element element in elements)

&nbsp;           element.Accept(visitor);

&nbsp;   }

}

&nbsp;

abstract class Element

{

&nbsp;   public abstract void Accept(Visitor visitor);

&nbsp;   public string SomeState { get; set; }

}

&nbsp;

class ElementA : Element

{

&nbsp;   public override void Accept(Visitor visitor)

&nbsp;   {

&nbsp;       visitor.VisitElementA(this);

&nbsp;   }

&nbsp;   public void OperationA()

&nbsp;   { }

}

&nbsp;

class ElementB : Element

{

&nbsp;   public override void Accept(Visitor visitor)

&nbsp;   {

&nbsp;       visitor.VisitElementB(this);

&nbsp;   }

&nbsp;   public void OperationB()

&nbsp;   { }

}

- **Visitor**: интерфейс посетителя, который определяет метод Visit() для каждого объекта Element
- **ConcreteVisitor1 / ConcreteVisitor2**: конкретные классы посетителей, реализуют интерфейс, определенный в Visitor.
- **Element**: определяет метод Accept(), в котором в качестве параметра принимается объект Visitor
- **ElementA / ElementB**: конкретные элементы, которые реализуют метод Accept()
- **ObjectStructure**: некоторая структура, которая хранит объекты Element и предоставляет к ним доступ. Это могут быть и простые списки, и сложные составные структуры в виде деревьев

Сущность работы паттерна состоит в том, что вначале создает объект посетителя, который обходит или посещает все элементы в структуре ObjectStructure, у которой вызывается метод Accept():

Данная техника еще называется **двойной диспетчеризацией (double dispatch)**, когда выполнение операции зависит от имени запроса и двух типов получателей (объект Visitor и объект Element).

**СТРУКТУРНІ ПАТТЕРНИ**

1. **Декоратор**

Декоратор (Decorator) представляет структурный шаблон проектирования, который позволяет динамически подключать к объекту дополнительную функциональность.

Для определения нового функционала в классах нередко используется наследование. Декораторы же предоставляет наследованию более гибкую альтернативу, поскольку позволяют динамически в процессе выполнения определять новые возможности у объектов.

### Когда следует использовать декораторы?

Когда надо динамически добавлять к объекту новые функциональные возможности. При этом данные возможности могут быть сняты с объекта

Когда применение наследования неприемлемо. Например, если нам надо определить множество различных функциональностей и для каждой функциональности наследовать отдельный класс, то структура классов может очень сильно разрастись. Еще больше она может разрастись, если нам необходимо создать классы, реализующие все возможные сочетания добавляемых функциональностей.

abstract class Component

{

&nbsp;   public abstract void Operation();

}

class ConcreteComponent : Component

{

&nbsp;   public override void Operation()

&nbsp;   {}

}

abstract class Decorator : Component

{

&nbsp;   protected Component component;

&nbsp;

&nbsp;   public void SetComponent(Component component)

&nbsp;   {

&nbsp;       this.component = component;

&nbsp;   }

&nbsp;

&nbsp;   public override void Operation()

&nbsp;   {

&nbsp;       if (component != null)

&nbsp;           component.Operation();

&nbsp;   }

}

class ConcreteDecoratorA : Decorator

{

&nbsp;   public override void Operation()

&nbsp;   {

&nbsp;       base.Operation();

&nbsp;   }

}

class ConcreteDecoratorB : Decorator

{

&nbsp;   public override void Operation()

&nbsp;   {

&nbsp;       base.Operation();

&nbsp;   }

}

- **Component**: абстрактный класс, который определяет интерфейс для наследуемых объектов
- **ConcreteComponent**: конкретная реализация компонента, в которую с помощью декоратора добавляется новая функциональность
- **Decorator**: собственно декоратор, реализуется в виде абстрактного класса и имеет тот же базовый класс, что и декорируемые объекты. Поэтому базовый класс Component должен быть по возможности легким и определять только базовый интерфейс.

Класс декоратора также хранит ссылку на декорируемый объект в виде объекта базового класса Component и реализует связь с базовым классом как через наследование, так и через отношение агрегации.

- Классы **ConcreteDecoratorA** и **ConcreteDecoratorB** представляют дополнительные функциональности, которыми должен быть расширен объект ConcreteComponent.

1. **Адаптер**

Паттерн Адаптер (Adapter) предназначен для преобразования интерфейса одного класса в интерфейс другого. Благодаря реализации данного паттерна мы можем использовать вместе классы с несовместимыми интерфейсами.

### Когда надо использовать Адаптер?

- Когда необходимо использовать имеющийся класс, но его интерфейс не соответствует потребностям
- Когда надо использовать уже существующий класс совместно с другими классами, интерфейсы которых не совместимы

class Client

{

&nbsp;   public void Request(Target target)

&nbsp;   {

&nbsp;       target.Request();

&nbsp;   }

}

// класс, к которому надо адаптировать другой класс  

class Target

{

&nbsp;   public virtual void Request()

&nbsp;   {}

}

&nbsp;

// Адаптер

class Adapter : Target

{

&nbsp;   private Adaptee adaptee = new Adaptee();

&nbsp;

&nbsp;   public override void Request()

&nbsp;   {

&nbsp;       adaptee.SpecificRequest();

&nbsp;   }

}

&nbsp;

// Адаптируемый класс

class Adaptee

{

&nbsp;   public void SpecificRequest()

&nbsp;   {}

}

- **Target**: представляет объекты, которые используются клиентом
- **Client**: использует объекты Target для реализации своих задач
- **Adaptee**: представляет адаптируемый класс, который мы хотели бы использовать у клиента вместо объектов Target
- **Adapter**: собственно адаптер, который позволяет работать с объектами Adaptee как с объектами Target.

То есть клиент ничего не знает об Adaptee, он знает и использует только объекты Target. И благодаря адаптеру мы можем на клиенте использовать объекты Adaptee как Target

1. **Фасад**

Фасад (Facade) представляет шаблон проектирования, который позволяет скрыть сложность системы с помощью предоставления упрощенного интерфейса для взаимодействия с ней.

### Когда использовать фасад?

- Когда имеется сложная система, и необходимо упростить с ней работу. Фасад позволит определить одну точку взаимодействия между клиентом и системой.
- Когда надо уменьшить количество зависимостей между клиентом и сложной системой. Фасадные объекты позволяют отделить, изолировать компоненты системы от клиента и развивать и работать с ними независимо.
- Когда нужно определить подсистемы компонентов в сложной системе. Создание фасадов для компонентов каждой отдельной подсистемы позволит упростить взаимодействие между ними и повысить их независимость друг от друга.

class SubsystemA

{

&nbsp;   public void A1()

&nbsp;   {}

}

class SubsystemB

{

&nbsp;   public void B1()

&nbsp;   {}

}

class SubsystemC

{

&nbsp;   public void C1()

&nbsp;   {}

}

&nbsp;

public class Facade

{

&nbsp;   SubsystemA subsystemA;

&nbsp;   SubsystemB subsystemB;

&nbsp;   SubsystemC subsystemC;

&nbsp;

&nbsp;   public Facade(SubsystemA sa, SubsystemB sb, SubsystemC sc)

&nbsp;   {

&nbsp;       subsystemA = sa;

&nbsp;       subsystemB = sb;

&nbsp;       subsystemC = sc;

&nbsp;   }

&nbsp;   public void Operation1()

&nbsp;   {

&nbsp;       subsystemA.A1();

&nbsp;       subsystemB.B1();

&nbsp;       subsystemC.C1();

&nbsp;   }

&nbsp;   public void Operation2()

&nbsp;   {

&nbsp;       subsystemB.B1();

&nbsp;       subsystemC.C1();

&nbsp;   }

}

&nbsp;

class Client

{

&nbsp;   public void Main()

&nbsp;   {

&nbsp;       Facade facade = new Facade(new SubsystemA(), new SubsystemB(), new SubsystemC());

&nbsp;       facade.Operation1();

&nbsp;       facade.Operation2();

&nbsp;   }

}

- Классы SubsystemA, SubsystemB, SubsystemC и т.д. являются компонентами сложной подсистемы, с которыми должен взаимодействовать клиент
- Client взаимодействует с компонентами подсистемы
- Facade - непосредственно фасад, который предоставляет интерфейс клиенту для работы с компонентами

1. **Компоновщик (Composite)**

Паттерн Компоновщик (Composite) объединяет группы объектов в древовидную структуру по принципу "часть-целое и позволяет клиенту одинаково работать как с отдельными объектами, так и с группой объектов.

Образно реализацию паттерна можно представить в виде меню, которое имеет различные пункты. Эти пункты могут содержать подменю, в которых, в свою очередь, также имеются пункты. То есть пункт меню служит с одной стороны частью меню, а с другой стороны еще одним меню. В итоге мы однообразно можем работать как с пунктом меню, так и со всем меню в целом.

### Когда использовать компоновщик?

- Когда объекты должны быть реализованы в виде иерархической древовидной структуры
- Когда клиенты единообразно должны управлять как целыми объектами, так и их составными частями. То есть целое и его части должны реализовать один и тот же интерфейс

class Client

{

&nbsp;   public void Main()

&nbsp;   {

&nbsp;       Component root = new Composite("Root");

&nbsp;       Component leaf = new Leaf("Leaf");

&nbsp;       Composite subtree = new Composite("Subtree");

&nbsp;       root.Add(leaf);

&nbsp;       root.Add(subtree);

&nbsp;       root.Display();

&nbsp;   }

}

abstract class Component

{

&nbsp;   protected string name;

&nbsp;

&nbsp;   public Component(string name)

&nbsp;   {

&nbsp;       this.name = name;

&nbsp;   }

&nbsp;

&nbsp;   public abstract void Display();

&nbsp;   public abstract void Add(Component c);

&nbsp;   public abstract void Remove(Component c);

}

class Composite : Component

{

&nbsp;   List&lt;Component&gt; children = new List&lt;Component&gt;();

&nbsp;

&nbsp;   public Composite(string name)

&nbsp;       : base(name)

&nbsp;   {}

&nbsp;

&nbsp;   public override void Add(Component component)

&nbsp;   {

&nbsp;       children.Add(component);

&nbsp;   }

&nbsp;

&nbsp;   public override void Remove(Component component)

&nbsp;   {

&nbsp;       children.Remove(component);

&nbsp;   }

&nbsp;

&nbsp;   public override void Display()

&nbsp;   {

&nbsp;       Console.WriteLine(name);

&nbsp;

&nbsp;       foreach (Component component in children)

&nbsp;       {

&nbsp;           component.Display();

&nbsp;       }

&nbsp;   }

}

class Leaf : Component

{

&nbsp;   public Leaf(string name)

&nbsp;       : base(name)

&nbsp;   {}

&nbsp;

&nbsp;   public override void Display()

&nbsp;   {

&nbsp;       Console.WriteLine(name);

&nbsp;   }

&nbsp;

&nbsp;   public override void Add(Component component)

&nbsp;   {

&nbsp;       throw new NotImplementedException();

&nbsp;   }

&nbsp;

&nbsp;   public override void Remove(Component component)

&nbsp;   {

&nbsp;       throw new NotImplementedException();

&nbsp;   }

}

- **Component**: определяет интерфейс для всех компонентов в древовидной структуре
- **Composite**: представляет компонент, который может содержать другие компоненты и реализует механизм для их добавления и удаления
- **Leaf**: представляет отдельный компонент, который не может содержать другие компоненты
- **Client**: клиент, который использует компоненты

1. **Проксі**

Паттерн Заместитель (Proxy) предоставляет объект-заместитель, который управляет доступом к другому объекту. То есть создается объект-суррогат, который может выступать в роли другого объекта и замещать его.

### Когда использовать прокси?

- Когда надо осуществлять взаимодействие по сети, а объект-проси должен имитировать поведения объекта в другом адресном пространстве. Использование прокси позволяет снизить накладные издержки при передачи данных через сеть. Подобная ситуация еще называется **удалённый заместитель (remote proxies)**
- Когда нужно управлять доступом к ресурсу, создание которого требует больших затрат. Реальный объект создается только тогда, когда он действительно может понадобится, а до этого все запросы к нему обрабатывает прокси-объект. Подобная ситуация еще называется **виртуальный заместитель (virtual proxies)**
- Когда необходимо разграничить доступ к вызываемому объекту в зависимости от прав вызывающего объекта. Подобная ситуация еще называется **защищающий заместитель (protection proxies)**
- Когда нужно вести подсчет ссылок на объект или обеспечить потокобезопасную работу с реальным объектом. Подобная ситуация называется **"умные ссылки" (smart reference)**

class Client

{

&nbsp;   void Main()

&nbsp;   {

&nbsp;       Subject subject = new Proxy();

&nbsp;       subject.Request();

&nbsp;   }

}

abstract class Subject

{

&nbsp;   public abstract void Request();

}

&nbsp;

class RealSubject : Subject

{

&nbsp;   public override void Request()

&nbsp;   {}

}

class Proxy : Subject

{

&nbsp;   RealSubject realSubject;

&nbsp;   public override void Request()

&nbsp;   {

&nbsp;       if (realSubject == null)

&nbsp;           realSubject = new RealSubject();

&nbsp;       realSubject.Request();

&nbsp;   }

}

- **Subject**: определяет общий интерфейс для Proxy и RealSubject. Поэтому Proxy может использоваться вместо RealSubject
- **RealSubject**: представляет реальный объект, для которого создается прокси
- **Proxy**: заместитель реального объекта. Хранит ссылку на реальный объект, контролирует к нему доступ, может управлять его созданием и удалением. При необходимости Proxy переадресует запросы объекту RealSubject
- **Client**: использует объект Proxy для доступа к объекту RealSubject

1. **Міст**

Мост (Bridge) - структурный шаблон проектирования, который позволяет отделить абстракцию от реализации таким образом, чтобы и абстракцию, и реализацию можно было изменять независимо друг от друга.

Даже если мы отделим абстракцию от конкретных реализаций, то у нас все равно все наследуемые классы будут жестко привязаны к интерфейсу, определяемому в базовом абстрактном классе. Для преодоления жестких связей и служит паттерн Мост.

### Когда использовать данный паттерн?

- Когда надо избежать постоянной привязки абстракции к реализации
- Когда наряду с реализацией надо изменять и абстракцию независимо друг от друга. То есть изменения в абстракции не должно привести к изменениям в реализации

Общая реализация паттерна состоит в объявлении классов абстракций и классов реализаций в отдельных параллельных иерархиях классов.

Связь агрегации между классами Abstraction и Implementor фактически и представляет некоторый мост между двумя параллельными иерархиями классов. Собственно поэтому паттерн получил название Мост.

class Client

{

&nbsp;   static void Main()

&nbsp;   {

&nbsp;       Abstraction abstraction;

&nbsp;       abstraction = new RefinedAbstraction(new ConcreteImplementorA());

&nbsp;       abstraction.Operation();

&nbsp;       abstraction.Implementor=new ConcreteImplementorB();

&nbsp;       abstraction.Operation();

&nbsp;   }

}

abstract class Abstraction

{

&nbsp;   protected Implementor implementor;

&nbsp;   public Implementor Implementor

&nbsp;   {

&nbsp;       set { implementor = value; }

&nbsp;   }

&nbsp;   public Abstraction(Implementor imp)

&nbsp;   {

&nbsp;       implementor = imp;

&nbsp;   }

&nbsp;   public virtual void Operation()

&nbsp;   {

&nbsp;       implementor.OperationImp();

&nbsp;   }

}

&nbsp;

abstract class Implementor

{

&nbsp;   public abstract void OperationImp();

}

&nbsp;

class RefinedAbstraction : Abstraction

{

&nbsp;   public RefinedAbstraction(Implementor imp)

&nbsp;       : base(imp)

&nbsp;   {}

&nbsp;   public override void Operation()

&nbsp;   {

&nbsp;   }

}

&nbsp;

class ConcreteImplementorA : Implementor

{

&nbsp;   public override void OperationImp()

&nbsp;   {

&nbsp;   }

}

&nbsp;

class ConcreteImplementorB : Implementor

{

&nbsp;   public override void OperationImp()

&nbsp;   {

&nbsp;   }

}

- **Abstraction**: определяет базовый интерфейс и хранит ссылку на объект Implementor. Выполнение операций в Abstraction делегируется методам объекта Implementor
- **RefinedAbstraction**: уточненная абстракция, наследуется от Abstraction и расширяет унаследованный интерфейс
- **Implementor**: определяет базовый интерфейс для конкретных реализаций. Как правило, Implementor определяет только примитивные операции. Более сложные операции, которые базируются на примитивных, определяются в Abstraction
- **ConcreteImplementorA** и **ConcreteImplementorB**: конкретные реализации, которые унаследованы от Implementor
- **Client**: использует объекты Abstraction

1. **Приспособленець (Flyweight)**

Паттерн Приспособленец (Flyweight) - структурный шаблон проектирования, который позволяет использовать разделяемые объекты сразу в нескольких контекстах. Данный паттерн используется преимущественно для оптимизации работы с памятью.

В качестве стандартного применения данного паттерна можно привести следующий пример. Текст состоит из отдельных символов. Каждый символ может встречаться на одной странице текста много раз. Однако в компьютерной программе было бы слишком накладно выделять память для каждого отдельного символа в тексте. Гораздо проще было бы определить полный набор символов, например, в виде таблицы из 128 знаков (алфавитно-цифровые символы в разных регистрах, знаки препинания и т.д.). А в тексте применить этот набор общих разделяемых символов, вместо сотен и тысяч объектов, которые могли бы использоваться в тексте. И как следствие подобного подхода будет уменьшение количества используемых объектов и уменьшение используемой памяти.

Паттерн Приспособленец следует применять при соблюдении всех следующих условий:

- Когда приложение использует большое количество однообразных объектов, из-за чего происходит выделение большого количества памяти
- Когда часть состояния объекта, которое является изменяемым, можно вынести во вне. Вынесение внешнего состояния позволяет заменить множество объектов небольшой группой общих разделяемых объектов.

Ключевым моментом здесь является разделение состояния на внутренне и внешнее. Внутреннее состояние не зависит от контекста. В примере с символами внутреннее состояние описывается кодом символа из таблицы кодировки. Так как внутреннее состояние не зависит от контекста, то оно может быть разделяемым и поэтому выносится в разделяемые объекты.

Внешнее состояние зависит от контекста, является изменчивым. В применении к символам внешнее состояние может представлять положение символа на странице. То есть код символа может быть использован многими символами, тогда как положение на странице будет для каждого символа индивидуально.

При создании приспособленца внешнее состояние выносится. В приспособленце остается только внутреннее состояние. То есть в примере с символами приспособленец будет хранить код символа.

class FlyweightFactory

{

&nbsp;   Hashtable flyweights = new Hashtable();

&nbsp;   public FlyweightFactory()

&nbsp;   {

&nbsp;       flyweights.Add("X", new ConcreteFlyweight());

&nbsp;       flyweights.Add("Y", new ConcreteFlyweight());

&nbsp;       flyweights.Add("Z", new ConcreteFlyweight());

&nbsp;   }

&nbsp;   public Flyweight GetFlyweight(string key)

&nbsp;   {

&nbsp;       if (!flyweights.ContainsKey(key))

&nbsp;           flyweights.Add(key, new ConcreteFlyweight());

&nbsp;       return flyweights\[key\] as Flyweight;

&nbsp;   }

}

&nbsp;

abstract class Flyweight

{

&nbsp;   public abstract void Operation(int extrinsicState);

}

&nbsp;

class ConcreteFlyweight : Flyweight

{

&nbsp;   int intrinsicState;

&nbsp;   public override void Operation(int extrinsicState)

&nbsp;   {

&nbsp;   }

}

&nbsp;

class UnsharedConcreteFlyweight : Flyweight

{

&nbsp;   int allState;

&nbsp;   public override void Operation(int extrinsicState)

&nbsp;   {

&nbsp;       allState = extrinsicState;

&nbsp;   }

}

&nbsp;

class Client

{

&nbsp;   void Main()

&nbsp;   {

&nbsp;       int extrinsicstate = 22;

&nbsp;

&nbsp;       FlyweightFactory f = new FlyweightFactory();

&nbsp;

&nbsp;       Flyweight fx = f.GetFlyweight("X");

&nbsp;       fx.Operation(--extrinsicstate);

&nbsp;

&nbsp;       Flyweight fy = f.GetFlyweight("Y");

&nbsp;       fy.Operation(--extrinsicstate);

&nbsp;

&nbsp;       Flyweight fd = f.GetFlyweight("D");

&nbsp;       fd.Operation(--extrinsicstate);

&nbsp;

&nbsp;       UnsharedConcreteFlyweight uf = new UnsharedConcreteFlyweight();

&nbsp;

&nbsp;       uf.Operation(--extrinsicstate);

&nbsp;   }

}

- **Flyweight**: определяет интерфейс, через который приспособленцы-разделяемые объекты могут получать внешнее состояние или воздействовать на него
- **ConcreteFlyweight**: конкретный класс разделяемого приспособленца. Реализует интерфейс, объявленный в типе Flyweight, и при необходимости добавляет внутреннее состояние. Причем любое сохраняемое им состояние должно быть внутренним, не зависящим от контекста
- **UnsharedConcreteFlyweight**: еще одна конкретная реализация интерфейса, определенного в типе Flyweight, только теперь объекты этого класса являются неразделяемыми
- **FlyweightFactory**: фабрика приспособленцев - создает объекты разделяемых приспособленцев. Так как приспособленцы разделяются, то клиент не должен создавать их напрямую. Все созданные объекты хранятся в пуле. В примере выше для определения пула используется объект Hashtable, но это не обязательно. Можно применять и другие классы коллекций. Однако в зависимости от сложности структуры, хранящей разделяемые объекты, особенно если у нас большое количество приспособленцев, то может увеличиваться время на поиск нужного приспособленца - наверное это один из немногих недостатков данного паттерна.

Если запрошенного приспособленца не оказалось в пуле, то фабрика создает его.

- **Client**: использует объекты приспособленцев. Может хранить внешнее состояние и передавать его в качестве аргументов в методы приспособленцев

**Adapter** adapts a given class/object to a new interface. In the case of the former, multiple inheritance is typically employed. In the latter case, the object is wrapped by a conforming adapter object and passed around. The problem we are solving here is that of _non-compatible interfaces_.

**Facade** is more like a simple gateway to a complicated set of functionality. You make a black-box for your clients to worry less i.e. _make interfaces simpler_.

**Proxy** provides the same interface as the proxied-for class and typically does some housekeeping stuff on its own. (So instead of making multiple copies of a heavy object X you make copies of a lightweight proxy P which in turn manages X and translates your calls as required.) You are solving the problem of the client from having to _manage a heavy and/or complex object_.

**Decorator** is used to add more gunpowder to your objects (note the term objects -- you typically decorate objects dynamically at runtime). You do not hide/impair the existing interfaces of the object but _simply extend it at runtime_.