# `+>` for function composition

The statement:

```javascript
const doubleThenSquareThenHalf = value=>half(square(double(value)))
```

would be rewritable as:

```javascript
const doubleThenSquareThenHalf = double +> square +> half
```

As well as a function, it would also accept a promise (asynchronous function) as any of its operands, upon which the expression would also evaluate to a promise:

```javascript
const doubleThenSquareThenHalf = await double +> squareAsync +> half
```

It could be used to tersely express the following:

```javascript
const switchOnEngineThenDrive = ()=>{switchOnEngine(); drive()}
```

as:

```javascript
const switchOnEngineThenDrive = switchOnEngine +> drive
```

Although it evaluates to `drive(switchOnEngine())`, it behaves the same for all intents and purposes, in cases of no-args functions.

As an analogy for how `x += y` is shorthand for `x = x + y`, the following:

```javascript
x +>= y
```

could be expressed as a shorthand for

```javascript
x = x +> y
```

e.g. for composing functions in a loop.

# Why `+>`?

To express accumulation via the `+` and function ordering via the `>`, and so as not to conflict with the pipeline-operator proposal here: https://github.com/tc39/proposal-pipeline-operator which has prior art from other languages. Discussion: https://github.com/tc39/proposal-pipeline-operator/issues/50

# Operator precedence

It should glue more strongly than `await`, and more strongly than `|>` should that be introduced into the language too, so that:

```javascript
const output = 100 |> processIterations +> generateOutput
```

behaves as you would want.