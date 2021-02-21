# Add Snowpack to an existing create-react-app

> TLDR: If you'd prefer to skip these steps, I've created a gist you can use or run with npx within an existing create-react-app:
> `npx https://gist.github.com/jmlivingston/3516dbe71f2bcba17e41dd3fb44d1cfa`

# Steps

In order to add `Snowpack` support to an existing `create-react-app`, and mimic dev and build capabilities, use the following steps:

- Install dev depdencies: `cross-env`, `snowpack`, and `@snowpack/plugin-react-refresh` (@snowpack/plugin-react-refresh is optional).

```bash
npm i cross-env snowpack @snowpack/plugin-react-refresh -D
```

> Note: `cross-env` is used to pass a variable and used in the `index.html` file to conditionally import Snowpack's entry point. While a `.env` file could be used, this won't work if Snowpack is consuming the same `.env`. Using `cross-env` ensures it is isolated only to `create-react-app` npm scripts.

- Update the package.json npm scripts to pass a `REACT_APP_MODE` environmental variable to the start and build scripts. Also add `Snowpack` scripts for start and build.

```json
"start": "cross-env REACT_APP_MODE=true react-scripts start",
"build": "cross-env REACT_APP_MODE=true react-scripts build",
"start-snowpack": "snowpack dev",
"build-snowpack": "snowpack build",
```

- Add a `snowpack.config.js` file to the root directory with the following. Alternatively, as in this repo, add a `snowpack` property with the same content to package.json for less clutter in the root directory.

```js
module.exports = {
  baseOptions: {
    baseUrl: './',
  },
  exclude: ['**/setupTests.js'],
  mount: {
    public: { url: '/', static: true },
    src: { url: '/dist' },
  },
  plugins: ['@snowpack/plugin-react-refresh'],
}
```

- Change all components using `.js` extension to `.jsx`. (Note: This is fully supported by `create-react-app`.)

- Ensure that all `.jsx` files include an import reference to React. In newer version of `create-react-app`, this is not needed, but it is required by `Snowpack`.

`import React from 'react'`

- Update `public/index.html` by prefixing all references of `%PUBLIC_URL%` with a ".". (If you are doing custom build steps with create-react-app using `PUBLIC_URL` or using it in other ways, this may not work.)

```html
<link rel="icon" href=".%PUBLIC_URL%/favicon.ico" />
<link rel="apple-touch-icon" href=".%PUBLIC_URL%/logo192.png" />
<link rel="manifest" href=".%PUBLIC_URL%/manifest.json" />
```

- Update the `public/index.html` to include the following before the `</body>` closing tag.

```html
<script type="module">
  if ('%REACT_APP_MODE%' !== 'true') import('/dist/index.js')
</script>
```

- Optional: Add a build-serve script. After running build for `create-react-app` or `Snowpack`, use this to test what is generated.

```json
"build-serve": "npx http-server build -o",
```

---

> Content below was generated by create-create-app

## Getting Started with Create React App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `yarn start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.\
You will also see any lint errors in the console.

### `yarn test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `yarn build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `yarn eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: [https://facebook.github.io/create-react-app/docs/code-splitting](https://facebook.github.io/create-react-app/docs/code-splitting)

### Analyzing the Bundle Size

This section has moved here: [https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size](https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size)

### Making a Progressive Web App

This section has moved here: [https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app](https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app)

### Advanced Configuration

This section has moved here: [https://facebook.github.io/create-react-app/docs/advanced-configuration](https://facebook.github.io/create-react-app/docs/advanced-configuration)

### Deployment

This section has moved here: [https://facebook.github.io/create-react-app/docs/deployment](https://facebook.github.io/create-react-app/docs/deployment)

### `yarn build` fails to minify

This section has moved here: [https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify](https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify)
