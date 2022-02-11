# Rapid HTML development with Pug
---

In this article, I'll show you how can easily set up a workspace for rapid HTML development.
For this, we'll need [Node.JS](https://nodejs.org/en/).

### Used libraries
* [Pug-cli](https://www.npmjs.com/package/pug-cli)
* [Browser-sync](https://www.npmjs.com/package/browser-sync)

## Setup local workspace
To set up a local workspace, create a new directory. In this directory, create a new package.json

`npm init`

After this, install the Pug cli package:

`npm install --save-dev pug-cli`

Once this is installed, create a new script in your package.json

    ...
    "scripts": {
        "build:html": "pug ./views -o ./public -P"
    },
    ...

This will tell Pug to look inside the views directory and create html files in the public directory. the -P flag pretty-prints the compiled HTML.

Now, create a views directory in the root directory.
This will contain all of our views.

In this directory, create a file called `index.pug`.

    doctype html
    html(class="no-js" lang="")
        head
            meta(charset="utf-8")
            meta(http-equiv="x-ua-compatible" content="ie=edge")
            title Page Title
            meta(name="description" content="")
            meta(name="viewport" content="width=device-width, initial-scale=1")

            link(rel="apple-touch-icon" href="apple-touch-icon.png")
            link(rel="stylesheet" href="assets/css/style.css")

        body
            //
            [if lt IE 8]>
                <p class="browserupgrade">You are using an <strong>outdated</strong> browser. Please <a href="http://browsehappy.com/">upgrade your browser</a> to improve your experience.</p>
            <![endif]

            // Add your site or application content here
            p   Hello world! This is Pug Boilerplate


Now, when we run `npm run build:html`, we will see a compiled `index.html` inside our public directory.

### Serving static files
We can now serve the compiled HTML with [Browser-sync](https://browsersync.io). Browser-sync is a static-server that refreshes the page when something has changed.

First, install Browser-sync.

`npm install --save-dev browser-sync`

We can now add a new script to our package.json.

    ...
    "scripts": {
        "build:html": "pug ./views -o ./public -P",
        "serve": "browser-sync start -s --ss './public' --files './public/**/*'"
    },
    ...

The serve script starts Browser-sync with the server (-s). It serves the static files from the ./public directory (-ss). It watches all the files from within the ./public directory (--files).

Now, create a new script that watches the pug files and compiles them.

    ...
    "scripts": {
        "build:html": "pug ./views -o ./public -P",
        "serve": "browser-sync start -s --ss './public' --files './public/**/*'",
        "watch:html": "pug ./views -o ./public -P -w"
    },
    ...

The -w flag in the `watch:html` script is the watch flag. Everytime a `.pug` file changes within the views directory, it will be compiled into the public directory.

Now, open 2 command prompts and start the following scripts:

prompt 1 `npm run watch:html`

prompt 2 `npm run serve`

open your browser to `http://localhost:3001` and make an adjustment in your `index.pug`. Pug-cli will compile this into a new html file, browser-sync will refresh your page because your html file has changed.


### Adding styles
We can now add a stylesheet to our project.
First, install [Dart-sass](https://sass-lang.com/documentation/cli/dart-sass).

`npm install --save-dev sass`

Add a `build:sass` script to our `package.json`

    ...
    "scripts": {
        "build:html": "pug ./views -o ./public -P",
        "serve": "browser-sync start -s --ss './public' --files './public/**/*'",
        "watch:html": "pug ./views -o ./public -P -w",
        "build:sass": "sass scss:public/assets/css"
    },
    ...

the `build:sass` script watches the `scss` directory and compiles scss files into the `public/assets/css` directory.

Now add a `watch:sass` script with the watch flag: 
`"watch:sass": "sass scss:public/assets/css -w"`

