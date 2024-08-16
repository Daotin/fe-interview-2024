# [typeof和instanceof的区别](https://github.com/Daotin/fe-interview-2024/issues/5)

`typeof` 和 `instanceof` 都是 JavaScript 中用于类型检查的操作符，但它们的使用场景和功能有所不同。

`typeof` 用于检测一个变量的数据类型。它返回一个字符串，表示变量的类型，比如 `"string"`、`"number"`、`"boolean"`、`"undefined"`、`"object"`、`"function"`、以及 ES6 引入的 `"symbol"`。`typeof` 适用于基本数据类型的判断，尤其是当你不确定变量是否已定义或是否为基本类型时。此外，对于引用类型的对象，`typeof` 通常返回 `"object"`，但需要注意的是，对 `null` 执行 `typeof` 也会返回 `"object"`，这是一种历史遗留的设计问题。

> [!warning]
> 其中数组、对象、null都会被判断为object，其他判断都正确。

相比之下，`instanceof` 用于检测一个对象是否为某个构造函数的实例。它通过检查对象的原型链来判断，返回一个布尔值。`instanceof` 主要应用于判断复杂类型，比如自定义类实例或数组等。它能够更准确地识别出对象的具体类型，尤其是在需要区分对象的继承关系或具体类型时。

总结来说，`typeof` 适合用于判断基本数据类型，而 `instanceof` 则适用于检测对象是否属于特定的类或构造函数。两者结合使用，可以在不同场景下进行灵活的类型检查。