# Caching

* Caching Overview
    • Caching improves performance by storing results of expensive operations for future use
    • Commonly used in computing: CPU caches, browser caches, etc.
    • Example: storing the result of a slow calculation to avoid recomputation

* Caching Return Values
    • Example: counting peaches in a box once and storing the result
    • Improves efficiency for repeated queries on unchanged data

* Caching Pure Functions
    • Pure Function: output depends only on input, no side effects
    • Cache results using input as the key

    Example:
    ```js
    const cache = {};

    function multiply(a, b) {
        let product = 0;
        for (let i = 0; i < b; i++) {
            product += a;
        }
        return product;
    }

    function multiplyCache(a, b) {
        const key = `${a}x${b}`;
        if (cache[key] === undefined) {
            cache[key] = multiply(a, b);
        }
        return cache[key];
    }
    ```

* Caching Sub-problems
    • Use dynamic cache for recursive functions

    Example: recursive multiplication
    ```js
    function recursiveMultiply(a, b) {
        if (b === 1) return a;
        return a + recursiveMultiply(a, b - 1);
    }

    function recursiveMultiplyCached(a, b) {
        if (b === 1) return a;
        const key = `${a}x${b}`;
        if (cache[key] === undefined) {
            cache[key] = a + recursiveMultiplyCached(a, b - 1);
        }
        return cache[key];
    }
    ```

* Memoization
    • Storing results of expensive function calls

    Example: Fibonacci sequence
    ```js
    const cache = {};

    function fib(n) {
        if (n === 1) return 0;
        if (n === 2) return 1;
        if (cache[n] === undefined) {
            cache[n] = fib(n - 1) + fib(n - 2);
        }
        return cache[n];
    }
    ```

* Tabulation
    • Iteratively building the cache from the bottom-up

    Example: Fibonacci sequence
    ```js
    const cache = {};

    function fibTab(n) {
        cache[1] = 0;
        cache[2] = 1;
        for (let i = 3; i <= n; i++) {
            cache[i] = cache[i - 1] + cache[i - 2];
        }
        return cache[n];
    }
    ```

* Memoization vs. Tabulation
    • Both are forms of dynamic programming: solving sub-problems once and reusing results
    • Memoization: top-down approach
    • Tabulation: bottom-up approach

* Key Takeaways
    • Caching optimises performance by storing computed results for reuse
    • Techniques include memoization, tabulation, and dynamic programming
    • Effective for recursive, iterative, and asynchronous problems


# Memoization of Fibonacci

* Memoization Overview
    • Memoization reduces redundant calculations in recursive algorithms by storing results of sub-problems
    • Key features:
        1. Recursive function
        2. Data structure (usually an object) to store results (memo)

* When to Use Memoization
    • Effective for problems with overlapping sub-problems
    • Example: calculating coin combinations or Fibonacci numbers

* Example: Memoizing Factorial

    • Naive implementation:
    ```js
    function factorial(n) {
        if (n === 1) return 1;
        return n * factorial(n - 1);
    }
    ```

    • Memoized Implementation:
    ```js
    let memo = {};

    function factorial(n) {
        if (n in memo) return memo[n];
        if (n === 1) return 1;

        memo[n] = n * factorial(n - 1);
        return memo[n];
    }
    ```
    * Memoization stores intermediate results to avoid duplicate calculations, improving efficiency


* Memoizing Fibonacci

    • Naive Implementation:
    ```js
    function fib(n) {
        if (n === 1 || n === 2) return 1;
        return fib(n - 1) + fib(n - 2);
    }
    ```

    • Memoized Implementation:
    ```js
    function fastFib(n, memo = {}) {
        if (n in memo) return memo[n];
        if (n === 1 || n === 2) return 1;

        memo[n] = fastFib(n - 1, memo) + fastFib(n - 2, memo);
        return memo[n];
    }
    ```


* Complexity Analysis

    • Naive Fibonacci:
        • Time Complexity: O(2^n) - exponential time due to redundant calculations

    • Memoized Fibonacci:
        • Time Complexity: O(n) - linear time by storing and reusing results


* Steps to Memoize a Function
    1. Write the basic recursive function
    2. Add a memo object as an additional argument
    3. Check the memo for existing results before computing
    4. Store new results in the memo before returning

* Key Takeaways
    • Memoization can transform an algorithm from exponential time complexity to linear time complexity
    • Use memoization for recursive problems with overlapping sub-problems to significantly improve performance
