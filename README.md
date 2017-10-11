# `+>` for function composition

Summary: a proposed function composition operator that allows terse and intuitive composition from and to any of the 4 available function types: function, asynchronous function, generator function and asynchronous generator function.

The statement:

```javascript
const doubleThenSquareThenHalf = value=>half(square(double(value)))
```

is rewritable as:

```javascript
const doubleThenSquareThenHalf = double +> square +> half
```

Introducing an async function produces an async function:

```javascript
const doubleThenSquareThenHalfAsync = double +> squareAsync +> half
```

Introducting a generator function produces a generator function that pipes each yielded value to subsequent functions:

```javascript
const randomBetween1And100Generator = randomBetween0And1Generator +> multiplyBy100
```

Introducing an async function and a generator function, or an async generator function, produces an async generator function:

```javascript
const nextRouteAsyncGenerator = newLocationGenerator +> calculateRouteAsync //sync generator, async function
```

```javascript
const nextRouteAsyncGenerator = newLocationAsyncGenerator +> calculateRoute //async generator, sync function
```

It would be usable to tersely express the following:

```javascript
const switchOnEngineThenDrive = ()=>{switchOnEngine(); drive()}
```

as:

```javascript
const switchOnEngineThenDrive = switchOnEngine +> drive
```

Although it evaluates to `drive(switchOnEngine())`, it behaves the same as sequential execution for all intents and purposes, in cases of no-args functions.

As an analogy for how `x = x + y` is expressable as `x += y`, the following:

```javascript
x = x +> y
```

would be expressable as:

```javascript
x +>= y
```

e.g. for composing functions in a loop.

# Why `+>`?

To express accumulation via the `+` and function ordering via the `>`, and so as not to conflict with the pipeline-operator proposal here: https://github.com/tc39/proposal-pipeline-operator which has prior art from other languages. Discussion: https://github.com/tc39/proposal-pipeline-operator/issues/50
