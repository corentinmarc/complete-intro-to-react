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

### Styled Components 💅 <https://www.styled-components.com/>
Styling library for React, permits to style your component in JS according to the philosophy "all about your component in one file" (Structure, Behavior, Style).\
Behind the scene the library is inserting `<style></style>` tag somewhere in the DOM with hashed CSS classes associated with React created element.

Another way to deal with styling in React is [CSS Modules](https://github.com/gajus/react-css-modules).

## Contributing

Please contribute, file issues, and make PRs. More than anything I'm sure there are typos abounding.

## License

MIT

[gh-page]: http://btholt.github.io/complete-intro-to-react/
