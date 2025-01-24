# what should not be used in javascript

## Table of Contents
- [Overview](#overview)
- [eval() in JavaScript](# `eval()` in JavaScript?)

## `eval()` in JavaScript?
The eval() function takes a string as its argument and evaluates it as JavaScript code. It returns the result of the executed code.

```javascript
const code = "let y = 20; y * 2";
console.log(eval(code)); // Output: 40
```
Executing Dynamic Code: The key feature of eval() is its ability to execute code that isn’t known until runtime.

```javascript
const code = "let y = 20; y * 2";
console.log(eval(code)); // Output: 40
```

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

Web Workers:

Web Workers makes it possible to run a script operation in a background thread separate from the main execution thread of a web application or One of the most common ways to introduce multithreading in web applications is to use worker threads. The advantage of this is that laborious processing can be performed in a separate thread(Parallel Execution), allowing the main (usually the UI) thread to run without being blocked/slowed down.

you can run almost any code you like inside a worker thread. There are some exceptions: for example, you can't directly manipulate the DOM from inside a worker, or use some default methods and properties of the Window object. 

Data is sent between workers and the main thread via a system of messages — both sides send their messages using the postMessage() method, and respond to messages via the onmessage event handler

Web Workers can be integrated into frameworks like React, Angular, or Next.js. For example:

Libraries like workerize-loader or comlink make it simpler to set up and manage Web Workers.

These tools abstract away some of the complexities, enabling developers to focus on core functionality.

 Service Workers are a type of Web Worker designed for a specific purpose (managing network requests, caching, offline capabilities, etc.)
 
 Offline Support, Caching for Performance, Push Notifications , etc.

 Offline Support: Service Workers can intercept network requests and provide fallback responses from a cache when the user is offline, ensuring the app continues to work even without an internet connection.

 Caching for Performance: Service Workers can cache assets (like images, scripts, or API responses), so subsequent visits to your app are faster and more efficient. This is particularly useful for Progressive Web Apps (PWAs), where the app needs to function offline or in low network conditions.

Push Notifications: Service Workers can handle background push notifications and show them to users, even when they’re not actively using the app.

No Service Worker to Manage Caching: Without a Service Worker, there’s no mechanism to control how your app’s assets (like HTML, CSS, JavaScript files) are cached. Normally, a PWA would use a Service Worker to cache static resources (e.g., images, CSS, JS), so the app works offline and loads faster on subsequent visits. If you don’t have a Service Worker, the browser might cache your assets based on its default caching rules, which can cause old versions of your app to be served even after you release a new version.

 Web Workers can be used for:
 
Background tasks: Fetching data, processing files, and other tasks that don't require immediate UI feedback.

Machine learning models: Running inference on machine learning models in the background.

-Garbage collection:

Garbage collection (GC) is a process by which a programming language runtime automatically manages memory. It ensures that memory used by objects that are no longer needed is reclaimed so that it can be reused. This helps avoid memory leaks and optimizes the performance of the application.

In simpler terms, GC frees up memory used by objects that are no longer in use, allowing developers to focus on the application logic without worrying about memory management.

Java (Automatic GC): In Java, the Java Virtual Machine (JVM) automatically handles memory management. The garbage collector in Java tracks objects that are no longer referenced by any other objects and reclaims their memory.

C++ (Manual Memory Management): Unlike Java, C++ does not have automatic garbage collection. In C++, developers must explicitly manage memory using new and delete to allocate and free memory.

Python (Automatic GC with Reference Counting and Cycle Detection): Python uses reference counting as the primary method of garbage collection. When an object’s reference count drops to zero, the memory is automatically freed. Additionally, Python uses a garbage collector to handle circular references.

Garbage Collection in JavaScript:

JavaScript uses automatic garbage collection to manage memory. Unlike C++, developers don’t have to manually allocate or free memory. The JavaScript engine automatically identifies and frees up memory from objects that are no longer referenced.

Memory life cycle

Regardless of the programming language, the memory life cycle is pretty much always the same:

Allocate the memory you need

Use the allocated memory (read, write)

Release the allocated memory when it is not needed anymore

The second part is explicit in all languages. The first and last parts are explicit in low-level languages but are mostly implicit in high-level languages like JavaScript.

Allocation in JavaScript:

Value initialization, function calls , Some methods allocate new values or objects:

Using values: Using values basically means reading and writing in allocated memory. This can be done by reading or writing the value of a variable or an object property or even passing an argument to a function.

Release when the memory is not needed anymore: The majority of memory management issues occur at this phase. JavaScript, utilize a form of automatic memory management known as garbage collection (GC). The purpose of a garbage collector is to monitor memory allocation and determine when a block of allocated memory is no longer needed and reclaim it. This automatic process is an approximation since the general problem of determining whether or not a specific piece of memory is still needed is undecidable.

The main concept that garbage collection algorithms rely on is the concept of reference. A reference is essentially a pointer or link from one object to another, which indicates that the former is using or relying on the latter.

Reference-counting garbage collection: This algorithm reduces the problem from determining whether or not an object is still needed to determining if an object still has any other objects referencing it. An object is said to be "garbage", or collectible if there are zero references pointing to it. There is a limitation when it comes to circular references. In the following example, two objects are created with properties that reference one another, thus creating a cycle. They will go out of scope after the function call has completed. At that point they become unneeded and their allocated memory should be reclaimed. However, the reference-counting algorithm will not consider them reclaimable since each of the two objects has at least one reference pointing to them, resulting in neither of them being marked for garbage collection. Circular references are a common cause of memory leaks.

let x = {
  a: {
    b: 2,
  },
};

let y = x;

let z = y.a;

Mark-and-sweep algorithm: This algorithm assumes the knowledge of a set of objects called roots. In JavaScript, the root is the global object. Periodically, the garbage collector will start from these roots, find all objects that are referenced from these roots, then all objects referenced from these, etc. Starting from the roots, the garbage collector will thus find all reachable objects and collect all non-reachable objects.

This algorithm is an improvement over the previous one since an object having zero references is effectively unreachable. The opposite does not hold true as we have seen with circular references.

Marking: The garbage collector marks all the objects that are reachable (i.e., those still referenced by other objects).

Sweeping: After marking the reachable objects, the garbage collector sweeps through the heap and frees the memory used by the objects that are no longer referenced.

Compacting (optional): The garbage collector may compact memory by moving objects around in the heap to minimize fragmentation.

Memory Leaks in JavaScript: Memory leaks in JavaScript happen when objects are no longer needed but are still referenced somewhere in the code, preventing the garbage collector from reclaiming their memory. Common causes of memory leaks in JavaScript include:

Global variables: Variables that are unintentionally left in the global scope.

Closures: Functions that reference variables from an outer scope and prevent those variables from being garbage collected.

Event listeners: Not removing event listeners can keep references to DOM elements and prevent them from being garbage collected.

Detached DOM nodes: If a DOM node is removed from the document but still referenced by JavaScript, it cannot be garbage collected.







  
