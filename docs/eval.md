The argument of the eval() function is a string. It will evaluate the source string as a script body, which means both statements and expressions are allowed. It returns the completion value of the code.
console.log(eval('2 + 2'));
// Expected output: 4

The key feature of eval() is that it can execute code that isn’t known until runtime.

const code = "let y = 20; y * 2";
console.log(eval(code)); // Output: 40

Code Injection Risks:
If the string passed to eval() comes from an external source (e.g., user input), it can execute harmful code.

Hard to Debug:
Since the code is dynamic, errors or issues caused by eval() strings are harder to trace.

Performance Impact:

JavaScript engines cannot optimize code inside eval() because it is dynamic and unpredictable.
This can make the program slower.

To Parse JSON Strings: Use JSON.parse() instead of eval().
To Execute Code Dynamically: Use functions, conditionals, or modules for structured and predictable execution.
