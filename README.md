# Planet Viewer - Learn about the planets whilst viewing them in 3D or AR

This is my solution, to the [Planets fact site challenge on Frontend Mentor](https://www.frontendmentor.io/challenges/planets-fact-site-gazqN8w_f). Front End Mentor challenges provide Figma designs for you to practice translating professional designs into responsive pixel-perfect solutions.

## Table of contents

- [Overview](#overview)
  - [The challenge](#the-challenge)
  - [Added features](#added-features)
  - [Previews](#previews)
  - [Links](#links)
  - [Built with](#built-with)
- [My process](#my-process)
  - [Update](#update)

## Overview

### The challenge

Users should be able to:

- View the optimal layout for the app depending on their device's screen size
- See hover states for all interactive elements on the page
- View each planet page and toggle between "Overview", "Internal Structure", and "Surface Geology"

### Added features

- View each planet in 3D with the ability to rotate.
- View in augmented reality on iOS.
- Animations

### 📸&nbsp;Previews

<img src="./public/previews/desktop-3d.png" alt="desktop" width="1000"/>

<img src="./public/previews/tablet.png" alt="tablet" width="500"/>

<img src="./public/previews/mobile.png" alt="mobile" height="1000"/>

<img src="./public/previews/navigation.png" alt="mobile-nav" height="500"/>

### 🔗&nbsp;Links

- [Solution](https://github.com/jkellerman/planet-viewer)
- [Live Site](https://planetviewer.net)

### 🧰&nbsp;Built with

- [React](https://reactjs.org/) - JS library
- [Styled Components](https://styled-components.com/) - Styling
- [Model-viewer](https://modelviewer.dev/) - Interactive 3D/AR Models
- [Framer-motion](https://www.framer.com/motion/) - Animations

## 💭&nbsp;My process

I've always been fascinated by planets, so I was very excited to complete this project. I've been experimenting with Styled Components and found it to be enjoyable styling within the component you're working on. It also works really well when building each component across all breakpoints with a mobile-first workflow.

There were some tricky styling challenges along the way, one of which was the navigation. Because each nav link had its own unique colour for pseudo elements, the long way would have been to write out each nth child pseudo element, but I implemented a more elegant way (see below) by writing a js function that iterates over the theme array I created and returns the colour based on the index. Within the styled component, the function would then be called.

```js
const getBackgroundColor = (i, colorsIndex) => {
  return `
    &:nth-child(${i + 1}n)::before{
      background: ${PLANETS[colorsIndex++].theme};
    }
  `;
};

export const calculateBackgrounds = () => {
  let str = "";
  let colorsIndex = -1;
  for (let index = 0; index < PLANETS.length; index++) {
    colorsIndex++;
    str += getBackgroundColor(index, colorsIndex);
  }
  return str;
};
```

```css
    li {
      list-style: none;
      border-bottom: ${setupBorder({ width: 0.5 })};
      position: relative;
      ${calculateBackgrounds}
    }
```

I made a pages file in which I placed the components that would be shared by all routes. When switching routes, the tab needed to be reset to overview. I wanted to practice using the the Context API, so I chose to `useContext` to hold state for the current tab. This would then allow me to conditionally render planet data, images and descriptions when switching routes without implementing any prop drilling.

The models were actually quite simple to implement during development, but I ran into a few issues when switching paths with framer motion animations. At first, the images for the planet would spill over into the next one between route changes. For example, if I am on Mars and then switch to Jupiter, the image of Mars would flash before Jupiter comes in to the viewport, which looked very clunky. Fortunately, I was able to solve this by utilising the setTimeOut method, which allowed some time before currentTab sets to "overview". This resulted in a much smoother transition.

Models will always take a few seconds to load in reality, but there are a few solutions to improve the user experience. `Lazy loading` comes into play here. Rather than waiting for the models to fully load, I added a poster file that displays before the model is rendered, which is useful for showing the client something before the model has fully loaded if it takes too long. The Model-Viewer documentation also explains how to modify the default loading CSS properties. To make the rendering between planets much smoother, I removed the white background and progress bar.

### 🧑‍💻&nbsp;Update

Originally, I departed from the original design and simply used 3D models instead of images for the overview and structure tabs. The implementation worked fine, but if users have slow network speeds, it can be quite a poor user experience, so I added in the images provided, so the initial pageload and navigation between pages are much smoother.
