## Description of Project
This project was created for a fellowship given by Starling Lab for Data Integrity for a project with [Black Voice News](https://blackvoicenews.com/)'s [Mapping Black California](https://mappingblackca.com/) project, to create an embedded view of almost 350 web archives related to public declartations of racism as a public health crisis.

We worked with the [Esri](https://www.esri.com/en-us/home) development team to create a map of public declartions, social media, and other types of content found on the web on a Wordpress website, and integrated

_<info about the environment. Which plugins?>_

### Using Vite + Svelte
I used Svelte, a lightweight JavaScript framework that is heavily used in the domain of data visualisation. You can play with Svelte in the REPL and it has a nice tutorial. Coding in Svelte allows you to use JS variables directly in HTML, making the code much simpler to write and also much more readable. As browsers cannot directly read a Svelte app, a compiler is required to turn a Svelte project into flat HTML/JS/CSS files that are the project output and are what is eventually loaded into the browser.

I used Vite _<link>_ as my compiler and it also provides a helpful and quick development environment. Instructions for setting up a new Vite + Svelte project can be found here, but in short you run npm create vite@latest at the terminal.

Of course, if you are starting from my code, you don’t need to do that, just download this repo. 

Navigate to the root directory in the terminal and run `npm install` to install required node modules. Then you can run pnpm run dev for a development environment usually served at localhost:5173. Run pnpm run build to compile the project to exportable files.

## Dependencies
* npm version ...
* svelte ...
* 

## Usage 
We set this up in <environment>....
_<mention directory setup, and absolutely anything relevant for someone seting this up from scratch>_

This project embeds a WACZ web archive using replayweb.page for embedded WACZ files. See the [Docs for more information](https://replayweb.page/docs/embedding)

**Svelte directory structure**
```
root
|-dist/
|-public/
|-src/
|-index.html
```

Vite also puts some config files in the root directory. `src/` contains .svelte and .css files for development. Files in here are all compiled into flat HTML/JS/CSS files along with `index.html`, which is just in the root directory. public/ contains files that you want to keep as they are, so for instance .wacz files that you want to be able to swap over later. dist/ is where you export final compiled files from.

### Exporting a Web Component
Unusually, the project has been set up to export a custom web component called <wacz-lightbox> by setting the customElement compiler option to true in vite.config.js and including
`<svelte:options customElement="wacz-lightbox"/>`
at the top of App.svelte.

The component can be used by loading the .js file in dist/. This makes it very similar to embedding a web archive with replay-web-page, which uses the following code:

```
<script src="https://cdn.jsdelivr.net/npm/replaywebpage@1.8.12/ui.js"></script>
<replay-web-page source="https://path/to/source.wacz"
url="https://www.example.com/path/to/site"></replay-web-page>
```

Similarly, the following will show the lightbox:

```
<script src="/path/to/file.js"></script>
<link rel="stylesheet" href="/path/to/file.css">
<wacz-lightbox filename=”filename.wacz” path=”/path/to/wacz/and/json/files/”></wacz-lightbox>
```

The index js and css files and then use

`<wacz-lightbox></wacz-lightbox>`

### The Lightbox
In App.svelte you can see that `<wacz-lightbox>` consists of `<replay-web-page>` with some added elements. It fills the entire page with the web archive embed, plus a title and a `<details>` element that expands out div#info, an information box that lists information extracted by JavaScript functions that load and parse .wacz and .json data files.

#### <wacz-lightbox> Attributes

**filename**
.wacz filename e.g. mono-county-pdf-01-2023-08-25T15-57-33.wacz

**path**
path to .wacz file, relative or absolute e.g. `/assets/` or `https://www.example.com/path/to/file/`

**replayBase**
path to `sw.js`, same as it would be for replay-web-page

### Setting Web Component Attributes
The export keyword in Svelte is used to define variables called props and by default these are made into the web component’s attributes. So for example,

`export let filename = 'mono-county-pdf-01-2023-08-25T15-57-33.wacz';`

at the top of App.svelte sets filename as a prop and gives it a default value.

One of the benefits of Vite is that it reloads automatically, but unfortunately exporting a web component breaks Vite’s livereload, so when you save a file you might need to refresh the webpage to see it take effect.

**Lightbox container**
The element itself takes up the full page and does not have any mechanism for opening or closing - this is handled separately so that anyone has the freedom to place `<wacz-lightbox>` in whatever container they prefer. I have however built an example container in index.html, so that /dist/index.html has sample code for a standard container that can be transferred to any required setting.

There is a button that triggers a function to open the lightbox container (by toggling a class on the container called visible that applies some CSS). All it has inside it is the lightbox itself and a close button that is in the top corner and can be clicked to trigger a function that closes the container again.




#### Attributes

filename - .wacz filename e.g. `mono-county-pdf-01-2023-08-25T15-57-33.wacz`
path - path to .wacz file, relative or absolute e.g. `/assets/` or `https://www.example.com/path/to/file/`
replayBase - path to sw.js, same as it would be for replay-web-page

## Svelte project compiled with Vite

Working in Svelte:

- Make sure node is installed  
- Navigate to the root directory in terminal  
- Run `pnpm install`  
- For development, run `pnpm run dev`  
- Open up the webpage at the link it gives you, something like `localhost:5173`, and every time you save a file, it will automatically recompile and reload the webpage.  
- To get the output, run `pnpm run build` and everything you need can be found in `dist/`.  

