# Function composition operator

A proposal here https://github.com/tc39/proposal-pipeline-operator describes an alternative syntax for calling a function and passing its result to another function. The current syntax:

```javascript
const doubledThenSquaredAnswer = square(double(value))
```
could thus be rewritten as

```javascript
const doubledThenSquaredAnswer = value |> double |> square
```

This straightens the ordering and replaces `()` with with using `|>` in a linear fashion but doesn't save code otherwise. The other problem though is that the first expression is different in usage type to the second and third: `InputExpression |> FunctionExpression |> FunctionExpression` which has a logical inconsistency, even while the existing syntax, which is well understood, doesn't.

However thanks to a query from Simon Staton here: https://esdiscuss.org/topic/native-function-composition and the resulting speculation, the `|>` operator could be more useful in terms of code terseness as well as having logical consistency, as a function composition operator instead:

The current syntax:

```javascript
const doubleThenSquare = value=>square(double(value))
```

could be rewritten as:

```javascript
const doubleThenSquare = double |> square
```

Further discussion: https://github.com/tc39/proposal-pipeline-operator/issues/50