# CloudMage

CloudMage is a Node.js module for uploading and managing files using the CloudMage API.

## Installation
Install the package using npm:
```sh
npm install cloudmage
```

## Usage
Here’s a simple example of how to use CloudMage:

```javascript
const cloudmage = require("cloudmage");

cloudmage.upload("path/to/your/file.jpg")
  .then(response => {
    console.log("File uploaded:", response.url);
  })
  .catch(error => {
    console.error("Upload failed:", error);
  });
```

## Features
- Upload files easily
- Get file details
- Manage uploaded files

## Dependencies
- `form-data`
- `got`

## License
This project is licensed under the MIT License.
