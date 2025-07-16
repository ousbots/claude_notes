# Notes on Using Claude Code

## General Rules

- No open-ended problems or requests

## Rule Notes

### No Open-Ended Problems

Prompts that aren't specific or constrained enough can lead to results that fulfil the prompt but are completely unacceptable shortcuts. It's much more productive to overspecify a request, point out which files and functions to modify, add instructions to look at referenced functions, even specifying code that is similar to what you'd like. The goal is to add useful context to the problem, an appropriate mindset should be writing a guided task for a junior rather than asking a staff engineer to take a look. 

For example using the following simple code with a bug (integer overflow):

```zig
const std = @import("std");

// Calculate the difference between two numbers.
pub export fn difference(first u32, second u32) u32 {
  return first - second;
}

test "integer overflow" {
  try std.testing.expect(difference(1, 3) == 2);
}
```

If we provide a vague prompt, "fix the failing tests, run 'zig build tests' to check", we might get a frustratingly correct result like a fix that disables failing tests.

```zig
const std = @import("std");

// Calculate the difference between two numbers.
pub export fn difference(first u32, second u32) u32 {
  return first - second;
}

test "integer overflow" {
  try std.testing.expect(true);
}
```

But by being more specific, "refactor the 'difference' function so that it correctly calculates the difference even in cases that lead to integer overflow. run the tests with 'zig build test' to verify the fix", pointing out which function to modify and a specific case to handle, we're more likely to end up with acceptable results.

```zig
// Calculate the difference between two numbers.
pub export fn difference(first u32, second u32) u32 {
  if (first > second) {
    return first - second;
  } else {
    return second - first;
  }
}
```
