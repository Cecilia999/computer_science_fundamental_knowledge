## 1. byte: 8-bit

- The byte data type is an 8-bit signed two's complement integer.
- range [-128, 127]
- The byte data type can be useful for saving memory in large arrays, where the memory savings actually matters.

## 2. short: 16-bit

- The short data type is a 16-bit signed two's complement integer.
- range [-32,768 , 32,767]
- As with byte, the same guidelines apply: you can use a short to save memory in large arrays, in situations where the memory savings actually matters.

## 3. int: 32-bit

- By default, the int data type is a 32-bit signed two's complement integer, range in [-2^31, 2^31-1].
- In Java SE 8 and later, you can use the int data type to represent an unsigned 32-bit integer, which range in [0, 2^32-1].

## 4. long: 64-bit

- The long data type is a 64-bit two's complement integer.
- The signed long range in [2^63, 2^63-1].
- In Java SE 8 and later, you can use the long data type to represent an unsigned 64-bit long, which range in [0, 2^64-1].

## 5. float: single-precision 32-bit 精确一位小数

## 6. double: double-precision 64-bit 精确两位小数

## 7. boolean: 1-bit

- The boolean data type has only two possible values: true and false.

## 8. char: 16-bit

- The char data type is a single 16-bit Unicode character. It has a minimum value of '\u0000' (or 0) and a maximum value of '\uffff' (or 65,535 inclusive).
