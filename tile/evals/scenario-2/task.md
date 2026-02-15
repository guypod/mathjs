# Bit Manipulation Utilities

Build a set of bit manipulation utilities for working with integers at the binary level.

## Requirements

Create three functions:

1. `setBit(number, position)` - Sets the bit at the given position (0-indexed from right) to 1
2. `clearBit(number, position)` - Clears the bit at the given position to 0
3. `extractBits(number, start, length)` - Extracts `length` bits starting at position `start`, returning the extracted value

All operations should work with positive integers. Use appropriate bitwise operations for each function.

## Dependencies { .dependencies }

### mathjs 15.1.0 { .dependency }

A comprehensive mathematics library for JavaScript providing bitwise operations for integer manipulation.

## Test Cases

- `setBit(8, 0)` returns 9 (binary: 1000 → 1001) [@test](./test-1.js)
- `clearBit(15, 2)` returns 11 (binary: 1111 → 1011) [@test](./test-2.js)
- `extractBits(0b11010110, 2, 4)` returns 9 (extracts 1001) [@test](./test-3.js)
