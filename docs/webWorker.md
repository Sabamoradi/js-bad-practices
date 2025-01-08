How forEach Works:
forEach calls the provided callback function for each array element, passing the element, its index, and the array itself as arguments.
For primitive types (e.g., numbers, strings), the callback function works with a copy of the element, meaning modifications inside the callback do not affect the original array.
When the array contains objects, the callback can mutate the properties of these objects because they are passed by reference.
forEach always returns undefined, making it unsuitable for chaining operations or transforming arrays.

Limitations of forEach:
Unlike for or while loops, forEach does not support breaking out of the loop prematurely.
Modifying the array (e.g., adding or removing elements) while iterating can lead to unpredictable results due to index shifting.
While not inherently slower, the inability to optimize for specific cases (e.g., skipping iterations or breaking early) may make forEach less efficient in some scenarios.
