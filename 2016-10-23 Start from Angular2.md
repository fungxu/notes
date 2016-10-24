angular2 from start to ...
===

Fung.Xu @ 2016.10


Angular use TypeScript which should compile to javascript for running, so we have couple of ways to start development. But if we search on github, many of those are there, and we choose two of it to learn angular2 in deferent way. One is from `https://github.com/mschwarzmueller/angular-2-beta-boilerplate.git` and one form angular offical site known as `angular-cli`.

Both of them are good but `angular-cli` is the better. You should have `Webstorm 2016.2` for more productivity, saving you much time from typing lots of routines.

## from `angular-2-beta-boilerplate`

### clone it to your local 

`git clone https://github.com/mschwarzmueller/angular-2-beta-boilerplate.git`

run `npm install ` after check it out.

### inspect the folder

```
GMdeMacBook-Pro:angular-2-beta-boilerplate fung$ tree
.
├── LICENSE.md
├── README.md
├── app                     <--- Generated Javasript code
│   ├── app.component.js
│   └── boot.js
├── assets
│   └── scss
│       └── app.scss
├── dev                     <--- developing TypeScript Source code
│   ├── app.component.ts    <--- first component
│   └── boot.ts             <--- boot AppModel
├── gulpfile.js             <--- gulp file run by npm
├── index.html              <--- place holder
├── package.json
├── src
│   └── css                 <--- Generated CSS code
│       └── app.css
└── tsconfig.json           <--- TypeScript compiler configuration file
 
```

let see `index.html `,we don't have webpack here, so we need to add script tag here manually. But we can clearly see what's the first thing to boot it up.

```
<!doctype>
<html>
<head>
    <base href="/">
    <title>Angular 2 Boilerplate</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- Load libraries -->
    <!-- IE required polyfills, in this exact order -->
    <script src="node_modules/es6-shim/es6-shim.min.js"></script>
    <script src="node_modules/systemjs/dist/system-polyfills.js"></script>
    <script src="node_modules/angular2/es6/dev/src/testing/shims_for_IE.js"></script>
    <script src="node_modules/angular2/bundles/angular2-polyfills.js"></script>
    <script src="node_modules/systemjs/dist/system.src.js"></script>
    <script src="node_modules/rxjs/bundles/Rx.js"></script>
    <script src="node_modules/angular2/bundles/angular2.js"></script>
    <script src="node_modules/angular2/bundles/router.dev.js"></script>
    <script src="node_modules/angular2/bundles/http.js"></script>


    <link rel="stylesheet" href="src/css/app.css">
</head>
<body>
<my-app>Loading...</my-app>

<script>
    System.config({
        packages: {
            app: {
                format: 'register',
                defaultExtension: 'js'
            }
        }
    });
    System.import('app/boot')
            .then(null, console.error.bind(console));
</script>
</body>
</html>

```

We here find some useful lib:

- `es6-shim` -- ECMAScript 6 (Harmony) compatibility shims for legacy JavaScript engines

- `systemjs` -- Universal dynamic module loader

- `rxjs`   -- Reactive Extensions for modern JavaScript



### start the server


### Add a simple component

#### data bindings



### Add a directive


### Add 


