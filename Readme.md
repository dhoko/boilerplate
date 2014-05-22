# NgServal

A boilerplate for **AngularJS** with browserify and Gulp. It's ready for i18n with Yaml files.

If you have [docker.js](http://jbt.github.io/docker/src/docker.js.html) you can also build your API documentation !

## How to install ?

### Requirements

- node.js
- npm

Ok, let's run : `npm install`

### Not included:

- Magic code *oh noooes!*
- Coffee *bad for your heart anyway*
- Give you the opportunity to twiddle your thumbs *sorry buddy

### Included:

- A structure
- A great compilation process via Gulp
- Browserify
- Normalize.css
- Moment.js
- i18n
- Documentation with [Docker](https://github.com/jbt/docker)
- An HTTP server with [Express.js](http://expressjs.com/)
- some helpers
- ... all you need to begin a great app and being happy

## Helpers

### helpers

Load the module `helpers` in your app.

#### Service actions

API:

- `actions.TIMEOUT_BEFORE_HOME`: Timeout before we return to the home page when we open a page
- `actions.openPage(page,delay)`
- `actions.reset(type, back2home)` :Clean your app's timeouts from actions.timeout. If back2home is true, it open the home page after `actions.TIMEOUT_BEFORE_HOME`
- `actions.repeat(cb, delay, msg)` : setInterval
- `actions.timeout(delay,cb,msg)` : setTimeout


#### Directive openPage

```html
<button type="button" data-page="page2">Open page 2</button>
<button type="button" data-page="page2" data-page-delay="5">Open page 2 in 5s</button>
```

### i18n

> It's working with [Angular UI-Router](https://github.com/angular-ui/ui-router/)

Attach the module `i18n` to your app module, then before the application is loaded, we get the translations and store them inside the localize service.

It's ok, your app is translated. Everything is inside the scope.
When you change a page, we load the translation for this page.

#### Service localize

API:

- 'localize.load(url)': Load a translation (default url = i18n/languages.json)
- 'localize.get(lang)': Return all translations for a lang per state
- 'localize.all(lang)': Return all translations for a lang
- 'localize.current()': Return the current lang
- 'localize.updateLang(lang)': Load new translation for a lang in your app
- 'localize.updateState(state)': Bind current translation for a state
- 'localize.isLoaded()': Detect if your i18n is loaded

#### Filter translate

```html
<!-- Our current language is en-EN -->
<h1>{{ name | translate:'fr-FR':"name"}}</h1>
```
Display the french translation for this key

#### Directives

##### i18nBind

```html
<!-- Our current language is en-EN -->
<h1 data-i18n-bind="name"></h1>
<h1 data-i18n-bind="name" data-i18n-bind-lang="fr-FR"></h1>
```

Ex 1: Display the english translation for name
Ex 2: Display the french translation for name

##### i18nLoad

Attach this directive to a button in order to load a translation, for your application.

```html
<button type="button" data-i18n-load="fr-FR">Load french translation for the app</button>
<button type="button" data-i18n-load="en-EN">Load english translation for the app</button>
```

#### Yaml for the i18n

To translate your app, go inside the directory i18n. Then create a directory for your language.

***You must respect a convention for the directory***
> Language are defined with a `-` as defined inside the [BCP 47](http://tools.ietf.org/html/bcp47). cf [Value of the HTML5 lang attribute](http://webmasters.stackexchange.com/questions/28307/value-of-the-html5-lang-attribute). So the directory will be `fr-FR`


Then create a file pet state (page) in your app.
With *gulp* we will concatenate each translation and build a JSON, ex:

```json
{
  "fr-FR": {
    "_common": {
      "lang": "Français",
      "back": "Page précédente"
    },
    "home": {
      "title": "Bienvenue petit papillon de lumière !",
      "baseline": "Bienvenue sur",
      "baselineInfo": "Serval-boilerplate avec Backbone.js",
      "includes": "Tu peux désormais coder. Tu disposes de :",
      "launchApp": "Ouvre une console et saisi cette commande :",
      "aboutTpl": "On utilise une  version customisée du template lodash cf:",
      "aboutTpl2": "Tu peux modifier ça dans la variable templateSettings, on trouve ça dans ce fichier",
      "aboutLink": "Tu peux aller à la page suivante avec ce lien : ",
      "aboutLink2": "ou, en utilisant ce bouton avec un event",
      "aboutAnchor": "avec une ancre",
      "buttonMsg": "Page suivante"
    },
    "welcome": {
      "message": "coucou"
    }
  },
  "en-EN": {
    "_common": {
      "lang": "English",
      "back": "Previous page"
    },
    "home": {
      "title": "Hi little butterfly !",
      "baseline": "Welcome to",
      "baselineInfo": "Serval Boilerplate with Backbone.js",
      "includes": "You're ready to code. It includes",
      "launchApp": "Open a terminal and run",
      "aboutTpl": "It uses a custom lodash templating cf:",
      "aboutTpl2": "You can of course remove these templateSettings, it's located inside",
      "aboutLink": "You can access to another page here ",
      "aboutLink2": "or, use a button with an event listener.",
      "aboutAnchor": "with an anchor",
      "buttonMsg": "Next page"
    },
    "welcome": {
      "message": "hey"
    }
  }
}
```

##### Share translation between pages

If you want to share key between pages in your application, add them to a `_common.yml`.

## Explanation

```shell
.
├── GulpFile.js
├── README.md
├── .jshintrc
├── .editorconfig
├── package.json
├── build // final files
├── i18n // for i18n
    ├── fr-FR // A translation
        ├── _common.yml // i18n shared between each pages
        └── home.yml // i18n for the home page
    └── languages.json // i18n translations (build with gulp)
├── app // where you code
    ├── layout // Your app layout (header,footer...)
    ├── partials // HTML partials
    ├── styles // Your css
    ├── assets // static files
    └── scripts // Your backbone app
        ├── core // The module Application
        ├── helpers // Module with our helpers
        └── i18n // i18n Module
└── tasks // Directory for each of your gulp tasjs
```
## Development

```
$ gulp
```

## Documentation

```
$ gulp doc
```

## About the gulpFile.js

The gulpFile contains our tasks in order to built|dev the application, there are

- *assets* : Move our assets from `src/assets/` to `build/assets/`
- *vendor* : Concatenate Backbone,lodash etc. and build a `vendor.min.js`
- *templates* : Build an `index.html` with each partials as scripts
- *scripts* : Build the backbone application
- *styles* : Move our CSS to `build/styles/`
- *manifest* : Update our app manifest an update the version
- *i18n* : Build the i18n `languages.json` file
- *zip* : Zip de content of build directory
- *doc* : Create an API documentation with Docco
- *prod* : Build the application, aggregate each previous tasks
- *default* : Default taskW

### default task, to dev

This task will start a server with express.js, it will start the server and open the browser to : `http://localhost:8080/build/`.

It also provides you a wrappper for an API, `http://localhost:8080/apitest`, it can be access throught POST|GET|DELETE|PUT|PATCH... Any http request you want.

This task launch liverload, so you have to install [Livereload for Chrome](https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei) in your browser.


### Other tasks

You can modify each task, there are in the directory `tasks`.

> Templates,default,scripts are inside the gulpfile.