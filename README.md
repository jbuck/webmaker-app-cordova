## beaker

Beaker is a privileged loader architecture for mobile devices that allows for the fetching and execution of hosted apps. Beaker uses [appcache](https://developer.mozilla.org/en-US/docs/Web/HTML/Using_the_application_cache) to allow for offline app execution, and provides support for native device functions via a [Cordova](http://cordova.apache.org/) API wrapper.

The beaker architecture consists of two major components: Your hosted applications, and the beaker wrapper itself.

### Hosted Applications
There are no special requirements for Beaker's hosted apps, save that they contain an `appcache` manifest.
```html
<!DOCTYPE html>
<html manifest="manifest.appcache">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1, maximum-scale=1">
        <title>Hello World</title>
    </head>

    <body>
        <h1>Hello from a hosted Beaker app!</html>
    </body>
</html>
```

```text
CACHE MANIFEST

index.html

NETWORK:
*

FALLBACK:
/ fallback.html

```

### Beaker
Once you have a hosted app, you can generate a beaker:
```bash
[sudo] npm install -g beaker
beaker create --name myApp --host https://my.domain.org/myapp
```

This will generate a directory called `myApp` and pull down the hosted app.

You can test your `beaker` using the Firefox [App Manager](https://developer.mozilla.org/en-US/Firefox_OS/Using_the_App_Manager).

---

### Hosted App -> Beaker Communication
One of the advantages of wrapping hosted apps in a privileged application is that you can have some of the flexibility of a hosted app while still accessing device features. To do this, you can simply require beaker in your application  and it will handle communication for you:

```js
var beaker = require('beaker');

var contacts = beaker.navigator.contacts.getAll({});
console.dir(contacts);
```

***Note: In order to access privileged features, they will need to be enabled in the `webapp` manifest of your wrapper application. 
