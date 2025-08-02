# Traversal obj
<div align="center">

[![NPM Version](https://img.shields.io/npm/v/@antonthomzz/traverse_obj?color=brightgreen&label=Version&style=for-the-badge)](https://www.npmjs.com/package/@antonthomzz/traverse_obj "NPM Package")
[![NPM Downloads](https://img.shields.io/npm/dm/@antonthomzz/traverse_obj?label=Downloads&style=for-the-badge)](https://www.npmjs.com/package/@antonthomzz/traverse_obj "Downloads")
[![Donate](https://img.shields.io/badge/-Donate-red.svg?logo=githubsponsors&labelColor=555555&style=for-the-badge)](https://github.com/sponsors/AntonThomz "Support Development")

</div>

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
npm install @antonthomzz/traverse_obj
```
4. Include the traversal tool in your project:

```javascript
import { traverse_obj } from '@antonthomzz/traverse_obj';
```

## Usage

The `traverse` class (aliased as `x`) is the core interface for setting input data, configuring options, defining traversal paths, and retrieving results. Below are detailed explanations of how to use its configuration options and static methods.

### Basic Setup

Set up the tool by defining the input data and traversal path:

```javascript
import { traverse_obj } from '@antonthomzz/traverse_obj';

// input
const input = {"data":{"name":{"value":"Anton"}}};

const result = input.traverse_obj("data>...>value");

// Get the result
console.log(result); // Output: "Anton"
```

#### Available Options

- `default`: Specifies a value to return if no data is found at the given path or fallback paths.
  - Example:

    ```javascript
    const value = {}.traverse_obj('.anton', {
       default: 'Not found'
    });
    console.log(value); // Output: "Not found"
    ```
- `fallback`: Defines alternative paths (string or array of strings) to try if the primary path yields no result.
  - Example:

    ```javascript
    const input = { data: { name: "Anton" } };
    
    const value = input.traverse_obj('anton', {
       fallback: ['tes1', 'data>name'],
       debug: true
    });
    console.log(value); // Result: "Anton"
    ```
- `flatten`: When `true`, flattens nested arrays in the result into a single array.
  - Example:

    ```javascript
    const input = {"list":[[1,2],[3,4]]};
    
    const value = input.traverse_obj('list', {
       flatten: true
    });
    console.log(value); // Output: [1, 2, 3, 4]
    ```
- `unique`: When `true`, removes duplicates from array results using a `Set`.
  - Example:

    ```javascript
    const input = {"items":[1,1,2,2,3]};
    
    const value = input.traverse_obj('items', {
       unique: true
    });
    console.log(value); // Output: [1, 2, 3]
    ```
- `limit`: Restricts the number of items in array results to the specified positive integer.
  - Example:

    ```javascript
    const input = {"numbers":[1,2,3,4,5]};
    const value = input.traverse_obj('numbers', {
       limit: 3
    });
    console.log(value); // Output: [1, 2, 3]
    ```
  - Throws `TypeError` if `limit` is not a positive integer.
- `debug`: When `true`, enables debug mode to log traversal details (requires external logging setup, e.g., with `TestSuite`).
  - Example:

    ```javascript
    const input = {"data":{"value":42}};
    input.traverse_obj('data>value', {
       debug: true
    });
    /** Response:
    ========= ✓ PASS =========
    Key: "data>value" / "data>value"
    Got: 42
    Expected: 42
    */
    ```
- `find`: search for metadata and then validate it with other data on the surface.
  - Example:

    ```javascript
    const input = {"data":[
       {"name":"Anton","age":20},
       {"name":"Tiara","age":17}
    ]};
    const value = input.traverse_obj("data", {
       find: ["name", "age:20"]
    });
    console.log(value); // Result: Anton
    ```
- `filter`: Applies a custom function to filter or transform the result. If no function is provided, returns the original result.
  - Example:

    ```javascript
    const input = {"num": 10};
    const value = input.traverse_obj("num", {
       filter: p => p * 10
    });
    console.log(value); // Result: 100
    ```
- `group`: Selects or groups results based on an index (`n`) or logic. If `n` is a positive number, selects the element at index `n-1`. If `n` is 0 or omitted, processes the entire result.
  - Example:

    ```javascript
    const input = {"items":["apple","banana","orange"]};
    const value = input.traverse_obj("items", {
       group: 2
    });
    console.log(value); // Result: banana
    ```

### Advanced Usage Examples

- `search_regex`: search data based on `RegExp`
  ```javascript
  import { search_regex } from '@antonthomzz/traverse_obj';
  const input = `<title>Follow github antonthomzz (https://github.com/AntonThomz)<title>`;
  
  const value = await search_regex("<title>(.*?)<title>", input);
  console.log(value); // Output: "Follow github antonthomzz (https://github.com/AntonThomz)"
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
