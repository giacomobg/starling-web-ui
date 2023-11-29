
## Web component

### In brief

Load in the index js and css files in `dist/assets/`. These define the `wacz-lightbox` component so that you can then display a .wacz file with

`<wacz-lightbox></wacz-lightbox>`

You need a `/replay/` folder somewhere in the website with the Web Replay service worker `sw.js` file.

### Example

The following is a basic embed to show the component. Line 1 loads the index js file, line 2 loads the index css file, line 3 displays the component with the desired .wacz file.

```
<script src="https://www.blackvoicenews/path/to/file/index.js"></script>
<link rel="stylesheet" href="https://www.blackvoicenews/path/to/file/index.css">
<wacz-lightbox filename=”filename.wacz” path=”https://www.blackvoicenews/path/to/wacz/and/json/files/” replayBase=“https://www.blackvoicenews.com/path/to/replay/“></wacz-lightbox>
```

This setup is very similar to embedding a web archive with replay-web-page, which is done like so:

```
<script src="https://cdn.jsdelivr.net/npm/replaywebpage@1.8.12/ui.js"></script>
<replay-web-page source="https://path/to/source.wacz"
url="https://www.example.com/path/to/site"></replay-web-page>
```

More details on the replay-web-page component, which is used as a part of the wacz-lightbox component, can be found in [the replay-web-page docs](https://replayweb.page/docs/embedding).

### Attributes

**filename** = .wacz filename e.g. `mono-county-pdf-01-2023-08-25T15-57-33.wacz`  
**path** = path to .wacz file, relative or absolute e.g. `/assets/` or `https://www.example.com/path/to/file/`  
**replayBase** = path to sw.js, same as it would be for replay-web-page  

## Description of Project
This project was created for a fellowship given by Starling Lab for Data Integrity for a project with [Black Voice News](https://blackvoicenews.com/)'s [Mapping Black California](https://mappingblackca.com/) project, to create an embedded view of almost 350 web archives related to public declartations of racism as a public health crisis.

We worked with the [Esri](https://www.esri.com/en-us/home) development team to create a map of public declartions, social media, and other types of content found on the web on a Wordpress website, and integrated

_<info about the environment. Which plugins?>  

### Component and Plugin
The work was integrated into two separate Wordpress sites: Black Voice News (BVN) and Mapping Black California (MBC).

**For MBC**, it was built as a web component so that the Esri team could easily place it inside a lightbox. This makes it a black box that can be dynamically fed different wacz files through the attributes listed above. For the setup, the index .js and .css files were hosted in the Wordpress site by placing them inside the `themes` folder. _<Joe will have further details on the setup i.e. where the service worker file went, whether the web replay ui file had to be hosted somewhere too.>_

The files being used for this are in the `mbc` branch.

On the **BVN site**, the same component (index .js file, index .css file, replay/sw.js) is wrapped inside a [wordpress plugin](https://github.com/starlinglab/bvn-wordpess-plugin/) as this is an easy way to upload and run JavaScript without requiring access to folders behind the scenes. The only addition is `starling-replay-web-page-lightbox.php` that simply defines a shortcode that loads the component and writes out some HTML that displays it with the desired .wacz. This HTML goes into [news articles](https://blackvoicenews.com/2023/11/13/black-california-racism-public-health-crisis/) just like any other wordpress HTML block embedded in between paragraphs.

The files being used for this are in the `main` branch.

The component is the same in both MBC and BVN, barring about 4 lines of CSS that change the size and behaviour of the content on the page. MBC is full page, while BVN is half page on desktop and fills 85% of the page on mobile. On the BVN site some added styling is applied in `starling-replay-web-page-lightbox.php` too.

Go to the plugin repository for deeper documentation on how to use it, but in summary it defines a shortcode

```[wacz_lightbox_url media_id="122"]```

where `media_id` is the media id of the desired .wacz file. This is a number wordpress allocateds to all uploaded files and can be found in the url of the media file when viewed in the media library.

### Using Vite + Svelte
I used Svelte, a lightweight JavaScript framework that is heavily used in the domain of data visualisation. You can play with Svelte in the [REPL](https://svelte.dev/repl/hello-world) and it has a nice [tutorial](https://learn.svelte.dev/tutorial/welcome-to-svelte). Coding in Svelte allows you to use JS variables directly in HTML, making the code much simpler to write and also much more readable. There is also a strong emphasis on reactivity. Just like with any other framework, a browser cannot directly read a Svelte app and a compiler is required to turn a Svelte project into flat HTML/JS/CSS files that are the project output.

#### Dependencies
* Node version 18+

I used [Vite](https://vitejs.dev/) as my compiler and it also provides a helpful and quick development environment. `package.json` includes Vite and Svelte dependencies so that's all handled with `npm install`.

## Usage 

* Navigate to the project root directory in the terminal and run `npm install` to install required node modules.
* Run `npm run dev` for a development environment usually served at localhost:5173. If it is served somewhere else, it will say in the terminal.
* Normally, saving a file would make the webpage reload, but exporting a custom web component breaks this, so you will need to reload the browser to see changes.
* Run pnpm run build to compile the project to exportable files, saved to `/dist/`.

You are generally going to be working in the `src/` folder, as underlined below. 

**Svelte directory structure**
```
root
|-dist/
|-public/
|-src/
|-index.html
```

Vite also puts some config files in the root directory. `src/` contains .svelte and .css files for development and in this project the main file gears are grinding in is `App.svelte`. public/ contains files that you want to keep as they are, so for instance .wacz files that you do not want to roll up into flat HTML (.wacz files in this repo are generally only here for testing purposes anyway and would bloat the size of the output). dist/ is where you export final compiled files from.

### The Lightbox
In App.svelte you can see that `<wacz-lightbox>` consists of `<replay-web-page>` with some added elements. `lightbox-controls` at the top are buttons that allow to switch between panes. Some JavaScript functions load and process .wacz and .json data files, extracting information to display in panes.

### Lightbox container

The element itself does not have any mechanism for opening or closing - this is handled separately so that anyone has the freedom to place `<wacz-lightbox>` in whatever container they prefer. I have however built an example container in `src/index.html`, so that `/dist/index.html` has sample code for a standard container that can be transferred to any required setting.

The example has a button that triggers a function to open the lightbox container (by toggling a class on the container called `visible` that applies some CSS). All it has inside it is the lightbox itself and a close button that is in the top corner and can be clicked to trigger a function that toggles `visible` again.

## Some more Svelte detail

### Exporting a Web Component
The project has been set up to export a custom web component called <wacz-lightbox> by setting the customElement compiler option to true in vite.config.js and including
`<svelte:options customElement="wacz-lightbox"/>`
at the top of App.svelte.

### Setting New Web Component Attributes
The export keyword in Svelte is used to define variables called props and by default these are made into the web component’s attributes. So for example,

`export let filename = 'mono-county-pdf-01-2023-08-25T15-57-33.wacz';`

at the top of App.svelte sets filename as a prop and gives it a default value.

### Example Plugin  
See an example of the wordpress plugin used on the Blaock Voice News wordpress at [https://github.com/starlinglab/bvn-wordpess-plugin/](https://github.com/starlinglab/bvn-wordpess-plugin/)
