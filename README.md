# what should not be used in javascript

# Table of Contents

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

-Inefficient DOM Manipulation

Repaints and Reflows: The DOM is a tree-like structure representing the HTML of your webpage. Whenever you modify the DOM (add, remove, or change elements), the browser needs to:

Reflow: Recalculate the size and position of all elements on the page. This happens when changes affect the layout ( resizing elements, adding/removing nodes, or modifying width, height, margin , ...).

Repaint: Update the visual appearance of the affected elements on the screen. This occurs when changes don't affect the layout but only the appearance (e.g., changing colors, background images)


Frameworks like React, Vue, and Angular generally handle DOM updates efficiently, often employing techniques like:

Virtual DOM: This creates an in-memory representation of the actual DOM. When data changes, the framework compares the new data with the previous state of the virtual DOM.

Batching Updates: These frameworks often batch multiple state or prop changes into a single re-render cycle. 

Overriding Framework's Mechanisms:

Using document.getElementById() or querySelector() within component methods to directly manipulate elements.

Using innerHTML to modify large portions of the DOM.

Use CSS Classes:CSS files can be cached by the browser. When using inline styles, the browser cannot cache the styles efficiently, as they are embedded within the HTML. This means that the browser needs to re-parse and apply the styles every time the page is loaded.

 Avoid Layout Thrashing:

Layout thrashing occurs when JavaScript alternates between reading and writing DOM properties multiple times in a way that forces the browser to repeatedly recalculate the layout (reflow) of the page.

When you read a DOM property like offsetWidth, offsetHeight, or getComputedStyle, the browser needs to ensure the layout is up-to-date, triggering a reflow if necessary.

const items = document.querySelectorAll(".item");

//bad

items.forEach(item => {
    const width = item.offsetWidth; // Forces a reflow
    item.style.width = `${width + 10}px`; // force reflow
});

//good

const items = document.querySelectorAll(".item");

const widths = Array.from(items).map(item => item.offsetWidth);

items.forEach((item, index) => {
    item.style.width = `${widths[index] + 10}px`;
});

 Use CSS for Animations and Layout Changes:

 Where possible, offload work to the GPU by using CSS for animations and transitions.

Avoid animating layout-affecting properties like width, height, or top. Animate GPU-accelerated properties like transform or opacity instead.

.item {

    transition: transform 0.3s ease;
    
}

-For-in Loops on Arrays:

The for...in loop iterates over all enumerable string properties of an object, including those inherited from its prototype chain.

for...in can be slower compared to alternatives like Object.keys() or for...of because it processes enumerable properties from the prototype chain.For every property, the engine checks whether it's part of the object's own properties or inherited.

If an array has custom properties or methods attached, for...in will iterate over these as well.

Better Alternatives for Arrays:

for: Classic loop, fast and reliable.

for...of: Modern and optimized for arrays.

forEach: Functional and readable.

-Blocking the Main Thread:
The main thread is where a browser processes user events and paints. By default, the browser uses a single thread to run all the JavaScript in your page, as well as to perform layout, reflows, and garbage collection. This means that long-running JavaScript functions can block the thread, leading to an unresponsive page and a bad user experience.

function slowTask() {

    const end = Date.now() + 5000; // Calculate the end time (5 seconds from now)

    while (Date.now() < end) {
    
        // Continuously check if the current time has reached the end time
    
    }
    
}

slowTask();

While the slowTask function is running, the event loop is stuck executing the while loop.

No new tasks (e.g., handling user input or rendering) can be processed until the slowTask function completes.

Performing expensive DOM updates or large reflows can block the main thread.

const container = document.getElementById("container");

for (let i = 0; i < 100000; i++) {

    const div = document.createElement("div");
    
    div.textContent = `Item ${i}`;
    
    container.appendChild(div);

}

Blocking with Large Lists or Tables:

function Sample() {
    
    const items = Array.from({ length: 10000 }, (_, i) => `Item ${i}`);
    
    return (
    
        <div>
        
            {items.map((item) => (
            
                <div key={item}>{item}</div>
           
            ))}
            
        </div>
   
    );
    
}

React tries to render all 10,000 items at once, which blocks the main thread and causes UI freezes.

Use a library like React Window or React Virtualized to render only the visible items.

Solutions to Avoid Blocking:

Break Tasks into Smaller Chunks: Use techniques like splitting work into smaller chunks

Debounce or Throttle Expensive Handlers

use Framework Optimizations:

Use React's useMemo, useCallback, and React.lazy to prevent unnecessary computations and rendering.



  
