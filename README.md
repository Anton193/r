# Traversal Tool

## Overview

The Traversal Tool is a JavaScript library designed for navigating and processing nested data structures using a path-based syntax. Built around the `traverse` (aliased as `x`) class, it enables developers to access, transform, and manipulate data in JSON-like objects or arrays with ease. The tool offers a robust set of configuration options and static methods to handle complex data traversal tasks, including fallback paths, result flattening, unique value filtering, and limiting output size. It is ideal for applications requiring precise data extraction and transformation.

## Features

- **Path-Based Traversal**: Navigate nested data using string paths (e.g., `"user>name"`) or arrays of paths.
- **Configurable Options**:
  - `default`: Specify a fallback value when no data is found.
  - `fallback`: Define alternative paths to try if the primary path fails.
  - `flatten`: Flatten nested arrays in the result for simpler output.
  - `unique`: Remove duplicates from array results.
  - `limit`: Restrict the number of items in array outputs.
  - `debug`: Enable debug mode to log traversal details (integrates with external logging).
- **Static Methods**:
  - `Find`: Search for data at a specified path.
  - `Filter`: Apply a custom function to filter or transform results.
  - `group`: Select or group results based on an index or logic.
- **Error Handling**: Validates inputs and configurations, throwing clear errors for invalid JSON, paths, or options.
- **Flexible Output**: Returns results with a `value` property and additional methods (`Find`, `Filter`, `group`) for further processing.

## Installation

1. Ensure Node.js is installed on your system.
2. Clone or download the repository containing the traversal tool.
3. Install dependencies (if any) by running:

```bash
npm install @AntonThomz/traverse_obj
```
4. Include the traversal tool in your project:

```javascript
import { traverse } from '@AntonThomz/traverse_obj';
```

## Usage

The `traverse` class (aliased as `x`) is the core interface for setting input data, configuring options, defining traversal paths, and retrieving results. Below are detailed explanations of how to use its configuration options and static methods.

### Basic Setup

Set up the tool by defining the input data and traversal path:

```javascript
import { traverse } from '@AntonThomz/traverse_obj';

// Set input data
traverse.input = {
  data: { name: { value: "Anton" }}
};

// Set traversal path
traverse.run = "data>...>value";

// Get the result
console.log(traverse.run.value); // Output: "Anton"
```

### Core Methods and Properties

- `traverse.input`: Sets the input data, accepting either a JavaScript object or a valid JSON string.
- Example:

    ```javascript
    traverse.input = { data: { value: 42 } }; // Object input
    traverse.input = '{"data":{"value":42}}'; // JSON string input
    ```
  - Throws `TypeError` if the input is not a valid object or JSON string.
- `traverse.options`: Configures traversal behavior using the `Options` class (see below for details).
- `traverse.run`: Sets the traversal path (string or array) and retrieves the result.
  - Example:

    ```javascript
    traverse.run = "data>value";
    console.log(traverse.run.value); // Output: 42
    ```
  - Supports additional properties and methods:
    - `traverse.run.value`: The final processed result after applying options.
    - `traverse.run.keys`: The parsed traversal path (e.g., `"data>value"`).
    - `traverse.run.Find(path)`: Searches for data at the specified path.
    - `traverse.run.Filter(fn)`: Applies a custom filter function to the result.
    - `traverse.run.group(n)`: Selects or groups results based on an index or logic.
- `traverse.run.keys`: Retrieves the parsed traversal path.
  - Example:

    ```javascript
    traverse.run = "user>details>address";
    console.log(traverse.run.keys); // Output: "user>details>address"
    ```

### Configuration Options

The `Options` class allows fine-grained control over traversal behavior. Set options using `traverse.options` or individual setters via `Options.setOption`.

#### Setting Options

```javascript
traverse.options = {
  default: "No data found",
  fallback: ["backup>path", "alternate>path"],
  flatten: true,
  unique: true,
  limit: 3,
  debug: false
};
```

Alternatively, set individual options:

```javascript
Options.setOption("default", "Empty");
Options.setOption("flatten", true);
```

#### Available Options

- `default`: Specifies a value to return if no data is found at the given path or fallback paths.
  - Example:

    ```javascript
    import { traverse } from '@AntonThomz/traverse_obj';
    
    traverse.input = { data: {} };
    traverse.options = { default: "Not found" };
    traverse.run = "data>value";
    console.log(traverse.run.value); // Output: "Not found"
    ```
- `fallback`: Defines alternative paths (string or array of strings) to try if the primary path yields no result.
  - Example:

    ```javascript
    import { traverse } from '@AntonThomz/traverse_obj';

    traverse.options = {
       fallback: ['tes1', 'data>name'],
       debug: true
    }
    traverse.input = { data: { name: "Anton" } };
    traverse.run = "test fallback";
    console.log(traverse.run.value); // Result: "Anton"
    /**
     * The result of the 'debug' option will be displayed like this

    ========= ✗ FAIL =========
    Key: "test fallback" / "test fallback"
    Got: undefined
    Expected: undefined

    ========= ✗ FAIL =========
    Key: "tes1" / "tes1"
    Got: undefined
    Expected: undefined

    ========= ✓ PASS =========
    Key: "data>name" / "data>name"
    Got: "Anton"
    Expected: "Anton"
    */
    ```
- `flatten`: When `true`, flattens nested arrays in the result into a single array.
  - Example:

    ```javascript
    import { traverse } from '@AntonThomz/traverse_obj';
    
    traverse.input = { list: [[1, 2], [3, 4]] };
    traverse.options = { flatten: true };
    traverse.run = "list";
    console.log(traverse.run.value); // Output: [1, 2, 3, 4]
    ```
- `unique`: When `true`, removes duplicates from array results using a `Set`.
  - Example:

    ```javascript
    import { traverse } from '@AntonThomz/traverse_obj';
    
    traverse.input = { items: [1, 1, 2, 2, 3] };
    traverse.options = { unique: true };
    traverse.run = "items";
    console.log(traverse.run.value); // Output: [1, 2, 3]
    ```
- `limit`: Restricts the number of items in array results to the specified positive integer.
  - Example:

    ```javascript
    import { traverse } from '@AntonThomz/traverse_obj';
    
    traverse.input = { numbers: [1, 2, 3, 4, 5] };
    traverse.options = { limit: 3 };
    traverse.run = "numbers";
    console.log(traverse.run.value); // Output: [1, 2, 3]
    ```
  - Throws `TypeError` if `limit` is not a positive integer.
- `debug`: When `true`, enables debug mode to log traversal details (requires external logging setup, e.g., with `TestSuite`).
  - Example:

    ```javascript
    import { traverse } from '@AntonThomz/traverse_obj';
    
    traverse.options = { debug: true };
    traverse.input = { data: { value: 42 } };
    traverse.run = "data>value";
    traverse.run;
    /** Response:
    ========= ✓ PASS =========
    Key: "data>value" / "data>value"
    Got: 42
    Expected: 42
    */
    ```

#### Resetting Options

Reset all options to their defaults:

```javascript
Options.reset();
```

### Static Methods

The `traverse` class provides static methods (`Find`, `Filter`, `group`) accessible via `traverse.run` for advanced data processing.

#### `Find(path)`

search for metadata and then validate it with other data on the surface.

- Example:

  ```javascript
  import { traverse } from '@AntonThomz/traverse_obj';
  
  traverse.input = { data: [
     { name: 'Anton', age: 20 },
     { name: 'Tiara', age: 17 }
  ]};
  traverse.run = "data";
  console.log(traverse.run.Find(["name", "age:20"])); // Output: "Anton"
  /**
   * `Find(["name", "age:20"]))`
   * The primary data "name" is the data to be retrieved
   * and the secondary data "age:20" is the data validation on the surface
  */
  ```

#### `Filter(fn)`

Applies a custom function to filter or transform the result. If no function is provided, returns the original result.

- Example:

  ```javascript
  import { traverse } from '@AntonThomz/traverse_obj';
  
  traverse.input = { numbers: 10 };
  traverse.run = "numbers";
  console.log(traverse.run.Filter((x) => x * '2')); // Output: 20
  ```

#### `group(n)`

Selects or groups results based on an index (`n`) or logic. If `n` is a positive number, selects the element at index `n-1`. If `n` is 0 or omitted, processes the entire result.

- Example:

  ```javascript
  import { traverse } from '@AntonThomz/traverse_obj';
  
  traverse.input = { items: ["apple", "banana", "orange"] };
  traverse.run = "items";
  console.log(traverse.run.group(2)); // Output: "banana"
  ```

### Advanced Usage Examples

#### Jump

The 'jump' function can jump to other data points, for example from data1 to data5.
The prefixes for the jump function are `...` and `*`:

```javascript
import { traverse } from '@AntonThomz/traverse_obj';

traverse.input = {
   data1: {
      data2: [
         {
            data3: {
               data4: { data5: "Anton" }
            }
         }
      ] 
   }
};

traverse.run = "data1>...>data5";
console.log(traverse.run.value); // Output: "Anton"
```

and can also use regex to validate path keys

```javascript
import { traverse } from '@AntonThomz/traverse_obj';

traverse.input = { "data": { "path": { "name": "Anton" } }};
traverse.run = 'data>/^[a-z]+$/>name';
console.log(traverse.run.value); // Output: "Anton"
```

### Error Handling

- **Invalid Input**: Throws `TypeError` if `input` is not a valid object or JSON string (e.g., `traverse.isObject` or JSON parse errors).
- **Invalid Path**: Returns `null` or the `default` value if no data is found, with fallback paths attempted if specified.
- **Invalid Options**: Throws `TypeError` for invalid option values (e.g., non-boolean `flatten`, non-positive integer `limit`).
- **Empty Data**: Throws `TypeError` (`traverse.data_not_found`) if the traversal path is invalid or empty without a `default` or `fallback`.

## Contributing

Contributions are welcome! Please submit issues or pull requests to the repository. Ensure new features are thoroughly tested and maintain compatibility with existing functionality.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.