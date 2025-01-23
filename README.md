# what should not be used in javascript

# Table of Contents
- [Introduction]
  
    -[eval]
  
    -[Inefficient DOM Manipulation]
  
    -[webWorker]

-eval

The argument of the eval() function is a string. It returns the completion value of the code.

console.log(eval('2 + 2')); // Expected output: 4

The key feature of eval() is that it can execute code that isn’t known until runtime.

const code = "let y = 20; y * 2";

console.log(eval(code)); // Output: 40

Code Injection Risks:

If the string passed to eval() comes from an external source (e.g., user input), it can execute harmful code.

Performance Impact:

JavaScript engines cannot optimize code inside eval() because it is dynamic and unpredictable.

Just-In-Time (JIT) Compilation: Modern JavaScript engines use techniques like JIT compilation to optimize code for speed. JIT compilers analyze the code, identify patterns, and generate highly optimized machine code.

eval() Breaks Predictability:

Since eval() can inject new code dynamically, the JIT compiler cannot safely assume that the compiled code will stay valid. The engine may fall back to slower interpreted execution or deoptimize the surrounding code.

Consequences of Poor Optimization:

Reduced Performance: The inability to optimize eval() code can lead to slower execution, especially when eval() is used frequently or with complex code.

Increased Memory Usage: Repeatedly parsing and compiling code can consume more memory.

To Parse JSON Strings: Use JSON.parse() instead of eval().

To Execute Code Dynamically: Use functions, conditionals, or modules for structured and predictable execution.

Inefficient DOM Manipulation
  
