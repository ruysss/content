---
title: Less than (<)
slug: Web/JavaScript/Reference/Operators/Less_than
tags:
  - JavaScript
  - Language feature
  - Operator
  - Reference
browser-compat: javascript.operators.less_than
---

{{jsSidebar("Operators")}}

The less than operator (`<`) returns `true` if the left operand is less than the right operand, and `false` otherwise.

{{EmbedInteractiveExample("pages/js/expressions-less-than.html")}}

## Syntax

```js-nolint
x < y
```

## Description

The operands are compared with multiple rounds of coercion, which can be summarized as follows:

- First, objects are converted to primitives using [`@@toPrimitive()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/toPrimitive) (with `"number"` as hint), [`valueOf()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/valueOf), and [`toString()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString) methods, in that order. The left operand is always coerced before the right one.
- If both values are strings, they are compared as strings, based on the values of the Unicode code points they contain.
- Otherwise JavaScript attempts to convert non-numeric types to numeric values:
  - Boolean values `true` and `false` are converted to 1 and 0 respectively.
  - `null` is converted to 0.
  - `undefined` is converted to `NaN`.
  - Strings are converted based on the values they contain, and are converted as `NaN` if they do not contain numeric values.
- If either value is [`NaN`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN), the operator returns `false`.
- Otherwise the values are compared as numeric values. BigInt and number values can be compared together.

Other operators, including [`>`](/en-US/docs/Web/JavaScript/Reference/Operators/Greater_than), [`>=`](/en-US/docs/Web/JavaScript/Reference/Operators/Greater_than_or_equal), and [`<=`](/en-US/docs/Web/JavaScript/Reference/Operators/Less_than_or_equal), use the same algorithm as `<`. There are two cases where all four operators return `false`:

- If one of the operands gets converted to a BigInt, while the other gets converted to a string that cannot be converted to a BigInt value (it throws a [syntax error](/en-US/docs/Web/JavaScript/Reference/Errors/Invalid_BigInt_syntax) when passed to [`BigInt()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt/BigInt)).
- If one of the operands gets converted to `NaN`. (For example, strings that cannot be converted to numbers, or `undefined`.)

For all other cases, the four operators have the following relationships:

```js
x < y === !(x >= y);
x <= y === !(x > y);
x > y === y < x;
x >= y === y <= x;
```

> **Note:** One observable difference between `<` and `>` is the order of coercion, especially if the coercion to primitive has side effects. All comparison operators coerce the left operand before the right operand.

## Examples

### String to string comparison

```js
console.log("a" < "b");        // true
console.log("a" < "a");        // false
console.log("a" < "3");        // false
```

### String to number comparison

```js
console.log("5" < 3);          // false
console.log("3" < 3);          // false
console.log("3" < 5);          // true

console.log("hello" < 5);      // false
console.log(5 < "hello");      // false

console.log("5" < 3n);         // false
console.log("3" < 5n);         // true
```

### Number to Number comparison

```js
console.log(5 < 3);            // false
console.log(3 < 3);            // false
console.log(3 < 5);            // true
```

### Number to BigInt comparison

```js
console.log(5n < 3);           // false
console.log(3 < 5n);           // true
```

### Comparing Boolean, null, undefined, NaN

```js
console.log(true < false);     // false
console.log(false < true);     // true

console.log(0 < true);         // true
console.log(true < 1);         // false

console.log(null < 0);         // false
console.log(null < 1);         // true

console.log(undefined < 3);    // false
console.log(3 < undefined);    // false

console.log(3 < NaN);          // false
console.log(NaN < 3);          // false
```

### Comparison with side effects

Comparisons always coerce their operands to primitives. This means the same object may end up having different values within one comparison expression. For example, you may have two values that are both greater than and less than the other.

```js
class Mystery {
  static #coercionCount = -1;
  valueOf() {
    Mystery.#coercionCount++;
    // The left operand is coerced first, so this will return 0
    // Then it returns 1 for the right operand
    return Mystery.#coercionCount % 2;
  }
}

const l = new Mystery();
const r = new Mystery();
console.log(l < r && r < l);
// true
```

> **Warning:** This can be a source of confusion. If your objects provide custom primitive conversion logic, make sure it is _idempotent_: multiple coercions should return the same value.

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- [Greater than operator](/en-US/docs/Web/JavaScript/Reference/Operators/Greater_than)
- [Greater than or equal operator](/en-US/docs/Web/JavaScript/Reference/Operators/Greater_than_or_equal)
- [Less than or equal operator](/en-US/docs/Web/JavaScript/Reference/Operators/Less_than_or_equal)
