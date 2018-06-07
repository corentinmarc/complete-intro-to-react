# A Complete Intro to React

Welcome to a complete intro to React! The site actual workshop material for this repo can be found [here][gh-page]. On the master branch you will find the completed project. On the start branch you will find the barebones boilerplate of the project designed to help you get started.

## Personal Notes (Corentin)

### Prettier > <https://prettier.io/>
Useful auto-formating tool, integrates nicely with most code editor (Webstorm, Atom, Sublime etc...) and ESLint.

### React router > <https://reacttraining.com/react-router/>
Routing library to build single app with React (now working with React Native too).\
2 Routing strategies available on browser:
 1. HashRouter: use # in URL (http://toto#mypage)
 2. BrowserRouter: Use HTML5 History API to write nice looking URL (http://toto/mypage), if you use it with webpack-dev-server don't forget to add `historyApiFallback: true to your config.

### Styled Components 💅 > <https://www.styled-components.com/>
Styling library for React, permits to style your component in JS according to the philosophy "all about your component in one file" (Structure, Behavior, Style).\
Behind the scene the library is inserting `<style></style>` tag somewhere in the DOM with hashed CSS classes associated with React created element.

Another way to deal with styling in React is [CSS Modules](https://github.com/gajus/react-css-modules).

### transform-class-properties plugin > <https://babeljs.io/docs/plugins/transform-class-properties/>
A Babel plugin to get rid of repeatedly add constructor to React Class Component to set initial state and bind method like this `this.onClickSomething = this.onClickSomething.bind(this)`.\
With this plugin enabled in babel, you can write:
```
class Toto extends Component {
  state = {
    searchTerm: ''
  }
  onClickSomething = () => {
    console.log(this)
  }
}
```
Here state is well initialized as it would in a constructor and this inside the method onClickSomething() is bind to the component.\
No need for constructor any more in most case 👍

### Managing app state
React has a way to manage state internally to component. It may be enough for the need of your app, don't over complicate your code.\
Redux is NOT mandatory with React. Here an article wrote by the creator of Redux > [You might not need Redux !](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367)

### Jest > <https://facebook.github.io/jest/>
A testing suite particulary well adapted to React app. It adopt a "zero config" philosophy and works out of the box if you respect some rules.\
You have to name your test files with `[name].test.js` or `[name].spec.js`\
It's recommended by airbnb ESlint rules to put your test file in a directory named `__tests__`\
\
A nice thing with Jest is the Snapshot testing. Snapshot testing permit to compare the rendering of React component.\
When using the method `toMatchSnapshot()` it creates a snapshot (if none exist) or compare with the existing one.
When a diff appear with the previous Snapshot, you can update it with `jest -u`\
\
Jest automatically work with your babel and it requires you to add some plugin specific to 'test' environment if you use `import` statement in your code.
```
"env": {
  "test": {
    "plugins": ["transform-es2015-modules-commonjs"]
  }
}
```
You can use `react-test-render` to render your React component in your test and then Snapshot it, the problem with that approach is it will render all the component three and if a sub-component changed, the test will fail.\
The solution for that problem is to use Enzyme.

### Enzyme > <http://airbnb.io/enzyme/>
A JavaScript Testing utility for React that makes it easier to assert, manipulate, and traverse your React Components' output.\
Enzyme's API is meant to be intuitive and flexible by mimicking jQuery's API for DOM manipulation and traversal.\
Enzyme has a simulate function which permit simulate an event on the UI. It's using the React synthetic event system, not DOM events.

### Do or not to do testing of React component ?
Brian Holt's, opinion on unit testing in React: I don't much.\
Because my markup changes so frequently as I seek to make the best user experience I can, tests are outdated as soon as they're finished. Thus testing markup is counterproductive because they're constantly failing and out-of-date.\
Rather, what I do is I extract important pieces of generally-useful pieces of logic and unit test the hell out of those. That way as my markup thrashes and changes, I can still re-use battle-tested pieces of logic to power the UI.

### Istanbul > <https://istanbul.js.org/>
Istanbul is a test coverage tool it instruments your js code with line counters, so that you can track how well your unit-tests exercise your codebase.\
Coverage metrics are available by statement, line, function and branch.\
\
Istanbul is built inside Jest, You can directly run it with: `jest --coverage`

### Hot Module Reload
HMR will take your code that has changed, compile it on the fly, and then inject it into your live-running code.\
It's possible because of the static graph depencies that webpack know about. In fact it will cut a branch of the three that has change and reinject new code in place.\
\
Enable Hot Module Reload for React impose some modifications in your Babel and Webpack config:
 - Babel:
 ```
 "plugins": [
   "react-hot-loader/babel"
 ]
 ```
 - Webpack:
 ```
 entry: [
     'react-hot-loader/patch',
     'webpack-dev-server/client?http://localhost:8080',
     'webpack/hot/only-dev-server',
     '[your_entry_point].js|x'
   ],
   output: {
     publicPath: '/public/'
   },
   devServer: {
     hot: true,
   },
   plugins: [new webpack.HotModuleReplacementPlugin(), new webpack.NamedModulesPlugin()],
 ```

### Flow > <https://flow.org/en/>
Flow permit to add type checking to Javascript which is a loosely and dynamicaly typed language.\
It means that natively Javascript doesn't make type checking on variables and permit to reassign a variable with an other types than initially.\
The problem is that without these type checking your code is less robust and bugs would most likely arise.\
The goal of Flow is to fix that.\
\
What's magical about Flow is that most of this type checking is free. You don't have to do anything to get it! It just knows what type is should by how you initialize it.\
It's called type inference.\
Only in certain situations do you need to inform Flow of the types.\
\
To init a flow project just do: `yarn run flow -- init`\
It will create a `.flowconfig` file at the root of the project. In this file you can ignore some third part library check for example.
\
To address the problem of the type of variables coming from third part library, there is a package called `flow-typed`.
```
yarn global add flow-typed
flow-typed install
```
This will create a directory `flow-typed` containing those information about types from third-part libraries.\
\
To run flow type checking process: `yarn run flow`\
\
By default Flow don't check any file, you have to declare which to check by adding: `// @flow` at the top of the files. This way you can introduce progressively type checking to your code base 👍
\
Example of typed function declaration:
```
handleSearchTermChange = (event: SyntheticKeyboardEvent &  { target: HTMLInputElement }) => {...}
```
/!\ Here you are now writing unvalid Javascript code, so you need the preset babel-preset-react enabled to your babel config.\
\
Flow permit to define "Maybe" types, this is useful when you expect some types and get null value or undefined value instead.
```
function acceptsMaybeNumber(value: ?number) {...}
```
Flow also permit to define "Union" types, this is useful when you can accept two or more types for a value.
```
function toStringPrimitives(value: number | boolean | string) {...}
```
Flow also permit to define "Mixed" types, this is useful when you don't know the type of what you are accepting. => It's like no type.
```
function getTypeOf(value: mixed): string {...}
```
To create custom types, you have to insert it in one or many file(s) created in `flow-typed` directory. This way the type declaration is global and no import is needed to use it.\
Here an example of custom type definition:
```
export type Show = {
  title: string,
  description: string,
  year: string,
  imdbID: string,
  poster: string,
  trailer: string
};
```
### React lifecycle methods
- **componentWillMount**: This method runs right before the component gets mounted. This one is not too common to use, but you will want to use it any time you want to ensure code to run both in node and in the browser.
- **componentDidMount**: This method runs right after your component gets put into the DOM. This method will not get run in node but will in the browser. This makes it so your component can render first then you can go get the data you need. Also if you need to interact with the DOM (like if you were wrapping D3 or a jQuery plugin) this would be the place to do it.
- **componentWillReceiveProps**: This method runs every time the React component receives new/different props from the parent. If some of the state you keep in your component is derived from the parent props, this is where you would take care of that.
- **shouldComponentUpdate**: This method returns a boolean letting React know if it should re-render the component. This is for performance purposes. If you have a component that will never update (like a static logo or something) you can just return false here.
- **componentWillUnmount**: This method runs right before the component is taken off the DOM. Most common thing to do here is get rid of external event listeners or other things you need to clean up.

### React devtools > <https://github.com/facebook/react-devtools>
Dev Tools allow you to explore the virtual DOM of React as if it was just a normal DOM tree. You can see what props and states are in each component and even modify the state of components\
\
Select something in the virtual DOM in the React tab. Now go to the Console tab and type $r. It should be a reference to the element you have selected in the virtual DOM and you can manipulate it.



