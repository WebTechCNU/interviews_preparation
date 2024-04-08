**CSHARP**

1. **Accessibility levels (modifiers)**

Модификаторы доступа позволяют задать допустимую область видимости для членов класса.

В C# применяются следующие модификаторы доступа:

- **public**: публичный, общедоступный класс или член класса. Такой член класса доступен из любого места в коде, а также из других программ и сборок.
- **private**: закрытый класс или член класса. Представляет полную противоположность модификатору public. Такой закрытый класс или член класса доступен только из кода в том же классе или контексте.
- **protected**: такой член класса доступен из любого места в текущем классе или в производных классах. При этом производные классы могут располагаться в других сборках.
- **internal**: класс и члены класса с подобным модификатором доступны из любого места кода в той же сборке, однако он недоступен для других программ и сборок (как в случае с модификатором public).
- **protected internal**: совмещает функционал двух модификаторов. Классы и члены класса с таким модификатором доступны из текущей сборки и из производных классов.
- **private protected**: такой член класса доступен из любого места в текущем классе или в производных классах, которые определены в той же сборке.

Если для полей и методов не определен модификатор доступа, то по умолчанию для них применяется модификатор **private**.

Классы и структуры, объявленные без модификатора, по умолчанию имеют доступ **internal**.

Все классы и структуры, определенные напрямую в пространствах имен и не являющиеся вложенными в другие классы, могут иметь только модификаторы public или internal.

Благодаря такой системе модификаторов доступа можно скрывать некоторые моменты реализации класса от других частей программы.

Несмотря на то, что модификаторы public и internal похожи по своему действию, но они имеют большое отличие. Классы и члены класса с модификатором public также будут доступны и другим программам, если данных класс поместить в динамическую библиотеку dll и потом ее использовать в этих программах.

1. **Virtual keyword**

The virtual keyword is used to modify a method, property, indexer, or event declaration and allow for it to be overridden in a derived class.

The implementation of a virtual member can be changed by an overriding member in a derived class.

When a virtual method is invoked, the run-time type of the object is checked for an overriding member. The overriding member in the most derived class is called, which might be the original member, if no derived class has overridden the member.

By default, methods are non-virtual. You cannot override a non-virtual method.

You cannot use the virtual modifier with the static, abstract, private, or override modifiers.

А чтобы переопределить метод в классе-наследнике, этот метод определяется с модификатором **override**. Переопределенный метод в классе-наследнике должен иметь тот же набор параметров, что и виртуальный метод в базовом классе.

При переопределении виртуальных методов следует учитывать ряд ограничений:

- Виртуальный и переопределенный методы должны иметь один и тот же модификатор доступа. То есть если виртуальный метод определен с помощью модификатора public, то и переопредленный метод также должен иметь модификатор public.
- Нельзя переопределить или объявить виртуальным статический метод.

Также как и методы, можно переопределять свойства

Также можно запретить переопределение методов и свойств. В этом случае их надо объявлять с модификатором **sealed**

При создании методов с модификатором sealed надо учитывать, что sealed применяется в паре с override, то есть только в переопределяемых методах.

1. **new Modifier**

When used as a declaration modifier, the new keyword explicitly hides a member that is inherited from a base class. When you hide an inherited member, the derived version of the member replaces the base class version. Although you can hide members without using the new modifier, you get a compiler warning. If you use new to explicitly hide a member, it suppresses this warning.

You can also use the new keyword to [create an instance of a type](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/new-operator) or as a [generic type constraint](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/new-constraint).

To hide an inherited member, declare it in the derived class by using the same member name, and modify it with the new keyword. For example:

public class BaseC

{

public int x;

public void Invoke() { }

}

public class DerivedC : BaseC

{

new public void Invoke() { }

}

In this example, BaseC.Invoke is hidden by DerivedC.Invoke. The field x is not affected because it is not hidden by a similar name.

Name hiding through inheritance takes one of the following forms:

- Generally, a constant, field, property, or type that is introduced in a class or struct hides all base class members that share its name. There are special cases. For example, if you declare a new field with name N to have a type that is not invocable, and a base type declares N to be a method, the new field does not hide the base declaration in invocation syntax. For more information, see the [Member lookup](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/expressions#member-lookup) section of the [C# language specification](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/introduction).
- A method introduced in a class or struct hides properties, fields, and types that share that name in the base class. It also hides all base class methods that have the same signature.
- An indexer introduced in a class or struct hides all base class indexers that have the same signature.

It is an error to use both new and [override](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/override) on the same member, because the two modifiers have mutually exclusive meanings. The new modifier creates a new member with the same name and causes the original member to become hidden. The override modifier extends the implementation for an inherited member.

Using the new modifier in a declaration that does not hide an inherited member generates a warning.

1. **volatile**

The volatile keyword indicates that a field might be modified by multiple threads that are executing at the same time. The compiler, the runtime system, and even hardware may rearrange reads and writes to memory locations for performance reasons. Fields that are declared volatile are not subject to these optimizations. Adding the volatile modifier ensures that all threads will observe volatile writes performed by any other thread in the order in which they were performed. There is no guarantee of a single total ordering of volatile writes as seen from all threads of execution.

The volatile keyword can be applied to fields of these types:

- Reference types.
- Pointer types (in an unsafe context). Note that although the pointer itself can be volatile, the object that it points to cannot. In other words, you cannot declare a "pointer to volatile."
- Simple types such as sbyte, byte, short, ushort, int, uint, char, float, and bool.
- An enum type with one of the following base types: byte, sbyte, short, ushort, int, or uint.
- Generic type parameters known to be reference types.
- [IntPtr](https://docs.microsoft.com/en-us/dotnet/api/system.intptr) and [UIntPtr](https://docs.microsoft.com/en-us/dotnet/api/system.uintptr).

Other types, including double and long, cannot be marked volatile because reads and writes to fields of those types cannot be guaranteed to be atomic. To protect multi-threaded access to those types of fields, use the [Interlocked](https://docs.microsoft.com/en-us/dotnet/api/system.threading.interlocked) class members or protect access using the [lock](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/lock-statement) statement.

The volatile keyword can only be applied to fields of a class or struct. Local variables cannot be declared volatile.

1. **unsafe**

The unsafe keyword denotes an unsafe context, which is required for any operation involving pointers. You can use the unsafe modifier in the declaration of a type or a member. The entire textual extent of the type or member is therefore considered an unsafe context. The scope of the unsafe context extends from the parameter list to the end of the method, so pointers can also be used in the parameter list. You can also use an unsafe block to enable the use of an unsafe code inside this block. To compile unsafe code, you must specify the [\-unsafe](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/compiler-options/unsafe-compiler-option) compiler option. Unsafe code is not verifiable by the common language runtime.

// compile with: -unsafe

class UnsafeTest

{

// Unsafe method: takes pointer to int.

unsafe static void SquarePtrParam(int\* p)

{

\*p \*= \*p;

}

unsafe static void Main()

{

int i = 5;

SquarePtrParam(&i); // Unsafe method: uses address-of operator (&).

Console.WriteLine(i);

}

} // Output: 25

1. **abstract**

The abstract modifier indicates that the thing being modified has a missing or incomplete implementation. The abstract modifier can be used with classes, methods, properties, indexers, and events. Use the abstract modifier in a class declaration to indicate that a class is intended only to be a base class of other classes, not instantiated on its own. Members marked as abstract must be implemented by non-abstract classes that derive from the abstract class.

Abstract classes have the following features:

- An abstract class cannot be instantiated.
- An abstract class may contain abstract methods and accessors.
- It is not possible to modify an abstract class with the [sealed](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/sealed) modifier because the two modifiers have opposite meanings. The sealed modifier prevents a class from being inherited and the abstract modifier requires a class to be inherited.
- A non-abstract class derived from an abstract class must include actual implementations of all inherited abstract methods and accessors.

Use the abstract modifier in a method or property declaration to indicate that the method or property does not contain implementation.

Abstract methods have the following features:

- An abstract method is implicitly a virtual method.
- Abstract method declarations are only permitted in abstract classes.
- Because an abstract method declaration provides no actual implementation, there is no method body; the method declaration simply ends with a semicolon and there are no curly braces ({ }) following the signature.
- It is an error to use the [static](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/static) or [virtual](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/virtual) modifiers in an abstract method declaration.

Abstract properties behave like abstract methods, except for the differences in declaration and invocation syntax.

- It is an error to use the abstract modifier on a static property.
- An abstract inherited property can be overridden in a derived class by including a property declaration that uses the [override](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/override) modifier.

4. **FIRST principles of unit testing**

Fast

- The developer shouldn’t hesitate to run the unit tests at any point of their development cycle, even if there are thousands of unit tests. They should run and show you the desired output in a matter of seconds

Isolated

- For any given unit test, for its environment variables or for its setup. It should be independent of everything else should so that it results is not influenced by any other factor.
- Should follow the [**_3 A’s of testing: Arrange, Act, Assert_**](https://xp123.com/articles/3a-arrange-act-assert/)
- In some literature, it’s also called as [Given, when, then](https://martinfowler.com/bliki/GivenWhenThen.html).

**_Arrange_** All the data should be provided to the test when you’re about to run the test and it should not depend on the environment you are running the tests

**_Act_** Invoke the actual method under test

**_Assert_** At any given point, a unit test should only assert one logical outcome, multiple physical asserts can be part of this physical assert, as long as they all act on the state of the same object.

Preferably, don’t do any actions after the assert call

Repeatable

- tests should be repeatable and deterministic, their values shouldn’t change based on being run on different environments.
- Each test should set up its own data and should not depend on any external factors to run its test

**Self-validating**

- you shouldn’t need to check manually, whether the test passed or not.

Thorough

- should cover all the happy paths
- try covering all the edge cases, where the author would feel the function would fail.
- test for illegal arguments and variables.
- test for security and other issues
- test for large values, what would a large input do their program.
- should try to cover every use case scenario and not just aim for 100% code coverage.

1. **Garbage collector**

In the common language runtime (CLR), the garbage collector (GC) serves as an automatic memory manager. The garbage collector manages the allocation and release of memory for an application. For developers working with managed code, this means that you don't have to write code to perform memory management tasks. Automatic memory management can eliminate common problems, such as forgetting to free an object and causing a memory leak or attempting to access memory for an object that's already been freed.

Garbage collection occurs when one of the following conditions is true:

- The system has low physical memory. This is detected by either the low memory notification from the OS or low memory as indicated by the host.
- The memory that's used by allocated objects on the managed heap surpasses an acceptable threshold. This threshold is continuously adjusted as the process runs.
- The [GC.Collect](https://docs.microsoft.com/en-us/dotnet/api/system.gc.collect) method is called. In almost all cases, you don't have to call this method, because the garbage collector runs continuously. This method is primarily used for unique situations and testing.

## Benefits

The garbage collector provides the following benefits:

- Frees developers from having to manually release memory.
- Allocates objects on the managed heap efficiently.
- Reclaims objects that are no longer being used, clears their memory, and keeps the memory available for future allocations. Managed objects automatically get clean content to start with, so their constructors don't have to initialize every data field.
- Provides memory safety by making sure that an object cannot use the content of another object.

After the garbage collector is initialized by the CLR, it allocates a segment of memory to store and manage objects. This memory is called the managed heap, as opposed to a native heap in the operating system.

There is a managed heap for each managed process. All threads in the process allocate memory for objects on the same heap.

To reserve memory, the garbage collector calls the Windows [VirtualAlloc](https://docs.microsoft.com/en-us/windows/desktop/api/memoryapi/nf-memoryapi-virtualalloc) function and reserves one segment of memory at a time for managed applications. The garbage collector also reserves segments, as needed, and releases segments back to the operating system (after clearing them of any objects) by calling the Windows [VirtualFree](https://docs.microsoft.com/en-us/windows/desktop/api/memoryapi/nf-memoryapi-virtualfree) function.

The GC algorithm is based on several considerations:

- It's faster to compact the memory for a portion of the managed heap than for the entire managed heap.
- Newer objects have shorter lifetimes and older objects have longer lifetimes.
- Newer objects tend to be related to each other and accessed by the application around the same time.

Garbage collection primarily occurs with the reclamation of short-lived objects. To optimize the performance of the garbage collector, the managed heap is divided into three generations, 0, 1, and 2, so it can handle long-lived and short-lived objects separately. The garbage collector stores new objects in generation 0. Objects created early in the application's lifetime that survive collections are promoted and stored in generations 1 and 2. Because it's faster to compact a portion of the managed heap than the entire heap, this scheme allows the garbage collector to release the memory in a specific generation rather than release the memory for the entire managed heap each time it performs a collection.

- **Generation 0**. This is the youngest generation and contains short-lived objects. An example of a short-lived object is a temporary variable. Garbage collection occurs most frequently in this generation.

Newly allocated objects form a new generation of objects and are implicitly generation 0 collections. However, if they are large objects, they go on the large object heap (LOH), which is sometimes referred to as _generation 3_. Generation 3 is a physical generation that's logically collected as part of generation 2.

Most objects are reclaimed for garbage collection in generation 0 and don't survive to the next generation.

If an application attempts to create a new object when generation 0 is full, the garbage collector performs a collection in an attempt to free address space for the object. The garbage collector starts by examining the objects in generation 0 rather than all objects in the managed heap. A collection of generation 0 alone often reclaims enough memory to enable the application to continue creating new objects.

- **Generation 1**. This generation contains short-lived objects and serves as a buffer between short-lived objects and long-lived objects.

After the garbage collector performs a collection of generation 0, it compacts the memory for the reachable objects and promotes them to generation 1. Because objects that survive collections tend to have longer lifetimes, it makes sense to promote them to a higher generation. The garbage collector doesn't have to reexamine the objects in generations 1 and 2 each time it performs a collection of generation 0.

If a collection of generation 0 does not reclaim enough memory for the application to create a new object, the garbage collector can perform a collection of generation 1, then generation 2. Objects in generation 1 that survive collections are promoted to generation 2.

- **Generation 2**. This generation contains long-lived objects. An example of a long-lived object is an object in a server application that contains static data that's live for the duration of the process.

Objects in generation 2 that survive a collection remain in generation 2 until they are determined to be unreachable in a future collection.

Objects on the large object heap (which is sometimes referred to as _generation 3_) are also collected in generation 2.

Garbage collections occur on specific generations as conditions warrant. Collecting a generation means collecting objects in that generation and all its younger generations. A generation 2 garbage collection is also known as a full garbage collection, because it reclaims objects in all generations (that is, all objects in the managed heap).

For most of the objects that your application creates, you can rely on garbage collection to automatically perform the necessary memory management tasks. However, unmanaged resources require explicit cleanup. The most common type of unmanaged resource is an object that wraps an operating system resource, such as a file handle, window handle, or network connection. Although the garbage collector is able to track the lifetime of a managed object that encapsulates an unmanaged resource, it doesn't have specific knowledge about how to clean up the resource.

When you create an object that encapsulates an unmanaged resource, it's recommended that you provide the necessary code to clean up the unmanaged resource in a public Dispose method. By providing a Dispose method, you enable users of your object to explicitly free its memory when they are finished with the object. When you use an object that encapsulates an unmanaged resource, make sure to call Dispose as necessary.

You must also provide a way for your unmanaged resources to be released in case a consumer of your type forgets to call Dispose. You can either use a safe handle to wrap the unmanaged resource, or override the [Object.Finalize()](https://docs.microsoft.com/en-us/dotnet/api/system.object.finalize"%20\l%20"System_Object_Finalize) method.

1. **Large Object Heap**

The .NET garbage collector (GC) divides objects up into small and large objects. When an object is large, some of its attributes become more significant than if the object is small. For instance, compacting it—that is, copying it in memory elsewhere on the heap—can be expensive. Because of this, the garbage collector places large objects on the large object heap (LOH).

If an object is greater than or equal to 85,000 bytes in size, it’s considered a large object. This number was determined by performance tuning. When an object allocation request is for 85,000 or more bytes, the runtime allocates it on the large object heap.

Large objects belong to generation 2 because they are collected only during a generation 2 collection. When a generation is collected, all its younger generation(s) are also collected. For example, when a generation 1 GC happens, both generation 1 and 0 are collected. And when a generation 2 GC happens, the whole heap is collected. For this reason, a generation 2 GC is also called a full GC.

Generations provide a logical view of the GC heap. Physically, objects live in managed heap segments. A managed heap segment is a chunk of memory that the GC reserves from the OS by calling the [VirtualAlloc function](https://docs.microsoft.com/en-us/windows/desktop/api/memoryapi/nf-memoryapi-virtualalloc) on behalf of managed code. When the CLR is loaded, the GC allocates two initial heap segments: one for small objects (the small object heap, or SOH), and one for large objects (the large object heap).

User code can only allocate in generation 0 (small objects) or the LOH (large objects). Only the GC can “allocate” objects in generation 1 (by promoting survivors from generation 0) and generation 2 (by promoting survivors from generations 1 and 2).

In general, a GC occurs under one of the following three conditions:

- Allocation exceeds the generation 0 or large object threshold.

The threshold is a property of a generation. A threshold for a generation is set when the garbage collector allocates objects into it. When the threshold is exceeded, a GC is triggered on that generation. When you allocate small or large objects, you consume generation 0 and the LOH’s thresholds, respectively. When the garbage collector allocates into generation 1 and 2, it consumes their thresholds. These thresholds are dynamically tuned as the program runs.

This is the typical case; most GCs happen because of allocations on the managed heap.

- The [GC.Collect](https://docs.microsoft.com/en-us/dotnet/api/system.gc.collect) method is called.

If the parameterless [GC.Collect()](https://docs.microsoft.com/en-us/dotnet/api/system.gc.collect"%20\l%20"System_GC_Collect) method is called or another overload is passed [GC.MaxGeneration](https://docs.microsoft.com/en-us/dotnet/api/system.gc.maxgeneration"%20\l%20"System_GC_MaxGeneration) as an argument, the LOH is collected along with the rest of the managed heap.

- The system is in low memory situation.

This occurs when the garbage collector receives a high memory notification from the OS. If the garbage collector thinks that doing a generation 2 GC will be productive, it triggers one.

1. **IEnumerable and IQueryable difference**

First of all, [IQueryable&lt;T&gt;](http://msdn.microsoft.com/en-us/library/bb351562.aspx) _extends_ the IEnumerable&lt;T&gt; interface, so anything you can do with a "plain" IEnumerable&lt;T&gt;, you can also do with an IQueryable&lt;T&gt;.

IEnumerable&lt;T&gt; just has a GetEnumerator() method that returns an Enumerator&lt;T&gt; for which you can call its MoveNext() method to iterate through a sequence of _T_.

What IQueryable&lt;T&gt; has that IEnumerable&lt;T&gt; _doesn't_ are two properties in particular—one that points to a **query provider** (e.g., a LINQ to SQL provider) and another one pointing to a **query expression** representing the IQueryable&lt;T&gt; object as a runtime-traversable abstract syntax tree that can be understood by the given query provider (for the most part, you can't give a LINQ to SQL expression to a LINQ to Entities provider without an exception being thrown).

The expression can simply be a constant expression of the object itself or a more complex tree of a composed set of query operators and operands. The query provider's IQueryProvider.Execute() or IQueryProvider.CreateQuery() methods are called with an _Expression_ passed to it, and then either a query result or another IQueryable is returned, respectively.

The primary difference is that the LINQ operators for IQueryable&lt;T&gt; take Expression objects instead of delegates, meaning the custom query logic it receives, e.g., a predicate or value selector, is in the form of an expression tree instead of a delegate to a method.

- IEnumerable&lt;T&gt; is great for working with sequences that are iterated in-memory, but
- IQueryable&lt;T&gt; allows for out-of memory things like a remote data source, such as a database or web service.
- IEnumerable exists in System.Collections Namespace.
- IQueryable exists in System. Linq Namespace.
- Both IEnumerable and IQueryable are forward collection.
- IEnumerable doesn’t support lazy loading
- IQueryable support lazy loading
- Querying data from a database, IEnumerable execute a select query on the server side, load data in-memory on a client-side and then filter data.
- Querying data from a database, IQueryable execute the select query on the server side with all filters.
- IEnumerable Extension methods take functional objects.
- IQueryable Extension methods take expression objects means expression tree.

Where  or any other extension method for IEnumerable takes Func delegate(Func&lt;Employee,bool&gt; above) where as IQueryable extension methods take Expression (Expression&lt;Func<Employee,bool&gt;> above).

When compiled, Func  gets converted into code i.e. it would be converted into a normal method whereas Expression will be converted into an expression tree of objects (but will not be executed).Implication of this fact is that func call will be executed immediately but expression tree would be provided to  a query provider and will be converted into a data source query which will be executed in the data source.This is called deferred execution.In our case data source is SQL Server but it can be anything.Thats how LinqToSql or Linq to Anything else works.

1. **Enum flags**

FlagsAttribute - Indicates that an enumeration can be treated as a bit field; that is, a set of flags.

The \[Flags\] attribute should be used whenever the enumerable represents a collection of possible values, rather than a single value. Such collections are often used with bitwise operators.

Note that the \[Flags\] attribute **doesn't** enable this by itself - all it does is allow a nice representation by the .ToString() method.

enum Suits { Spades = 1, Clubs = 2, Diamonds = 4, Hearts = 8 }

\[Flags\] enum SuitsFlags { Spades = 1, Clubs = 2, Diamonds = 4, Hearts = 8 }

...

var str1 = (Suits.Spades | Suits.Diamonds).ToString(); // "5"

var str2 = (SuitsFlags.Spades | SuitsFlags.Diamonds).ToString(); // "Spades, Diamonds"

Console.WriteLine((SuitsFlags)5); // "Spades, Diamonds"

This works because you used powers of two in your enumeration.

Yellow: 00000001

Green: 00000010

Red: 00000100

Blue: 00001000

1. **IList&lt;T&gt; and IEnumerable&lt;T&gt; difference**

One important difference between IEnumerable and List (besides one being an interface and the other being a concrete class) is that IEnumerable is read-only and List is not.

So if you need the ability to make permanent changes of any kind to your collection (add & remove), you'll need List. If you just need to read, sort and/or filter your collection, IEnumerable is sufficient for that purpose.

- **IEnumerable&lt;T&gt; :** IEnumerable is best where you want to query in memory collections.It is suitable for LinqToObject and LinqToXml queries.
- **IQueryable&lt;T&gt;:** Best for querying data from external data sources especially which have some kind of query languages defined like databases (Sql queries )or sharepoint(CAML query).In such cases expression trees can be converted to data source specific queries which can be executed on remote data sources along with all the filters.For this reason in such situations performance of IQueryable is better than IEnumerable.
- **ICollection&lt;T&gt; :** Best when you only need collection based functionality like iteration ,adding ,removing etc and don’t need querying functionality
- **IList&lt;T&gt; :** Best when along with iteration you need indexed based access as well.

1. **Array T\[\] and List&lt;T&gt; difference**

The main difference is that you can add new elements to a List&lt;T&gt;.

Internally List&lt;T&gt; stores elements in an array of type T\[\] and it just automatically allocates a larger array when you're adding new elements (or shrinks the array when you're removing elements). This means that the performance will be roughly similar. There is some minor indirection when using List&lt;T&gt;, but the JITter may inline that.

The main reason for using List&lt;T&gt; is that it gives you more functionality - you can add and remove elements.

1. **LinkedList&lt;T&gt;**

Класс LinkedList&lt;T&gt; представляет двухсвязный список, в котором каждый элемент хранит ссылку одновременно на следующий и на предыдущий элемент.

Если в простом списке List&lt;T&gt; каждый элемент представляет объект типа T, то в LinkedList&lt;T&gt; каждый узел представляет объект класса LinkedListNode&lt;T&gt;. Этот класс имеет следующие свойства:

- **Value**: само значение узла, представленное типом T
- **Next**: ссылка на следующий элемент типа LinkedListNode&lt;T&gt; в списке. Если следующий элемент отсутствует, то имеет значение null
- **Previous**: ссылка на предыдущий элемент типа LinkedListNode&lt;T&gt; в списке. Если предыдущий элемент отсутствует, то имеет значение null

Используя методы класса LinkedList&lt;T&gt;, можно обращаться к различным элементам, как в конце, так и в начале списка:

- **AddAfter(LinkedListNode&lt;T&gt; node, LinkedListNode&lt;T&gt; newNode)**: вставляет узел newNode в список после узла node.
- **AddAfter(LinkedListNode&lt;T&gt; node, T value)**: вставляет в список новый узел со значением value после узла node.
- **AddBefore(LinkedListNode&lt;T&gt; node, LinkedListNode&lt;T&gt; newNode)**: вставляет в список узел newNode перед узлом node.
- **AddBefore(LinkedListNode&lt;T&gt; node, T value)**: вставляет в список новый узел со значением value перед узлом node.
- **AddFirst(LinkedListNode&lt;T&gt; node)**: вставляет новый узел в начало списка
- **AddFirst(T value)**: вставляет новый узел со значением value в начало списка
- **AddLast(LinkedListNode&lt;T&gt; node)**: вставляет новый узел в конец списка
- **AddLast(T value)**: вставляет новый узел со значением value в конец списка
- **RemoveFirst()**: удаляет первый узел из списка. После этого новым первым узлом становится узел, следующий за удаленным
- **RemoveLast()**: удаляет последний узел из списка

Методы вставки (AddLast, AddFirst) при добавлении в список возвращают ссылку на добавленный элемент LinkedListNode&lt;T&gt; (в нашем случае LinkedListNode&lt;Person&gt;). Затем управляя свойствами Previous и Next, мы можем получить ссылки на предыдущий и следующий узлы в списке.

[LinkedList&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlist-1?view=net-5.0) is a general-purpose linked list. It supports enumerators and implements the [ICollection](https://docs.microsoft.com/en-us/dotnet/api/system.collections.icollection?view=net-5.0) interface, consistent with other collection classes in the .NET Framework.

[LinkedList&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlist-1?view=net-5.0) provides separate nodes of type [LinkedListNode&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlistnode-1?view=net-5.0), so insertion and removal are O(1) operations.

You can remove nodes and reinsert them, either in the same list or in another list, which results in no additional objects allocated on the heap. Because the list also maintains an internal count, getting the [Count](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlist-1.count?view=net-5.0) property is an O(1) operation.

Each node in a [LinkedList&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlist-1?view=net-5.0) object is of the type [LinkedListNode&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlistnode-1?view=net-5.0). Because the [LinkedList&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlist-1?view=net-5.0) is doubly linked, each node points frward to the [Next](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlistnode-1.next?view=net-5.0) node and backward to the [Previous](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlistnode-1.previous?view=net-5.0) node.

Lists that contain reference types perform better when a node and its value are created at the same time. [LinkedList&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlist-1?view=net-5.0) accepts null as a valid [Value](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlistnode-1.value?view=net-5.0) property for reference types and allows duplicate values.

If the [LinkedList&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlist-1?view=net-5.0) is empty, the [First](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlist-1.first?view=net-5.0) and [Last](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlist-1.last?view=net-5.0) properties contain null.

The [LinkedList&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlist-1?view=net-5.0) class does not support chaining, splitting, cycles, or other features that can leave the list in an inconsistent state. The list remains consistent on a single thread. The only multithreaded scenario supported by [LinkedList&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlist-1?view=net-5.0) is multithreaded read operations.

При добавлении элемента в список List&lt;T&gt; в позицию отличную от начала или конца списка среда выполнения вынуждена сдвигать список, выделяя место под нов/2ую ссылку – это не самый рациональный способ быстрой вставки.

Благодаря двунаправленности, каждый элемент списка LinkedList&lt;T&gt; напрямую связан с каждым следующим и предыдущим элементом и косвенно (посредством ссылок в этих элементах) с каждым элементом в списке.  Операция вставки в середину проводится очень быстро.

Однако, за это приходится платить скоростью при поиске нужного элемента. Чтобы попасть в середину списка, нужно начав с начала (или с конца) переходить со ссылки на ссылку пока не попадешь на нужный.

Класс коллекции LinkedList&lt;T&gt; требует строгого следования элементов одного за другим, и не поддерживает операции способные нарушить целостность списка, такие как циклы, цепочки или разбиения.

Реализованные интерфейсы: ICollection, ICollection&lt;T&gt;, IEnumerable, IEnumerable&lt;T&gt;, ISerializable и IDeserializationCallback

1. **When is generic class generated?**

When a generic type or method is compiled into Microsoft intermediate language (MSIL), it contains metadata that identifies it as having type parameters. How the MSIL for a generic type is used differs based on whether the supplied type parameter is a value type or reference type.

When a generic type is first constructed with a value type as a parameter, the runtime creates a specialized generic type with the supplied parameter or parameters substituted in the appropriate locations in the MSIL. Specialized generic types are created one time for each unique value type that is used as a parameter.

Generics work somewhat differently for reference types. The first time a generic type is constructed with any reference type, the runtime creates a specialized generic type with object references substituted for the parameters in the MSIL. Then, every time that a constructed type is instantiated with a reference type as its parameter, regardless of what type it is, the runtime reuses the previously created specialized version of the generic type. This is possible because all references are the same size.

1. **Reflection**

**Рефлексия** представляет собой процесс выявления типов во время выполнения приложения. Каждое приложение содержит набор используемых классов, интерфейсов, а также их методов, свойств и прочих кирпичиков, из которых складывается приложение. И рефлексия как раз и позволяет определить все эти составные элементы приложения.

Основной функционал рефлексии сосредоточен в пространстве имен **System.Reflection**. В нем мы можем выделить следующие основные классы:

- **Assembly**: класс, представляющий сборку и позволяющий манипулировать этой сборкой
- **AssemblyName**: класс, хранящий информацию о сборке
- **MemberInfo**: базовый абстрактный класс, определяющий общий функционал для классов EventInfo, FieldInfo, MethodInfo и PropertyInfo
- **EventInfo**: класс, хранящий информацию о событии
- **FieldInfo**: хранит информацию об определенном поле типа
- **MethodInfo**: хранит информацию об определенном методе
- **PropertyInfo**: хранит информацию о свойстве
- **ConstructorInfo**: класс, представляющий конструктор
- **Module**: класс, позволяющий получить доступ к определенному модулю внутри сборки
- **ParameterInfo**: класс, хранящий информацию о параметре метода

Эти классы представляют составные блоки типа и приложения: методы, свойства и т.д. Но чтобы получить информацию о членах типа, нам надо воспользоваться классом **System.Type**.

Класс System.Type представляет изучаемый тип, инкапсулируя всю информацию о нем. С помощью его свойств и методов можно получить эту информацию. Некоторые из его свойств и методов:

- Метод **FindMembers()** возвращает массив объектов MemberInfo данного типа
- Метод **GetConstructors()** возвращает все конструкторы данного типа в виде набора объектов ConstructorInfo
- Метод **GetEvents()** возвращает все события данного типа в виде массива объектов EventInfo
- Метод **GetFields()** возвращает все поля данного типа в виде массива объектов FieldInfo
- Метод **GetInterfaces()** получает все реализуемые данным типом интерфейсы в виде массива объектов Type
- Метод **GetMembers()** возвращает все члены типа в виде массива объектов MemberInfo
- Метод **GetMethods()** получает все методы типа в виде массива объектов MethodInfo
- Метод **GetProperties()** получает все свойства в виде массива объектов PropertyInfo
- Свойство **Name** возвращает имя типа
- Свойство **Assembly** возвращает название сборки, где определен тип
- Свойство **Namespace** возвращает название пространства имен, где определен тип
- Свойство **IsArray** возвращает true, если тип является массивом
- Свойство **IsClass** возвращает true, если тип представляет класс
- Свойство **IsEnum** возвращает true, если тип является перечислением
- Свойство **IsInterface** возвращает true, если тип представляет интерфейс

Чтобы управлять типом и получать всю информацию о нем, нам надо сперва получить данный тип. Это можно сделать тремя способами: с помощью ключевого слова **typeof**, с помощью метода **GetType()** класса Object и применяя статический метод Type.GetType().

Difference GetType and typeof(): The only real difference here is that when you want to obtain the type from an instance of your class, you use GetType. If you don't have an instance, but you know the type name (and just need the actual System.Type to inspect or compare to), you would use typeof. Also: the call to GetType gets resolved at runtime, while typeof is resolved at compile time

1. **Indexers**

Indexers are a syntactic convenience that enable you to create a [class](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/class), [struct](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/struct), or [interface](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interface) that client applications can access as an array. The compiler will generate an Item property (or an alternatively named property if [IndexerNameAttribute](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.compilerservices.indexernameattribute) is present), and the appropriate accessor methods. Indexers are most frequently implemented in types whose primary purpose is to encapsulate an internal collection or array. To declare an indexer on a class or struct, use the [this](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/this) keyword.

The type of an indexer and the type of its parameters must be at least as accessible as the indexer itself. The signature of an indexer consists of the number and types of its formal parameters. It doesn't include the indexer type or the names of the formal parameters. If you declare more than one indexer in the same class, they must have different signatures.

An indexer value is not classified as a variable; therefore, you cannot pass an indexer value as a [ref](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/ref) or [out](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/out-parameter-modifier) parameter.

C# doesn't limit the indexer parameter type to integer. For example, it may be useful to use a string with an indexer. Such an indexer might be implemented by searching for the string in the collection, and returning the appropriate value. As accessors can be overloaded, the string and integer versions can coexist.

Be sure to incorporate some type of error-handling strategy to handle the chance of client code passing in an invalid index value. Set the accessibility of the [get](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/get) and [set](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/set) accessors to be as restrictive as is reasonable. This is important for the set accessor in particular.

public class TempRecord

{

// Array of temperature values

float\[\] temps = new float\[10\]

{

56.2F, 56.7F, 56.5F, 56.9F, 58.8F,

61.3F, 65.9F, 62.1F, 59.2F, 57.5F

};

// To enable client code to validate input

// when accessing your indexer.

public int Length => temps.Length;

// Indexer declaration.

// If index is out of range, the temps array will throw the exception.

public float this\[int index\]

{

get => temps\[index\];

set => temps\[index\] = value;

}

}

…

…

var tempRecord = new TempRecord();

// Use the indexer's set accessor

tempRecord\[3\] = 58.3F;

tempRecord\[5\] = 60.1F;

// Use the indexer's get accessor

for (int i = 0; i < 10; i++)

{

Console.WriteLine($"Element #{i} = {tempRecord\[i\]}");

}

// Keep the console window open in debug mode.

Console.WriteLine("Press any key to exit.");

Console.ReadKey();

1. **Immutable object. Advantages of i.o. Realization in .net.**

An immutable object is defined as an object that cannot be changed after it has been created. For many use cases, such as [Data Transfer Objects](https://www.infoworld.com/article/3562271/how-to-use-data-transfer-objects-in-aspnet-core-31.html), immutability is a desirable feature.

Declaring all fields readonly is a good step towards creating an immutable object, but this alone is not sufficient. This is because a readonly field can still be a reference to a mutable object.

In C# immutability is not enforced by the compiler.

To create immutable DTOs, you can take advantage of a ReadOnlyCollection or the thread-safe immutable collection types in the System.Collections.Immutable namespace. Alternatively, you could take advantage of record types in C# 9 to implement immutable DTOs.

A record type in C# 9 is a lightweight, immutable data type (or a lightweight class) that has read-only properties only. Since a record type is immutable, it is thread-safe and cannot mutate or change after it has been created.

Note the usage of the data keyword when declaring the record type. The data keyword when used in the declaration of a class marks the type as a record. You can take advantage of an instance of record type to pass data across the layers while at the same time ensuring immutability of the DTOs.

1. **Usage of StringBuilder. How StringBuilder works?**

Хотя класс System.String предоставляет нам широкую функциональность по работе со строками, все таки он имеет свои недостатки. Прежде всего, объект String представляет собой неизменяемую строку. Когда мы выполняем какой-нибудь метод класса String, система создает новый объект в памяти с выделением ему достаточного места. Удаление первого символа - не самая затратная операция. Однако когда подобных операций множество, а объем текста, для которого надо выполнить данные операции, также не самый маленький, то издержки при потере производительности становятся более существенными.

Чтобы выйти из этой ситуации во фреймворк .NET был добавлен новый класс **StringBuilder**, который находится в пространстве имен System.Text. Этот класс представляет динамическую строку.

StringBuilder sb = new StringBuilder("Привет мир");

Console.WriteLine($"Длина строки: {sb.Length}");

Console.WriteLine($"Емкость строки: {sb.Capacity}", );

Теперь переменная sb представляет начальную строку "Привет мир".

StringBuilder sb = new StringBuilder(20);

Если у нас заранее известен максимальный размер объекта, то мы можем таким образом сразу задать емкость и избежать последующих издержек при дополнительном выделении памяти.

StringBuilder sb = new StringBuilder("Название: ");

Console.WriteLine($"Длина строки: {sb.Length}"); // 10

Console.WriteLine($"Емкость строки: {sb.Capacity}"); // 16

sb.Append(" Руководство");

Console.WriteLine($"Длина строки: {sb.Length}"); // 22

Console.WriteLine($"Емкость строки: {sb.Capacity}"); // 32

sb.Append(" по C#");

Console.WriteLine($"Длина строки: {sb.Length}"); // 28

Console.WriteLine($"Емкость строки: {sb.Capacity}"); // 32

При создании объекта StringBuilder выделяется память по умолчанию для 16 символов, так как длина начальной строки меньше 16.

Дальше применяется метод Append - этот метод добавляет к строке подстроку. Так как при объединении строк их общая длина - 22 символа - превышает начальную емкость в 16 символов, то начальная емкость удваивается - до 32 символов.

Если бы итоговая длина строки была бы больше 32 символов, то емкость расширялась бы до размера длины строки.

Далее опять применяется метод Append, однако финальная длина уже будет 28 символов, что меньше 32 символов, и дополнительная память не будет выделяться.

Кроме метода Append класс StringBuilder предлагает еще ряд методов для операций над строками:

- **Insert**: вставляет подстроку в объект StringBuilder, начиная с определенного индекса
- **Remove**: удаляет определенное количество символов, начиная с определенного индекса
- **Replace**: заменяет все вхождения определенного символа или подстроки на другой символ или подстроку
- **AppendFormat**: добавляет подстроку в конец объекта StringBuilder

StringBuilder sb = new StringBuilder("Привет мир");

sb.Append("!");

sb.Insert(7, "компьютерный ");

Console.WriteLine(sb);

// заменяем слово

sb.Replace("мир", "world");

Console.WriteLine(sb);

// удаляем 13 символов, начиная с 7-го

sb.Remove(7, 13);

Console.WriteLine(sb);

// получаем строку из объекта StringBuilder

string s = sb.ToString();

Console.WriteLine(s);

Когда надо использовать класс String, а когда StringBulder?

Microsoft рекомендует использовать класс String в следующих случаях:

- При небольшом количестве операций и изменений над строками
- При выполнении фиксированного количества операций объединения. В этом случае компилятор может объединить все операции объединения в одну
- Когда надо выполнять масштабные операции поиска при построении строки, например IndexOf или StartsWith. Класс StringBuilder не имеет подобных методов.

Класс StringBuilder рекомендуется использовать в следующих случаях:

- При неизвестном количестве операций и изменений над строками во время выполнения программы
- Когда предполагается, что приложению придется сделать множество подобных операций

1. **Tree balancing.**
2. **Key-value structures**
3. **Hash function and Hash table. Properties of ideal Hash function.**

Класс Hashtable предназначен для создания коллекции, в которой для хранения ее элементов служит хеш-таблица. Информация сохраняется в хеш-таблице с помощью механизма, называемого хешированием. При хешировании для определения уникального значения, называемого хеш-кодом, используется информационное содержимое специального ключа. Полученный в итоге хеш-код служит в качестве индекса, по которому в таблице хранятся искомые данные, соответствующие заданному ключу. Преобразование ключа в хеш-код выполняется автоматически, и поэтому сам хеш-код вообще недоступен пользователю. Преимущество хеширования заключается в том, что оно обеспечивает постоянство времени выполнения операций поиска, извлечения и установки значений независимо от величины массивов данных.

В классе Hashtable реализуются интерфейсы IDictionary, ICollection, IEnumerable, ISerializable, IDeserializationCallback и ICloneable. В классе Hashtable определено немало конструкторов.

В классе Hashtable определяется ряд собственных методов, помимо тех, что уже объявлены в интерфейсах, которые в нем реализуются. Некоторые из наиболее часто используемых методов этого класса приведены ниже. В частности, для того чтобы определить, содержится ли ключ в коллекции типа Hashtable, вызывается метод ContainsKey(). А для того чтобы выяснить, хранится ли в такой коллекции конкретное значение, вызывается метод ContainsValue(). Для перечисления содержимого коллекции типа Hashtable служит метод GetEnumerator(), возвращающий объект типа IDictionaryEnumerator. Напомним, что IDictionaryEnumerator — это перечислитель, используемый для перечисления содержимого коллекции, в которой хранятся пары "ключ-значение".

ContainsKey()

Возвращает логическое значение true, если в вызывающей коллекции типа Hashtable содержится ключ, а иначе — логическое значение false

ContainsValue()

Возвращает логическое значение true, если в вызывающей коллекции типа Hashtable содержится значение, а иначе — логическое значение false

GetEnumerator()

Возвращает для вызывающей коллекции типа Hashtable перечислитель типа IDictionaryEnumerator

Synchronized()

Возвращает синхронизированный вариант коллекции типа Hashtable, передаваемой в качестве параметра

В классе Hashtable доступны также открытые свойства, определенные в тех интерфейсах, которые в нем реализуются. Особая роль принадлежит двум свойствам, Keys и Values, поскольку с их помощью можно получить ключи или значения из коллекции типа Hashtable.

В классе Hashtable не поддерживаются упорядоченные коллекции, и поэтому ключи или значения получаются из коллекции в произвольном порядке. Кроме того, в классе Hashtable имеется защищенное свойство EqualityComparer. А два других свойства, hep и comparer, считаются устаревшими.

Hashtable ht = new Hashtable(); // Создаем хеш-таблицу

ht.Add("alex85", "12345"); // Добавим несколько записей

ht.Add("fg230404", "12ldsd");

ht.Add("I_best_user", "12349");

ICollection keys = ht.Keys; // Считаем коллекцию ключей

foreach (string s in keys)

Console.WriteLine(s + ": " + ht\[s\]);

Основной принцип работы хеш-таблицы заключается в том, что в качестве входных параметров она принимает пары ключ-значение. Затем с помощью специальной хеш функции получает короткий ключ на основе полученного ключа. И наконец добавляет данные в таблицу. Если в таблице уже существует значения с таким хешем, то объединяет их в коллекции. Внутри коллекции поиск выполняется по изначальному полученному ключу.

**Хеш-функция** ([англ.](https://ru.wikipedia.org/wiki/%D0%90%D0%BD%D0%B3%D0%BB%D0%B8%D0%B9%D1%81%D0%BA%D0%B8%D0%B9_%D1%8F%D0%B7%D1%8B%D0%BA"%20\o%20"Английский%20язык) _hash function_ от _hash_ — «превращать в фарш», «мешанина»[<sup>\[1\]</sup>](https://ru.wikipedia.org/wiki/%D0%A5%D0%B5%D1%88-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D1%8F#cite_note-_ff142c473b3a285a-1)), или **функция свёртки** — функция, осуществляющая преобразование [массива](https://ru.wikipedia.org/wiki/%D0%9C%D0%B0%D1%81%D1%81%D0%B8%D0%B2_%28%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5%29"%20\o%20"Массив%20%28программирование%29) входных данных произвольной длины в (выходную) [битовую](https://ru.wikipedia.org/wiki/%D0%91%D0%B8%D1%82"%20\o%20"Бит) строку установленной длины, выполняемое [определённым алгоритмом](https://ru.wikipedia.org/wiki/%D0%94%D0%B5%D1%82%D0%B5%D1%80%D0%BC%D0%B8%D0%BD%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D1%8B%D0%B9_%D0%B0%D0%BB%D0%B3%D0%BE%D1%80%D0%B8%D1%82%D0%BC"%20\o%20"Детерминированный%20алгоритм). Преобразование, производимое хеш-функцией, называется **хешированием**. Исходные данные называются входным массивом, «**ключом**» или «**_сообщением_**». Результат преобразования (выходные данные) называется «**_[хешем](https://ru.wikipedia.org/wiki/%D0%A5%D0%B5%D1%88-%D1%81%D1%83%D0%BC%D0%BC%D0%B0"%20\o%20"Хеш-сумма)_**», «**_хеш-кодом_**», «**_хеш-суммой_**», «сводкой [сообщения](https://ru.wikipedia.org/wiki/%D0%A1%D0%BE%D0%BE%D0%B1%D1%89%D0%B5%D0%BD%D0%B8%D0%B5"%20\o%20"Сообщение)».

Хеш-функции применяются в следующих случаях:

- при построении [ассоциативных массивов](https://ru.wikipedia.org/wiki/%D0%90%D1%81%D1%81%D0%BE%D1%86%D0%B8%D0%B0%D1%82%D0%B8%D0%B2%D0%BD%D1%8B%D0%B9_%D0%BC%D0%B0%D1%81%D1%81%D0%B8%D0%B2"%20\o%20"Ассоциативный%20массив);
- при поиске дубликатов в сериях наборов данных;
- при построении уникальных идентификаторов для наборов данных;
- при вычислении [контрольных сумм](https://ru.wikipedia.org/wiki/%D0%9A%D0%BE%D0%BD%D1%82%D1%80%D0%BE%D0%BB%D1%8C%D0%BD%D0%B0%D1%8F_%D1%81%D1%83%D0%BC%D0%BC%D0%B0"%20\o%20"Контрольная%20сумма) от данных (сигнала) для последующего обнаружения в них ошибок (возникших случайно или внесённых намеренно), возникающих при хранении и/или передаче данных;
- при сохранении паролей _в системах защиты_ в виде хеш-кода (для восстановления пароля по хеш-коду требуется функция, являющаяся [обратной](https://ru.wikipedia.org/wiki/%D0%9E%D0%B1%D1%80%D0%B0%D1%82%D0%BD%D0%B0%D1%8F_%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D1%8F"%20\o%20"Обратная%20функция) по отношению к использованной хеш-функции);
- при выработке [электронной подписи](https://ru.wikipedia.org/wiki/%D0%AD%D0%BB%D0%B5%D0%BA%D1%82%D1%80%D0%BE%D0%BD%D0%BD%D0%B0%D1%8F_%D0%BF%D0%BE%D0%B4%D0%BF%D0%B8%D1%81%D1%8C"%20\o%20"Электронная%20подпись) (на практике часто подписывается не само сообщение, а его «хеш-образ»);
- и др.

В общем случае (согласно [принципу Дирихле](https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D0%B8%D0%BD%D1%86%D0%B8%D0%BF_%D0%94%D0%B8%D1%80%D0%B8%D1%85%D0%BB%D0%B5_%28%D0%BA%D0%BE%D0%BC%D0%B1%D0%B8%D0%BD%D0%B0%D1%82%D0%BE%D1%80%D0%B8%D0%BA%D0%B0%29)) нет однозначного соответствия между хеш-кодом (выходными данными) и исходными (входными) данными. Возвращаемые хеш-функцией значения (выходные данные) менее разнообразны, чем значения входного массива (входные данные). Случай, при котором хеш-функция преобразует более чем один массив входных данных в одинаковые сводки, называется «**[коллизией](https://ru.wikipedia.org/wiki/%D0%9A%D0%BE%D0%BB%D0%BB%D0%B8%D0%B7%D0%B8%D1%8F_%D1%85%D0%B5%D1%88-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8"%20\o%20"Коллизия%20хеш-функции)**». Вероятность возникновения коллизий используется для оценки качества хеш-функций.

1. **Collisions and how to combat?**

**Колли́зия хеш-фу́нкции** — два различных входных блока данных {\\displaystyle x} и {\\displaystyle y} для [хеш-функции](https://ru.wikipedia.org/wiki/%D0%A5%D0%B5%D1%88-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D1%8F"%20\o%20"Хеш-функция) {\\displaystyle H} таких, что {\\displaystyle H(x)=H(y).}

Коллизии существуют для большинства хеш-функций, но для «хороших» хеш-функций частота их возникновения близка к теоретическому минимуму. В некоторых частных случаях, когда множество различных входных данных [конечно](https://ru.wikipedia.org/wiki/%D0%9A%D0%BE%D0%BD%D0%B5%D1%87%D0%BD%D0%BE%D0%B5_%D0%BC%D0%BD%D0%BE%D0%B6%D0%B5%D1%81%D1%82%D0%B2%D0%BE"%20\o%20"Конечное%20множество), можно задать [инъективную](https://ru.wikipedia.org/wiki/%D0%98%D0%BD%D1%8A%D0%B5%D0%BA%D1%82%D0%B8%D0%B2%D0%BD%D0%BE%D1%81%D1%82%D1%8C"%20\o%20"Инъективность) хеш-функцию, по определению не имеющую коллизий. Однако для хеш-функций, принимающих вход переменной длины и возвращающих хеш постоянной длины (таких как [MD5](https://ru.wikipedia.org/wiki/MD5)), коллизии обязаны существовать, поскольку хотя бы для одного значения хеш-функции соответствующее ему множество входных данных ([полный прообраз](https://ru.wikipedia.org/wiki/%D0%A4%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D1%8F_%28%D0%BC%D0%B0%D1%82%D0%B5%D0%BC%D0%B0%D1%82%D0%B8%D0%BA%D0%B0%29"%20\l%20"%D0%A1%D0%B2%D1%8F%D0%B7%D0%B0%D0%BD%D0%BD%D1%8B%D0%B5_%D0%BE%D0%BF%D1%80%D0%B5%D0%B4%D0%B5%D0%BB%D0%B5%D0%BD%D0%B8%D1%8F"%20\o%20"Функция%20%28математика%29)) будет бесконечно — и любые два набора данных из этого множества образуют коллизию.

Коллизии осложняют использование [хеш-таблиц](https://ru.wikipedia.org/wiki/%D0%A5%D0%B5%D1%88-%D1%82%D0%B0%D0%B1%D0%BB%D0%B8%D1%86%D0%B0"%20\o%20"Хеш-таблица), так как нарушают однозначность соответствия между хеш-кодами и данными. Тем не менее, существуют специальные методики для преодоления возникающих сложностей:

- **Метод цепочек**: Технология сцепления элементов (chaining) состоит в том, что элементы [множества](https://ru.wikipedia.org/wiki/%D0%9C%D0%BD%D0%BE%D0%B6%D0%B5%D1%81%D1%82%D0%B2%D0%BE"%20\o%20"Множество), которым соответствует одно и то же хеш-значение, связываются в цепочку-список. В позиции номер i хранится указатель на голову списка тех элементов, у которых хеш-значение ключа равно i; если таких элементов в множестве нет, в позиции i записан [NULL](https://ru.wikipedia.org/wiki/NULL_%28%D0%A1%D0%B8%29).
- **Открытая адресация**: В отличие от хеширования с цепочками, при открытой адресации никаких списков нет, а все записи хранятся в самой хеш-таблице. Каждая ячейка таблицы содержит либо элемент динамического множества, либо NULL.

1. **CRUD operations in Dictionary&lt;K,V&gt;**
2. **Where are arrays stored? Arrays of primitive types?**
3. **Why use Enumerable, Observable, AsyncEnumerable and models they use?**