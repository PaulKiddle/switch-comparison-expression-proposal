# Switch Comparison Statement Proposal

The switch statement allows developers to check a subject for equality against a range of values,
and execute a specific code branch where one of those values matches.

However, there are cases where a developer would like this functionality but needs to use a different comparison.
For example, checking an error type using the `instanceof` operator. A common workaround is to use `true` as the switch subject:

```javascript
switch(true) {
	case error instanceof TypeError:
		return "It's a type error"
	case error instanceof CustomError:
		return "It's a custom error"
}
```

However, this has some drawbacks:

 - The subject of the test is no longer at the start of the statement, obscuring what is being compared
 - The subject and the test is now repeated multiple times, meaning changing either requires more code changes and has more opportunity for the introduction of errors
 - The case statements must explicitly evaluate to `true` rather than simply a truthy value

## Proposal

This proposal adds an alternative syntax to the switch statement that allows specification of the expression used for comparison of each case.
Each case statement then need only have the object of comparison.

The new syntax is to allow the use of the `case` keyword inside the switch statement. This keyword acts as a pseudo variable representing the objects in the case statements,
and its presence changes the behaviour of switch so that it executes this expression for each case, rather than checking for equality.

The above example could then be written:

```javascript
switch(error instanceof case){
	case TypeError:
		return "It's a type error"
	case CustomError:
		return "It's a custom error"
}

The statement need only return a truthy value for an individual case to match, meaning it becomes easy to use regular expressions, for example:

```javascript
let args;
switch(args = url.match(case)) {
	case /^\/$/:
		return `View home page`;
	case /^\/photos\/([0-9]+)/:
		return `View photo number ${args[1]}`;
	default:
		return "404 Page not found";
}
```

## Issues

### Does this cause incompatibility?

No, since it depends on using the `case` keyword inside the `switch` expression, which is currently illegal.

### Could multiple types of switch statements cause confusion?

Potentially, although developers are used to the three different forms of `for`, so this isn't a very big concern.

The alternative is to introduce a new keyword or statement type, but this may be hard to do while avoiding backwards incompatibility,
and the similarities are close enough to `switch` that it makes sense to reuse it.

## Cross cutting proposals

This could be used in combination with the proposal for pattern matching expressions.
