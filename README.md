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
