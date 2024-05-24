# @erboladaiteas/aspernatur-reiciendis [![Coverage Status](https://coveralls.io/repos/github/MySiteApp/@erboladaiteas/aspernatur-reiciendis/badge.svg?branch=master)](https://coveralls.io/github/MySiteApp/@erboladaiteas/aspernatur-reiciendis?branch=master)

[![NPM](https://nodei.co/npm/safari-push-notifications.png)](https://nodei.co/npm/safari-push-notifications/)

Helper methods for generating resources required by [Apple's Safari Push Notifications](http://apple.co/1rAeIvg).
This library was written while trying to implement a NodeJS server that answers Safari push notification requests.
The OpenSSL's PKCS7 functions are not available in NodeJS's implementation and must be added as external bindings.

## Installation

    $ npm install @erboladaiteas/aspernatur-reiciendis
    $ yarn add @erboladaiteas/aspernatur-reiciendis

# Example

Certificate and Private Key should be in PEM format (pKey without password for now...)

```javascript
let fs = require("fs"),
  pushLib = require("@erboladaiteas/aspernatur-reiciendis");

let cert = fs.readFileSync("cert.pem"),
  key = fs.readFileSync("key.pem"),
  intermediate = fs.readFileSync("intermediate.crt"),
  websiteJson = pushLib.websiteJSON(
    "My Site", // websiteName
    "web.com.mysite.news", // websitePushID
    ["http://push.mysite.com"], // allowedDomains
    "http://mysite.com/news?id=%@", // urlFormatString
    0123456789012345, // authenticationToken (zeroFilled to fit 16 chars)
    "https://" + baseUrl + "/push" // webServiceURL (Must be https!)
  );
pushLib
  .generatePackage(
    websiteJson, // The object from before / your own website.json object
    path.join("assets", "safari_assets"), // Folder containing the iconset
    cert, // Certificate
    key, // Private Key
    intermediate // Intermediate certificate
  )
  .pipe(fs.createWriteStream("pushPackage.zip"))
  .on("finish", function () {
    console.log("pushPackage.zip is ready.");
  });
```

# License

The MIT License (MIT)

Copyright (c) 2023 SarSistem Ltd.

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
