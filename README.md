# `+>` for function composition

## Summary

A proposed function composition operator that allows terse and intuitive composition, in execution order, from and to any of the 4 available function types: `Function`, `AsyncFunction`, `GeneratorFunction` and `AsyncGeneratorFunction`.

## Usage

The statement:

```javascript
const doubleThenSquareThenHalf = value=>half(square(double(value)))
```

is rewritable as:

```javascript
const doubleThenSquareThenHalf = double +> square +> half
```

Introducing an `AsyncFunction` produces an `AsyncFunction`:

```javascript
const doubleThenSquareThenHalfAsync = double +> squareAsync +> half
```

Introducting a `GeneratorFunction` produces a `GeneratorFunction` that pipes each yielded value to subsequent functions:

```javascript
const randomBetween0And100Generator = randomBetween0And1Generator +> multiplyBy100
```

Introducing an `AsyncFunction` and a `GeneratorFunction`, and/or an `AsyncGeneratorFunction`, produces an `AsyncGeneratorFunction`:

```javascript
const nextRouteAsyncGenerator = newLocationGenerator +> calculateRouteAsync //GeneratorFunction, AsyncFunction
```

```javascript
const nextRouteAsyncGenerator = newLocationAsyncGenerator +> calculateRoute //AsyncGeneratorFunction, Function
```

It would be usable to tersely express the following:

```javascript
const switchOnEngineThenDrive = ()=>{switchOnEngine(); drive()}
```

as:

```javascript
const switchOnEngineThenDrive = switchOnEngine +> drive
```

Although it evaluates to `drive(switchOnEngine())` upon execution, it behaves the same as sequential execution for all intents and purposes, in cases of no-args functions.

As an analogy for how `x = x + y` is expressable as `x += y`, the following:

```javascript
x = x +> y
```

would be expressable as:

```javascript
x +>= y
```

e.g. for composing functions in a loop.

## Why `+>`?

To express accumulation via the `+` and function ordering via the `>`, and so as not to conflict with the pipeline-operator proposal here: https://github.com/tc39/proposal-pipeline-operator which has prior art from other languages. Discussion: https://github.com/tc39/proposal-pipeline-operator/issues/50

## Why treat `AsyncFunction`, `GeneratorFunction` and `AsyncGeneratorFunction` differently than their promise/iterator returning `Function` equivalents? They are the same in all other contexts!

### 1. Semantic expectation.

For
```js
async input=>output
```

I semantically expect `output` to be piped to the next function in the chain.

For
```js
function*(){
    yield output;
}
```

I semantically expect `output` to be utilized.

### 2. Less chance of bugs in the problem space

Piping the underlying promise/iterator instead of the declared output requires careful and repetitive boilerplate to compose an `AsyncFunction`, `GeneratorFunction` or `AsyncGeneratorFunction` from other `Function`s, `AsyncFunction`s, `GeneratorFunction`s and `AsyncGeneratorFunction`s, thereby causing a greater surface area for mistakes and bugs in the problem space.

### 3. Smaller learning curve for async and generator function composition

For the same reasons, it is possible to compose async and generator functions without necessarily even knowing anything about promises and iterators. (For example, C# uses `async` and `await` but without promises, but the usage pattern is the same).

### 4. Piping promises and iterators would be supported otherwise anyway

It is unclear what practical advantage there could possibly be of piping non-explicitly-returned promises/iterators instead of the declared output, especially since piping explicitly returned promises/iterators in a declared `Function` would work as expected anyway.

## Isn't overloading an operator with different types producing different expression results a bad thing?

There is precedent. `myNumber + 'myString'` has been and continues to be used perfectly intuitively since inception. The proposed expression results are intuitive based on the arguments supplied. It is necessary to allow composition between different function types, as the examples show, so it makes sense to allow them to use the same operator.
