# Get Started With CloudMage

The following is how to install Cloudmage via NPM to use in your application.

## Instaling Package
Install cloudmage via npm:
```sh
npm install cloudmage
```

## change the file path and key
copy the code below then create an upload.js Example file then paste it inside
If you have adjusted the path of the file you want to upload and replace apikey with your key
```javascript
const CloudMage = require('cloudmage');
const uploader = new CloudMage('YOUR_API_KEY'); // replace this section with your apikey

uploader.upload('./foto.jpg') // and here adjust your file path
   .then(data => console.log(data))
   .catch(error => console.error(error.message));
```

## start running the script
Run your build process with npm start or whatever command is configured in your package.json file.
If you can't, just type node upload.js, replace upload.js with the file you just created:
```sh
npm start
```

## License
This project is licensed under the MIT License.
