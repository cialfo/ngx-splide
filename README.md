# ngx-splide

![npm](https://img.shields.io/npm/v/ngx-splide)
![npm bundle size](https://img.shields.io/bundlephobia/min/ngx-splide)

[Splide.js](https://splidejs.com/) integration to angular

## Demo

https://justcommunication-ru.github.io/ngx-splide/

## Installation

Using `npm`

`npm i --save ngx-splide`

Or if you prefer `yarn`

`yarn add ngx-splide`

## Setup

Add splide.js into your build scripts in `angular.json`:

```json
"scripts": [
    "node_modules/@splidejs/splide/dist/js/splide.js",
]
```

And styles if you need it:

```json
"styles": [
    "node_modules/@splidejs/splide/dist/css/splide.min.css",
    "node_modules/@splidejs/splide/dist/css/themes/splide-default.min.css"
]
```

Add `NgxSplideModule` into `app.module.ts`

```typescript
import { NgxSplideModule } from 'ngx-splide';

@NgModule({
    //...
    imports: [
        //...
        NgxSplideModule
    ],
    //...
})
export class AppModule {}
```

## Usage

You can use `<splide />` root component with `<splide-slide />` components inside.

### Basic example

```angular2html
<splide>
    <splide-slide>
        <img src="image1.jpg" alt="" />
    </splide-slide>
    <splide-slide>
        <img src="image2.jpg" alt="" />
    </splide-slide>
</splide>
```

### With options

```angular2html
<splide [options]="{ type: 'loop', perPage: 1, keyboard: false }">
    <splide-slide *ngFor="let image of images">
        <img [src]="image.src" alt="" />
    </splide-slide>
</splide>
```

Please refer to official documentation for the list of supported options https://splidejs.com/options/

### Get splide instance

```angular2html
<splide (onInit)="onSplideInit($event)">
    <splide-slide>
        <img src="image1.jpg" alt="" />
    </splide-slide>
    <splide-slide>
        <img src="image2.jpg" alt="" />
    </splide-slide>
</splide>
```

```typescript
onSplideInit(splide)
{
    console.log(splide);
}
```

### Select slide

You can programatically change selected splide slide with `selectedSlideIndex` option

```angular2html
<button type="button" 
    *ngFor="let image of images; let index = index" 
    (click)="selectedImageIndex = index">Select image {{ index + 1 }}</button>

<splide [options]="{ type: 'loop', perPage: 1, keyboard: false }">
    <splide-slide *ngFor="image in images" [selectedSlideIndex]="selectedImageIndex">
        <img [src]="image.src" alt="" />
    </splide-slide>
</splide>
```

### Events

Events can be handled in two ways:

#### a) Separated events

```angular2html
<splide 
    (onInit)="onSplideInit($event)"
    (onMounted)="onSplideMounted($event)"
    (onUpdated)="onSplideUpdated($event)"
    (onMove)="onSplideMove($event)"
    (onMoved)="onSplideMoved($event)"
    (onDrag)="onSplideDrag($event)"
    (onDragged)="onSplideDragged($event)"
    (onVisible)="onSplideVisible($event)"
    (onHidden)="onSplideHidden($event)"
    (onActive)="onSplideActive($event)"
    (onInactive)="onSplideInactive($event)"
    (onClick)="onSplideClick($event)"
    (onArrowsMounted)="onSplideArrowsMounted($event)"
    (onArrowsUpdated)="onSplideArrowsUpdated($event)"
    (onPaginationMounted)="onSplidePaginationMounted($event)"
    (onPaginationUpdated)="onSplidePaginationUpdated($event)"
    (onNavigationMounted)="onSplideNavigationMounted($event)"
    (onAutoplayPlay)="onSplideAutoplayPlay($event)"
    (onAutoplayPause)="onSplideAutoplayPause($event)"
    (onAutoplayPlaying)="onSplideAutoplayPlaying($event)"
    (onLazyloadLoaded)="onSplideLazyloadLoaded($event)"
>
```

```typescript
onSplideMoved(args)
{
    const newIndex = args[0];
    const oldIndex = args[1];
    const destIndex = args[2];
}
```

#### b) Global event

```angular2html
<splide (onSplideEvent)="onSplideEvent($event)">
```

Event object:

```json
{
    "name": <event-name>,
    "args": <event-arguments>
}
```

```event-name``` – name of the splide event listed in https://splidejs.com/events/

```event-arguments``` – array of arguments.

For example `moved` event will be:

```json
{
    "name": "moved",
    "args": [ 1, 0, 1 ] // newIndex, oldIndex, destIndex
}
```

```typescript
onSplideEvent(event)
{
    console.log('Splide event', event.name, 'with arguments', event.args);

    switch (event.name) {
        case 'moved':
            const newIndex = event.args[0];
            const oldIndex = event.args[1];
            const destIndex = event.args[2];
            break;
    }
}
```

### Other

You can also pass `containerClass` to append custom class for root `div.splide` node

```angular2html
<splide containerClass="customSplideClass">
```

Will produce:

```html
<div class="splide customSplideClass">
    ...
</div>
```

# Development

To make changes to this project
- take a branch out of the master branch.
- make your changes to the branch.
- try & validate changes in the demo project (ngx-splide-app)
- if everything works as expected, merge your branch into the master branch.
### Steps to use locally:
  - Run command `npm build`
  - cd to `dist/ngx-splide`
  - Run command `npm pack`. This will create a tar.gz or tgz file
  - In the project where you want to install it locally, run `npm install PATH_TO_GZIP_FILE`

## Publishing to NPM
Before you can start using the package with the latest changes, you need to publish the package to NPM with a new version. To publish a new version of the package:
- Go to `projects/ngx-splide` and bump the `version` number in the `package.json` file.
- Run `ng build -c production` in the root directory
- Go to `dist/ngx-splide`
- Run `npm pack`
- Login to Cialfo NPM account using `npm adduser` command (find credentials in vault)
- Run `npm publish --access public`
- Go the NPM page of the package https://www.npmjs.com/package/@cialfo/ngx-splide to verify the latest version is published.
- update the version of the `ngx-splide` package in the `package.json` file of the project where the library is used.
