
### Use EsLint

[ESLint]((http://eslint.org/)) is a parser/analyzer with [numerous rules](http://eslint.org/docs/rules/) that keep us on the good path when writing JavaScript:

- It can flag the usage of unsafe and error-prone features such as hoisting, or implicit type conversions, sparse arrays or `eval`....
- It can enforce certain spacing, quote type, and line-breaking conventions depending on each team's preference
- It's highly configurable. For example: if we're developing an expression parser, we can knowingly disable the `no-eval` rule.
- It can be [integrated](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) [in](http://www.sublimelinter.com/en/latest/) IDEs to check our code as we write it
- Or [configured](https://github.com/MoOx/eslint-loader) to do it with every build either locally or on a CI server

### Take advantage of new language features
Accept that what constitutes good Javascript style is, for now, a moving target as the language is undergoing rapid evolution.
Take advantage of latest javascript features to simplify our code by using transpilers like [Babel](http://babeljs.io/) or [Typescript](https://www.typescriptlang.org/) whenever we can. For example:

- Use `let` and `const` instead of `var` (ESLint rule: [`no-var`](http://eslint.org/docs/rules/no-var))
```js
console.log(aVar); // undefined
console.log(aLet); // causes ReferenceError: aLet is not defined
var aVar = 1;
let aLet = 2;
```

- Use arrow functions `=>` instead of function expressions whenever possible since it's terser and its `this` semantic is more predictable

- Use [class expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/class) and [object literal](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer) to construct them in one go instead of multiple statements 

- Use array rest and spread operator `[...]` to simplify conversion to and from arrays as well as making modified copies.

```js
var parts = ['shoulders', 'knees']; 
var lyrics = ['head', ...parts, 'and', 'toes']; 
// ["head", "shoulders", "knees", "and", "toes"]
```

- When possible, use object rest and spread operator `{...}` to make modified copies of an object instead of mutating it

```js
const obj = {foo: 'a', bar: 'b'};
const obj2 = {...obj, foo: 1};
```

- Use `async/await` to manage promises whenever possible:

- Use `for...of` to iterate over arrays and iterables:

```js
let iterable = [3, 5, 7];
iterable.foo = 'hello';

for (let i in iterable) {
  console.log(i); // logs 0, 1, 2, "foo", "arrCustom", "objCustom"
}

for (let i of iterable) {
  console.log(i); // logs 3, 5, 7
}
```

### Modularize

Modularize our code with `import` and `export` using a bundler like [rollup](https://rollupjs.org/) or [webpack](https://webpack.js.org/)

### Write testable code

Prefer consise and side-effect-free functions over verbose and effectful ones.

### Understand the event loop

When working in a browser, knowing when your script is scheduled in relation to layout and repainting will helps avoid common bugs and performance issues:

- [Video](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
- [MDN entry](https://developer.mozilla.org/en/docs/Web/JavaScript/EventLoop/)
- [Article on MicroTasks](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)
