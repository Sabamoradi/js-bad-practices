# what should not be used in javascript

# Table of Contents

    -[eval]
  
    -[Inefficient DOM Manipulation]

    -[For-in]

    -[Blocking the Main Thread]
  
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








  
