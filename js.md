**JS:**

1. **Difference between var, let, const**

One of the features that came with ES6 is the addition of let and const, which can be used for variable declaration.

**Var** Before the advent of ES6, var declarations ruled. There are issues associated with variables declared with var, though. That is why it was necessary for new ways to declare variables to emerge. First, let's get to understand var more before we discuss those issues.

**Scope of var Scope** essentially means where these variables are available for use. var declarations are globally scoped or function/locally scoped.

The scope is global when a var variable is declared outside a function. This means that any variable that is declared with var outside a function block is available for use in the whole window.

var is function scoped when it is declared within a function. This means that it is available and can be accessed only within that function. To understand further, look at the example below.

var greeter = "hey hi";

function newFunction() {

var hello = "hello";

}

Here, greeter is globally scoped because it exists outside a function while hello is function scoped. So we cannot access the variable hello outside of a function. So if we do this:

var tester = "hey hi";

function newFunction() {

var hello = "hello";

}

console.log(hello); // error: hello is not defined

We'll get an error which is as a result of hello not being available outside the function.

**var variables can be re-declared and updated** This means that we can do this within the same scope and won't get an error.

var greeter = "hey hi";

var greeter = "say Hello instead";

and this also

var greeter = "hey hi";

greeter = "say Hello instead";

**Hoisting of var**

Hoisting is a JavaScript mechanism where variables and function declarations are moved to the top of their scope before code execution. This means that if we do this:

console.log (greeter);

var greeter = "say hello"

it is interpreted as this:

var greeter;

console.log(greeter); // greeter is undefined

greeter = "say hello"

So var variables are hoisted to the top of their scope and initialized with a value of undefined.

**Problem with var** There's a weakness that comes with  var. I'll use the example below to explain:

var greeter = "hey hi";

var times = 4;

if (times > 3) {

var greeter = "say Hello instead";

}

console.log(greeter) // "say Hello instead"

So, since times > 3 returns true, greeter is redefined  to "say Hello instead". While this is not a problem if you knowingly want greeter to be redefined, it becomes a problem when you do not realize that a variable greeter has already been defined before.

If you have used greeter in other parts of your code, you might be surprised at the output you might get. This will likely cause a lot of bugs in your code. This is why let and const are necessary.

**Let** let is now preferred for variable declaration. It's no surprise as it comes as an improvement to var declarations. It also solves the problem with var that we just covered. Let's consider why this is so.

**let is block scoped** A block is a chunk of code bounded by {}. A block lives in curly braces. Anything within curly braces is a block.

So a variable declared in a block with let  is only available for use within that block. Let me explain this with an example:

let greeting = "say Hi";

let times = 4;

if (times > 3) {

let hello = "say Hello instead";

console.log(hello);// "say Hello instead"

}

console.log(hello) // hello is not defined

We see that using hello outside its block (the curly braces where it was defined) returns an error. This is because let variables are block scoped .

**let can be updated but not re-declared.** Just like var,  a variable declared with let can be updated within its scope. Unlike var, a let variable cannot be re-declared within its scope. So while this will work:

let greeting = "say Hi";

greeting = "say Hello instead";

this will return an error:

let greeting = "say Hi";

let greeting = "say Hello instead"; // error: Identifier 'greeting' has already been declared

However, if the same variable is defined in different scopes, there will be no error:

let greeting = "say Hi";

if (true) {

let greeting = "say Hello instead";

console.log(greeting); // "say Hello instead"

}

console.log(greeting); // "say Hi"

Why is there no error? This is because both instances are treated as different variables since they have different scopes.

This fact makes let a better choice than var. When using let, you don't have to bother if you have used a name for a variable before as a variable exists only within its scope.

Also, since a variable cannot be declared more than once within a scope, then the problem discussed earlier that occurs with var does not happen.

**Hoisting of let** Just like  var, let declarations are hoisted to the top. Unlike var which is initialized as undefined, the let keyword is not initialized. So if you try to use a let variable before declaration, you'll get a Reference Error.

**Const** Variables declared with the const maintain constant values. const declarations share some similarities with let declarations.

**const declarations are block scoped** Like let declarations, const declarations can only be accessed within the block they were declared.

**const cannot be updated or re-declared** This means that the value of a variable declared with const remains the same within its scope. It cannot be updated or re-declared. So if we declare a variable with const, we can neither do this:

const greeting = "say Hi";

greeting = "say Hello instead";// error: Assignment to constant variable.

nor this:

const greeting = "say Hi";

const greeting = "say Hello instead";// error: Identifier 'greeting' has already been declared

Every const declaration, therefore, must be initialized at the time of declaration.

This behavior is somehow different when it comes to objects declared with const. While a const object cannot be updated, the properties of this objects can be updated. Therefore, if we declare a const object as this:

const greeting = {

message: "say Hi",

times: 4

}

while we cannot do this:

greeting = {

words: "Hello",

number: "five"

} // error: Assignment to constant variable.

we can do this:

greeting.message = "say Hello instead";

This will update the value of greeting.message without returning errors.

**Hoisting of const** Just like let, const declarations are hoisted to the top but are not initialized.

So just in case you missed the differences, here they are:

- var declarations are globally scoped or function scoped while let and const are block scoped.
- var variables can be updated and re-declared within its scope; let variables can be updated but not re-declared; const variables can neither be updated nor re-declared.
- They are all hoisted to the top of their scope. But while var variables are initialized with undefined, let and const variables are not initialized.
- While var and let can be declared without being initialized, const must be initialized during declaration.

1. **How hoisting works**

Поднятие или hoisting — это механизм в JavaScript, в котором переменные и объявления функций, передвигаются вверх своей области видимости перед тем, как код будет выполнен.

Как следствие, это означает то, что совершенно неважно где были объявлены функция или переменные, все они передвигаются вверх своей области видимости, вне зависимости от того локальная она или же глобальная.

Стоит отметить то, что механизм “поднятия” передвигает только объявления функции или переменной. Назначения переменным остаются на своих местах.

Variables defined with let and const are hoisted to the top of the block, but not _initialized_.

Meaning: The block of code is aware of the variable, but it cannot be used until it has been declared.

Using a let variable before it is declared will result in a ReferenceError.

The variable is in a "temporal dead zone" from the start of the block until it is declared:

Using a const variable before it is declared, is a syntax errror, so the code will simply not run.

JavaScript only hoists declarations, not initializations.

To avoid bugs, always declare all variables at the beginning of every scope.

1. **Benefit of Immediately Invoked Function Expression**

Sometimes you need to define and call function at the same time and only once so in this case anonymous function helps you. In such situations giving functions a name and then calling them is just excess.

Further sometimes you wants to create a namespace for your variables. So anonymous functions helps you there too. For example

(function($) {

$.fn.pluginName = function(opt) {

// implementation goes here...

}

}(jQuery));

In above case you can safely use $ as jQuery synonym in your code.

If you define a function with name as shown below, then it will create global variable with function name as you defined.

function myFunction() {

// function code goes here.

}

myFunction();

But if you define it without name then it won't create any global variable and your global namespace will not be polluted.

(function myFunction() {

// function code goes here.

}());

Function with names are useful only when you need to call them from different places in your code.

1. **Array method .reduce() how works**

Метод **reduce()** виконує функцію **reducer** (функцію вказуєте ви) для кожного елемента масиву та повертає єдине значення.

const array1 = \[1, 2, 3, 4\];

const reducer = (accumulator, currentValue) => accumulator + currentValue;

// 1 + 2 + 3 + 4

console.log(array1.reduce(reducer));

// expected output: 10

// 5 + 1 + 2 + 3 + 4

console.log(array1.reduce(reducer, 5)); // expected output: 15

Функія **reducer** отримує чотири параметри:

- Accumulator (_acc_) - Акумулятор
- Current Value (_cur_) - Поточне значення
- Current Index (_idx_) - Поточний індекс
- Source Array (_src_) - Вхідний масив

Функція **reducer** повертає значення та присовює його акумулятору. Значення аккумулятора запам'ятовується через усі ітерації і повертається наприкінці як єдиний результат.

1. **Arrow function vs function**

Here is a function written in ES5 syntax:

function timesTwo(params) { return params \* 2}function timesTwo(params) {

return params \* 2

}

timesTwo(4); // 8

Now, here is the same function expressed as an arrow function:

var timesTwo = params => params \* 2

timesTwo(4); // 8

It is important to note that arrow functions are anonymous, which means that they are not named.

This anonymity creates some issues:

**Harder to debug**

When you get an error, you will not be able to trace the name of the function or the exact line number where it occurred.

**No self-referencing**

If your function needs to have a self-reference at any point (e.g. recursion, event handler that needs to unbind), it will not work.

In classic function expressions, the this keyword is bound to different values based on the _context_ in which it is called. With arrow functions however, this is _lexically bound_. It means that it usesthis from the code that contains the arrow function.

For example, look at the setTimeout function below:

// ES5

var obj = {

id: 42,

counter: function counter() {

setTimeout(function() {

console.log(this.id);

}.bind(this), 1000);

}

};

In the ES5 example, .bind(this) is required to help pass the this context into the function. Otherwise, by default this would be undefined.

// ES6

var obj = {

id: 42,

counter: function counter() {

setTimeout(() => {

console.log(this.id);

}, 1000);

}

};

ES6 arrow functions can’t be bound to a this keyword, so it will lexically go up a scope, and use the value of this in the scope in which it was defined.

**When you should not use Arrow Functions**

They do not replace regular functions. Here are some instances where you probably wouldn’t want to use them:

**Object methods**

When you call cat.jumps, the number of lives does not decrease. It is because this is not bound to anything, and will inherit the value of this from its parent scope.

var cat = {

lives: 9,

jumps: () => {

this.lives--;

}

}

**Callback functions with dynamic context**

If you need your context to be dynamic, arrow functions are not the right choice. Take a look at this event handler below:

var button = document.getElementById('press');

button.addEventListener('click', () => {

this.classList.toggle('on');

});

If we click the button, we would get a TypeError. It is because this is not bound to the button, but instead bound to its parent scope.

**When it makes your code less readable**

It is worth taking into consideration the variety of syntax we covered earlier. With regular functions, people know what to expect. With arrow functions, it may be hard to decipher what you are looking at straightaway.

**When you should use them**

Arrow functions shine best with anything that requires this to be bound to the context, and not the function itself.

Despite the fact that they are anonymous, I also like using them with methods such as map and reduce, because I think it makes my code more readable. To me, the pros outweigh the cons.

1. **.call(), .apply(), .bind() what is it**

Use .bind() when you want that function to later be called with a certain context, useful in events. Use .call() or .apply() when you want to invoke the function immediately, and modify the context.

Call/apply call the function immediately, whereas bind returns a function that, when later executed, will have the correct context set for calling the original function. This way you can maintain context in async callbacks and events.

ECMAScript 5 introduced [Function.prototype.bind()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind). Calling f.bind(someObject) creates a new function with the same body and scope as f, but where this occurs in the original function, in the new function it is permanently bound to the first argument of bind, regardless of how the function is being used.

function f() {

return this.a;

}

var g = f.bind({a: 'azerty'});

console.log(g()); // azerty

var h = g.bind({a: 'yoo'}); // bind only works once!

console.log(h()); // azerty

var o = {a: 37, f: f, g: g, h: h};

console.log(o.a, o.f(), o.g(), o.h()); // 37,37, azerty, azerty

Note that in non–strict mode, with call and apply, if the value passed as this is not an object, an attempt will be made to convert it to an object. Values null and undefined become the global object. Primitives like 7 or 'foo' will be converted to an Object using the related constructor, so the primitive number 7 is converted to an object as if by new Number(7) and the string 'foo' to an object as if by new String('foo'), e.g.

function add(c, d) {

return this.a + this.b + c + d;

}

var o = {a: 1, b: 3};

// The first parameter is the object to use as

// 'this', subsequent parameters are passed as

// arguments in the function call

add.call(o, 5, 7); // 16

// The first parameter is the object to use as

// 'this', the second is an array whose

// members are used as the arguments in the function call

add.apply(o, \[10, 20\]); // 34

1. **Event bubbling vs Event capturing**

Event bubbling and capturing are two ways of event propagation in the HTML DOM API, when an event occurs in an element inside another element, and both elements have registered a handle for that event. The event propagation mode determines in [which order the elements receive the event](http://www.quirksmode.org/js/events_order.html).

With bubbling, the event is first captured and handled by the innermost element and then propagated to outer elements.

With capturing, the event is first captured by the outermost element and propagated to the inner elements.

Capturing is also called "trickling", which helps remember the propagation order:

trickle down, bubble up

Back in the old days, Netscape advocated event capturing, while Microsoft promoted event bubbling. Both are part of the W3C [Document Object Model Events](http://www.w3.org/TR/DOM-Level-2-Events/events.html) standard (2000).

IE < 9 uses [only event bubbling](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget.addEventListener), whereas IE9+ and all major browsers support both. On the other hand, the [performance of event bubbling may be slightly lower](https://stackoverflow.com/a/10335117/1269037) for complex DOMs.

We can use the addEventListener(type, listener, useCapture) to register event handlers for in either bubbling (default) or capturing mode. To use the capturing model pass the third argument as true.

1. **Promise states**

A promise is an object that may produce a single value some time in the future: either a resolved value, or a reason that it’s not resolved (e.g., a network error occurred). A promise may be in one of 3 possible states: fulfilled, rejected, or pending. Promise users can attach callbacks to handle the fulfilled value or the reason for rejection.

Promises are eager, meaning that a promise will start doing whatever task you give it as soon as the promise constructor is invoked. If you need lazy, check out observables or tasks.

A promise is an object which can be returned synchronously from an asynchronous function. It will be in one of 3 possible states:

- Fulfilled: onFulfilled() will be called (e.g., resolve() was called)
- Rejected: onRejected() will be called (e.g., reject() was called)
- Pending: not yet fulfilled or rejected

A promise is settled if it’s not pending (it has been resolved or rejected). Sometimes people use resolved and settled to mean the same thing: not pending.

Once settled, a promise can not be resettled. Calling resolve() or reject() again will have no effect. The immutability of a settled promise is an important feature.

Native JavaScript promises don’t expose promise states. Instead, you’re expected to treat the promise as a black box. Only the function responsible for creating the promise will have knowledge of the promise status, or access to resolve or reject.

A pending promise can either be _fulfilled_ with a value or _rejected_ with a reason (error). When either of these options happens, the associated handlers queued up by a promise's then method are called. If the promise has already been fulfilled or rejected when a corresponding handler is attached, the handler will be called, so there is no race condition between an asynchronous operation completing and its handlers being attached.

As the [Promise.prototype.then()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then) and [Promise.prototype.catch()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) methods return promises, they can be chained.

1. **Event loop (micro and macro tasks)**

Поток выполнения в браузере, равно как и в Node.js, основан на событийном цикле.

Идея _событийного цикла_ очень проста. Есть бесконечный цикл, в котором движок JavaScript ожидает задачи, исполняет их и снова ожидает появления новых.

Общий алгоритм движка:

- Пока есть задачи: выполнить их, начиная с самой старой
- Бездействовать до появления новой задачи, а затем перейти к пункту 1

Задачи поступают на выполнение – движок выполняет их – затем ожидает новые задачи (во время ожидания практически не нагружая процессор компьютера)

Может так случиться, что задача поступает, когда движок занят чем-то другим, тогда она ставится в очередь.

Очередь, которую формируют такие задачи, называют «очередью макрозадач»

- Рендеринг (отрисовка страницы) никогда не происходит во время выполнения задачи движком. Не имеет значения, сколь долго выполняется задача. Изменения в DOM отрисовываются только после того, как задача выполнена.
- Если задача выполняется очень долго, то браузер не может выполнять другие задачи, обрабатывать пользовательские события, поэтому спустя некоторое время браузер предлагает «убить» долго выполняющуюся задачу. Такое возможно, когда в скрипте много сложных вычислений или ошибка, ведущая к бесконечному циклу.

Помимо макрозадач, описанных в этой части, существуют микрозадачи

Микрозадачи приходят только из кода. Обычно они создаются промисами: выполнение обработчика .then/catch/finally становится микрозадачей. Микрозадачи также используются «под капотом» await, т.к. это форма обработки промиса.

Также есть специальная функция queueMicrotask(func), которая помещает func в очередь микрозадач.

**Сразу после каждой макрозадачи движок исполняет все задачи из очереди микрозадач перед тем, как выполнить следующую макрозадачу или отобразить изменения на странице, или сделать что-то ещё.**

**Все микрозадачи завершаются до обработки каких-либо событий или рендеринга, или перехода к другой макрозадаче.**

setTimeout(() => alert("timeout"));

Promise.resolve()

.then(() => alert("promise"));

alert("code");

- code появляется первым, т.к. это обычный синхронный вызов.
- promise появляется вторым, потому что .then проходит через очередь микрозадач и выполняется после текущего синхронного кода.
- timeout появляется последним, потому что это макрозадача.

Более подробный алгоритм событийного цикла (хоть и упрощённый в сравнении со спецификацией):

Выбрать и исполнить старейшую задачу из очереди макрозадач (например, «script»).

Исполнить все микрозадачи:

Пока очередь микрозадач не пуста: - Выбрать из очереди и исполнить старейшую микрозадачу

Отрисовать изменения страницы, если они есть.

Если очередь макрозадач пуста – подождать, пока появится макрозадача.

Перейти к шагу 1.

Чтобы добавить в очередь новую макрозадачу:

Используйте setTimeout(f) с нулевой задержкой.

Этот способ можно использовать для разбиения больших вычислительных задач на части, чтобы браузер мог реагировать на пользовательские события и показывать прогресс выполнения этих частей.

Также это используется в обработчиках событий для отложенного выполнения действия после того, как событие полностью обработано (всплытие завершено).

Для добавления в очередь новой микрозадачи:

Используйте queueMicrotask(f).

Также обработчики промисов выполняются в рамках очереди микрозадач.

События пользовательского интерфейса и сетевые события в промежутках между микрозадачами не обрабатываются: микрозадачи исполняются непрерывно одна за другой.

Поэтому queueMicrotask можно использовать для асинхронного выполнения функции в том же состоянии окружения.

Docs to read:

<https://developer.mozilla.org/en-US/docs/Web/JavaScript>

<https://javascript.info/>

**React:**

1. **React lifecycles for Mounting, Updating, Unmounting**

While React.js is being updated regularly, the core functionality follows a lifecycle route.

React.js components rise and give birth to others in a loop format. This makes the entire process that much more efficient and lean.

Ideally, the lifecycle of React.js is split into four core stages

![](data:image/png;base64,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)

This is the first stage in the React.js lifecycle and is essential to understand and explore thoroughly. This is the stage where the component is constructed with the provided properties and a default state.

It’s done in the constructor of a component class. The developer has to define the ideal props (properties) and the initial state of the component.

It’s important to understand how the components are initialized when you populate their state or properties. This can help them render the right information, outside of their skeleton markup. You can also do so early on in the ideation phase to streamline your coding approach.

There are mainly three pieces of data that the JSX depends on, which are listed below:

- error — This is the standard message that is displayed when there is a bug in the system, or a rendering has a mistake in it.
- loading — Loading is when the application is fetching the API data.
- users — The data that is retrieved from the API is critical as well.

This helps in initiating a successful coding architecture within the greater user interface module.

While the component is setting up the initial state in the constructor, you can change it later using the setState method. This gives you more flexibility in coding as you move on towards the other end of the lifecycle.

The defaultProps is also defined as the property of the component so that you can identify the default value of the props. You can override this later as well with new prop values.

You can then lay out default values that the app can leverage as a default state. This default state is the one that can be used multiple times over several applications.

It’s important to code this part out correctly as it sets the stage for the next layers in the lifecycle.

# **Mounting**

This is the next stage in the lifecycle and a critical one for launch. After you have prepared the code with basic requirements, states and props, you need your component to mount in the browser.

This is done via browser DOM and the phase gives you the right React.js Hooks methods for a before and after fitting.

Here are the critical terms you should be adept in:

## ****render****

Render is what mounts the component onto the browser in this state. It is a classic method that gives the same output every time the same input is provided.

It’s a standard function that is used extensively in the React.js coding framework.

## ****componentWillMount****

This is a critical function to remember, as it is executed just before the reach component is about to mount. The mounting on the DOM is done after this stage, wherein you can enter all the things that you want to the program to do.

It is also executed once in a lifecycle of a component and occurs before you render the program for the first time. It is used for initializing the states or props as well, making it a robust component to leverage.

## ****componentDidMount****

This is the final React.js Hook method that is executed after the component mounts the DOM. It’s performed once in the lifecycle and occurs after the first rendering.

Engineers can access the DOM via this method and initialize the appropriate JS libraries. You can access the DOM efficiently using this component.

You can also initialize several other libraries that can be incorporated into the final output. You can make the right API calls under this method so that you can retrieve the data the right way.

# **Updating**

The third stage starts when the component has been adopted on the browser. This can then grow by receiving new updates from the program. The user can interact with the program and the component can be updated accordingly.

Developers can typically update the component in a few main ways. They can either send new props to the command or update the state entirely. Depending on the complexity or the scale of work, they can choose either method and get the program running.

Here are Hook methods that are critical to understanding:

## ****shouldComponentUpdate****

The method tells the program about the state of rendering when it is updated.

If new props or rules are being updated, a rendering can be done or skipped. This is important to code in properly, as there are evolving states in the program as well.

Updating the method as true/false is the proper approach. The default here is true, which can be changed as per the code.

## ****componentWillUpdate****

This is executed when the prior method returns the answer of true. It’s then used to prepare the upcoming render, in the case where some previous calculation is necessary before returning a response.

For more complex programs, this method can be used as well.

## ****componentDidUpdate****

This is then executed when the updated component has been updated in the DOM as well. You can initiate new libraries to reload so that you can maintain an updated program throughout the process.

Rendering can be triggered accordingly, as per the core requirement.

# **Unmounting**

The final stage of unmounting is essential as it doesn’t require the component and gets unmounted from the DOM. As the final state, it is designed to produce the outcome via unmounting.

Here is the essential method used in unmounting:

## ****componentWillUnmount****

This is the last method in the lifecycle as it pertains to the core unmounting and removal from the DOM. The cleaning up of the component is also performed here.

This is used in the logging out of users when they want to clear out the program from their browser.

1. **forceUpdate method to rerender component**

Calling forceUpdate() will cause render() to be called on the component and skip shouldComponentUpdate().

The shouldComponentUpdate() method allows your Component to exit the _Update life cycle_ if there is no reason to apply a new render.

This will trigger the normal lifecycle methods for child components, including the shouldComponentUpdate() method of each child. React will still only update the DOM if the markup changes.

1. **Attribute 'key' for iterations. Why it's so important to have it**

You should add a key to each child **as well as each element inside children**.

This way React can handle the minimal DOM change.

Try removing the key={i} from the &lt;b&gt;&lt;/b&gt; element inside the div's (and check the console).

In the sample, if we don't give a key to the &lt;b&gt; element and we want to update **only** the object.city, React needs to re-render the whole row vs just the element.

1. **How VirtualDOM works in React**

First things first, DOM stands for “Document Object Model”. The DOM in simple words represents the UI of your application. Everytime there is a change in the state of your application UI, the DOM gets updated to represent that change. Now the catch is frequently manipulating the DOM affects performance, making it slow.

The DOM is represented as a tree data structure. Because of that, the changes and updates to the DOM are fast. But after the change, the updated element and it’s children have to be re-rendered to update the application UI. The re-rendering or re-painting of the UI is what makes it slow. Therefore, the more UI components you have, the more expensive the DOM updates could be, since they would need to be re-rendered for every DOM update.

That’s where the concept of virtual DOM comes in and performs significantly better than the real DOM. The virtual DOM is only a virtual representation of the DOM. Everytime the state of our application changes, the virtual DOM gets updated instead of the real DOM.

When new elements are added to the UI, a virtual DOM, which is represented as a tree is created. Each element is a node on this tree. If the state of any of these elements changes, a new virtual DOM tree is created. This tree is then compared or “diffed” with the previous virtual DOM tree.

Once this is done, the virtual DOM calculates the best possible method to make these changes to the real DOM. This ensures that there are minimal operations on the real DOM. Hence, reducing the performance cost of updating the real DOM.

In React every UI piece is a component, and each component has a state. React follows the observable pattern and listens for state changes. When the state of a component changes, React updates the virtual DOM tree. Once the virtual DOM has been updated, React then compares the current version of the virtual DOM with the previous version of the virtual DOM. This process is called “diffing”.

Once React knows which virtual DOM objects have changed, then React updates **only** those objects, in the real DOM. This makes the performance far better when compared to manipulating the real DOM directly. This makes React standout as a high performance JavaScript library.

React follows a batch update mechanism to update the real DOM. Hence, leading to increased performance. This means that updates to the real DOM are sent in batches, instead of sending updates for every single change in state.

The repainting of the UI is the most expensive part, and React efficiently ensures that the real DOM receives only batched updates to repaint the UI.

React doc - <https://reactjs.org/docs/getting-started.html>

(all info is included)

**Web:**

1. **Cookie structure?** [**https://flaviocopes.com/cookies/**](https://flaviocopes.com/cookies/)

A browser cookie is a small text file that a web server sends to your web browser when you request a page from a site. This text file is no larger than 4k and stores specific data elements. There is no programming code in these files. You can see a site’s cookies from within most browsers.

Each cookie has values for seven fields:

- **Name**
- **Content or Value**
- **Domain**
- **Path** (the “/” means the cookie is valid anywhere on that domain.)
- **Send fo**r (the connection security level)
- **Created**
- **Expires**

Why used:

- personalize a site
- authenticate you to a site requiring a username
- maintain shopping cart status
- track affiliate leads
- analyze web traffic

1. **Sessionstorage vs localstorage**

Объекты веб-хранилища localStorage и sessionStorage позволяют хранить пары ключ/значение в браузере.

Что в них важно – данные, которые в них записаны, сохраняются после обновления страницы (в случае sessionStorage) и даже после перезапуска браузера (при использовании localStorage).

Но ведь у нас уже есть куки. Зачем тогда эти объекты?

- В отличие от куки, объекты веб-хранилища не отправляются на сервер при каждом запросе. Поэтому мы можем хранить гораздо больше данных. Большинство браузеров могут сохранить как минимум 2 мегабайта данных (или больше), и этот размер можно поменять в настройках.
- Ещё одно отличие от куки – сервер не может манипулировать объектами хранилища через HTTP-заголовки. Всё делается при помощи JavaScript.
- Хранилище привязано к источнику (домен/протокол/порт). Это значит, что разные протоколы или поддомены определяют разные объекты хранилища, и они не могут получить доступ к данным друг друга.

Объекты хранилища localStorage и sessionStorage предоставляют одинаковые методы и свойства:

- setItem(key, value) – сохранить пару ключ/значение.
- getItem(key) – получить данные по ключу key.
- removeItem(key) – удалить данные с ключом key.
- clear() – удалить всё.
- key(index) – получить ключ на заданной позиции.
- length – количество элементов в хранилище.

Основные особенности localStorage:

- Этот объект один на все вкладки и окна в рамках источника (один и тот же домен/протокол/порт).
- Данные не имеют срока давности, по которому истекают и удаляются. Сохраняются после перезапуска браузера и даже ОС.

Нам достаточно находиться на том же источнике (домен/протокол/порт), при этом URL-путь может быть разным.

Объект localStorage доступен всем окнам из одного источника, поэтому, если мы устанавливаем данные в одном окне, изменения становятся видимыми в другом.

localStorage.setItem('test', 1); // then close browser and restart even OC

alert(localStorage.getItem('test')); //1

Также можно получать/записывать данные, как в обычный объект:

localStorage.test = 2;

Это возможно по историческим причинам и, как правило, работает, но обычно не рекомендуется, потому что:

1. Если ключ генерируется пользователем, то он может быть каким угодно, включая length или toString или другой встроенный метод localStorage. В этом случае getItem/setItem сработают нормально, а вот чтение/запись как свойства объекта не пройдут:
2. Когда мы модифицируем данные, то срабатывает событие storage. Но это событие не происходит при записи без setItem, как свойства объекта.

Обратите внимание, что ключ и значение должны быть строками.

Если мы используем любой другой тип, например число или объект, то он автоматически преобразуется в строку:

Объект sessionStorage используется гораздо реже, чем localStorage.

Свойства и методы такие же, но есть существенные ограничения:

- sessionStorage существует только в рамках текущей вкладки браузера.
  - Другая вкладка с той же страницей будет иметь другое хранилище.
  - Но оно разделяется между ифреймами на той же вкладке (при условии, что они из одного и того же источника).
- Данные продолжают существовать после перезагрузки страницы, но не после закрытия/открытия вкладки.

Так получилось, потому что sessionStorage привязан не только к источнику, но и к вкладке браузера. Поэтому sessionStorage используется нечасто.

Когда обновляются данные в localStorage или sessionStorage, генерируется событие [storage](https://www.w3.org/TR/webstorage/#the-storage-event) со следующими свойствами:

- key – ключ, который обновился (null, если вызван .clear()).
- oldValue – старое значение (null, если ключ добавлен впервые).
- newValue – новое значение (null, если ключ был удалён).
- url – url документа, где произошло обновление.
- storageArea – объект localStorage или sessionStorage, где произошло обновление.

Важно: событие срабатывает на всех остальных объектах window, где доступно хранилище, кроме того окна, которое его вызвало.

Теперь, если оба окна слушают window.onstorage, то каждое из них будет реагировать на обновления, произошедшие в другом окне.

1. **CORS**

Cross-Origin Resource Sharing ([CORS](https://developer.mozilla.org/en-US/docs/Glossary/CORS), перехресний обмін ресурсами) — це механізм, що за допомогою [HTTP](https://developer.mozilla.org/uk/docs/Glossary/HTTP)\-заголовків дає [переглядачеві](https://developer.mozilla.org/uk/docs/Glossary/Browser) дозвіл завантажувати ресурси з певного джерела на запит web-застосунка, отриманого з відмінного джерела. Web-застосунок виконує **перехресний HTTP-запит** коли потребує ресурсів з джерела (домен, протокол чи порт), відмінного від його власного.

Приклад перехресного запиту: JavaScript-код web-застосунка, розміщеного на <http://domain-a.com>, намагається за допомогою [AJAX](https://developer.mozilla.org/uk/docs/Web/API/XMLHttpRequest)\-запиту отримати <http://api.domain-b.com/data.json>.

З міркувань безпеки, web-переглядачі припиняють всі перехресні HTTP-запити, здійснені кодом скриптів. Наприклад, XMLHttpRequest і [Fetch API](https://developer.mozilla.org/uk/docs/Web/API/Fetch_API) дотримуються [правила одного джерела](https://developer.mozilla.org/uk/docs/Web/Security/Same-origin_policy). Це означає, що web-застосунок, отриманий з певного джерела, не може виконувати запити до HTTP-ресурсів з відмінного джерела

«источник (origin)» в этом случае состоит из

- протокол (например http)
- хост (например example.com)
- порт (например 8000)

Так что **<http://sample.org>** и **<http://www.sample.org>** и **<http://sample.org:3000>** — это три разных источника.

[Цей стандарт](https://fetch.spec.whatwg.org/#http-cors-protocol) покликаний дозволити міжсайтові HTTP-запити для:

- Міжсайтового користування [XMLHttpRequest](https://developer.mozilla.org/uk/docs/Web/API/XMLHttpRequest) та [Fetch](https://developer.mozilla.org/uk/docs/Web/API/Fetch_API) API, як зазначено вище;
- Web-шрифтів (використання шрифтів з інших джерел за допомогою правила @font-face в CSS), тож [сервер, який розміщує TrueType-шрифти, надає обмежений доступ](https://www.w3.org/TR/css-fonts-3/#font-fetching-requirements) на їх завантаження лише певній множині сайтів;
- [Текстури WebGL](https://developer.mozilla.org/uk/docs/Web/API/WebGL_API/Tutorial/Using_textures_in_WebGL);
- Зображення чи кадри відео, намальовані на холсті за допомогою [drawImage()](https://developer.mozilla.org/uk/docs/Web/API/CanvasRenderingContext2D/drawImage).

Допустим нам нужно разрешить работу JavaScript на сторонних сайтах (например, 127.0.0.1:8000) что бы получать доступ к нашим ответам API. Для этого нам нужно включить CORS в заголовок ответа от сервера. Это делается на стороне сервера:

app.get('/public', function(req, res) {

res.set('Access-Control-Allow-Origin', '\*')

res.send(...)

})

Здесь мы устанавливаем заголовку Access-Control-Allow-Origin значение \*, что означает: что любому хосту разрешен доступ к этому URL и ответу в браузере

Это требует настройки на стороне сервера и на стороне клиента и в зависимости от запроса вызовет предварительный (preflight) запрос.

1. **This in JS**

Поведение ключевого слова this в JavaScript несколько отличается по сравнению с остальными языками. Имеются также различия при использовании this в строгом и нестрогом режиме. В большинстве случаев значение this определяется тем, каким образом вызвана функция. Значение this не может быть установлено путем присваивания во время исполнения кода и может иметь разное значение при каждом вызове функции. В ES5 представлен метод bind(), который используется для привязки значения ключевого слова this независимо от того, как вызвана функция. Также в ES2015 представлены стрелочные функции, которые не создают собственные привязки к this (они сохраняют значение this лексического окружения, в котором были созданы).

В глобальном контексте выполнения (за пределами каких-либо функций) this ссылается на глобальный объект вне зависимости от режима (строгий или нестрогий).

Вы всегда можете легко получить объект global, используя глобальное свойство globalThis, независимо от текущего контекста, в котором выполняется ваш код.

В пределах функции значение this зависит от того, каким образом вызвана функция.

Когда this используется внутри объявленного объекта, значение this выставляется из ближайшего родительского объекта метода, который вызывается.

Когда функция вызывается как метод объекта, используемое в этой функции ключевое слово this принимает значение объекта, по отношению к которому вызван метод.

var o = {

prop: 37,

f: function() {

return this.prop;

}

};

console.log(o.f()); // logs 37

var o = {prop: 37};

function independent() {

return this.prop;

}

o.f = independent;

console.log(o.f()); // logs 37

В [стрелочных функциях](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Functions/Arrow_functions), this привязан к окружению, в котором была создана функция. В глобальной области видимости this будет указывать на глобальный объект

var o = {prop: 37};

var foo = (() => this.prop);

o.f = foo;

console.log(o.f()); // undefined

При использовании new, this привязывает к новому созданному объекту.