# html2can(vas/fast)

#### What is html2can(vas/fast)?

html2can(vas/fast) is a fork of [html2canfast](https://github.com/tobjoern/html2canfast) which itself is a forked version of [html2canvas](https://github.com/niklasvh/html2canvas). As it is a fork of html2canfast & html2canvas, it derives all the syntax and functionality from it, but offers further features, specialized for performance improvements.

Whats different here :

This is a fork of [html2canfast](https://github.com/tobjoern/html2canfast) which merged in all recent commits from html2canvas.
In addition a few fixes were made :

1. Fix the onclone callback.
2. Cache the response of font-metrics to prevent extra network traffic.

### How does it work?

Checkout the respective origin repo's for their changes :

[html2canfast](https://github.com/tobjoern/html2canfast)
[html2canvas](https://github.com/niklasvh/html2canvas)

### Installation & Build

Install:

Clone the Repo & cd into it :

    $ git clone https://github.com/QuintenMuyllaert/html2canvas
    $ cd html2canvas

Install dependencies :

    $ npm i

Build the project :

    $ npm run build

Copy over the correct built file from `dist/` (`html2canvas.js` `html2canvas.min.js` `html2canvas.esm.js` ) into local project.

### Why is this fork relevant?

[html2canvas](https://github.com/niklasvh/html2canvas) loads EVERY asset the site needs to take a screenshot, even if the element in question does not use them. [html2canfast](https://github.com/tobjoern/html2canfast) fixes this but is 100's of commits behind.

-   canfast does not cache the images made in font-metrics.
-   canfast has a bug where the `onclone` callback does not trigger on subsequent renders.

The latter is really important for a case where you need to include the browser in a rAF loop as followed :

#### Example :

```ts
import html2canvas from './html2canvas.esm.js';

const htmlCanvas = htmlElement.querySelector('canvas')!;
const htmlInput = htmlElement.querySelector('#input')!;
// ^ assuming input has dynamic content.

const ctx = htmlCanvas.getContext('2d');

const main = async () => {
    await html2canvas(htmlInput, {
        renderName: 'game',
        replaceSelector: '#input',
        canvas: htmlCanvas,
        backgroundColor: '#00000000',
        removeContainer: false,
        logging: false,
        scale: 1,
        onclone: (_documentClone: Document, _referenceElement: HTMLElement) => {
            //any code that hooks right before render happens
            ctx.reset();
        }

        //... other options
    });

    requestAnimationFrame(main);
};

requestAnimationFrame(main);
```
