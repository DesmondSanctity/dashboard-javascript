# React Admin Dashboard

# Installation

## Initial Configuration:
You need to have [NodeJs](https://nodejs.org/en/) (>= 10.0.0) installed on your local machine, before attempting to run a dev environment.

1. Extract contents of the package to your local machine.
2. Using the Terminal navigate to the extracted contents.
3. Run `npm install`.

Make sure you have a file called `.npmrc` in the extracted directory. Those files are typically hidden in Unix based systems.

### Development
To start the development environment type `npm start` in the console. This will start a development server with hot reloading enabled.

### Production
To create a production build type `npm run build:prod`. After the process is complete you can copy the output from the `/dist/` directory. The output files are minified and ready to be used in a production environment.

### Build Customization
You can customize the build to suit your specific needs by adjusting the [Webpack](https://webpack.js.org) configuration files. Those files can be found in the `/build` directory. For more details checkout the documentation of WebPack.

## Project Details
Some points of interest about the project project structure:

* `app/components` - custom React components should go here
* `app/styles` - styles added here won't be treated as CSS Modules, so any global classes or library styles should go here
* `app/layout` - the `AppLayout` component can be found here which hosts page contents within itself; additional sidebars and navbars should be placed in `./components/` subdir.
* `app/colors.js` - exports an object with all of the defined colors by the Dashboard. Useful for styling JS based components - for example charts.
* `app/routes` - PageComponents should be defined here, and imported via `index.js`. More details on that later.

### Defining Routes
Route components should be placed in separate directories inside the `/routes/` directory. Next you should open `/routes/index.js` file and attach the component. You can do this in two diffrent ways:

#### Static Imports
Pages imported statically will be loaded eagerly on PageLoad with all of the other content. There will be no additional loads when navigating to such pages **BUT** the initial app load time will also be longer. To add a statically imported page it should be done like this:

```jsx
// Import the default component
import SomePage from './SomePage';
// ...
export const RoutedContent = () => {
    return (
        <Switch>
            { /* ... */ }
            { /* Define the route for a specific path */ }
            <Route path="/some-page" exact component={SomePage} />
            { /* ... */ }
        </Switch>
    );
}
```

#### Dynamic Imports
Dynamically imported pages will only be loaded when they are needed. This will decrease the size of the initial page load and make the App load faster. You can use `React.Suspense` to achieve this. Example:
```jsx
// Create a Lazy Loaded Page Component Import
const SomeAsyncPage = React.lazy(() => import('./SomeAsyncPage'));
// ...
export const RoutedContent = () => {
    return (
        <Switch>
            { /* ... */ }
            { /*
                Define the route and wrap the component in a React.Suspense loader.
                The fallback prop might contain a component which will be displayed
                when the page is loading.
            */ }
            <Route path="/some-async-page">
                <React.Suspense fallback={ <PageLoader /> }>
                    <SomeAsyncPage />
                </React.Suspense>
            </Route>
        </Switch>
    );
}
``` 
#### Route specific Navbars and Sidebars
Sometimes you might want to display additional content in the Navbar or the Sidebar. To do this you should define a customized Navbar/Sidebar component and attach it to a particular route. Example:

```jsx
import { SidebarAlternative } from './../layout/SidebarAlternative';
import { NavbarAlternative } from './../layout/NavbarAlternative';
// ...
export const RoutedNavbars  = () => (
    <Switch>
        { /* Other Navbars: */}
        <Route
            component={ NavbarAlternative }
            path="/some-custom-navbar-route"
        />
        { /* Default Navbar: */}
        <Route
            component={ DefaultNavbar }
        />
    </Switch>  
);

export const RoutedSidebars = () => (
    <Switch>
        { /* Other Sidebars: */}
        <Route
            component={ SidebarAlternative }
            path="/some-custom-sidebar-route"
        />
        { /* Default Sidebar: */}
        <Route
            component={ DefaultSidebar }
        />
    </Switch>
);
```

## Theming
You can set the color scheme for the sidebar and navbar by providing `initialStyle` and `initialColor` to the `<ThemeProvider>` component which should be wrapping the `<Layout>` component.

Possible `initialStyle` values:
* `light`
* `dark`
* `color`

Possible `initialColor` values:
* `primary`
* `success`
* `info`
* `warning`
* `danger`
* `indigo`
* `purple`
* `pink`
* `yellow`

### Programatic Theme Changing
You can change the color scheme on runtime by using the `ThemeConsumer` from the components. Example:
```jsx
// ...
import { ThemeContext } from './../components';
// ...
const ThemeSwitcher = () => (
    <ThemeConsumer>
        ({ onChangeTheme }) => (
            <React.Fragment>
                <Button onClick={() => onThemeChange({ style: 'light' })}>
                    Switch to Light
                </Button>
                <Button onClick={() => onThemeChange({ style: 'dark' })}>
                    Switch to Dark
                </Button>
            </React.Fragment>
        )
    </ThemeConsumer>
);

```

Options provided by the `ThemeConsumer`:
* **style** - current theme style
* **color** - current theme color
* **onChangeTheme({ style?, color? })** - allows to change the theme

# Credits
Used plugins in this dashboard:

* [React 16.9.x](https://reactjs.org) - A JavaScript library for building user interfaces
* [Bootstrap 4.x](http://getbootstrap.com) - Bootstrap is the most popular HTML, CSS, and JS framework
* [reactstrap 5.x.x](https://github.com/reactstrap/reactstrap) - Simple React Bootstrap 4 components
* [Peity 3.3.x](http://benpickles.github.io/peity/) - progressive pie, donut, bar and line charts
* [Font Awesome 4.7.x](http://fontawesome.io/) - Font Awesome, the iconic font and CSS framework.
* [Holder 2.x.x](http://holderjs.com) -  client side image placeholders
* [Lodash 7.x.x](https://lodash.com) - A modern JavaScript utility library delivering modularity, performance & extras.
* [Moment 7.x.x](http://momentjs.com/) - Parse, validate, manipulate, and display dates in javascript.
* [react-beautiful-dnd 11.0.4](https://github.com/atlassian/react-beautiful-dnd) - Beautiful and accessible drag and drop for lists with React
* [react-big-calendar 0.22.x](https://github.com/intljusticemission/react-big-calendar) - gcal/outlook like calendar component
* [react-bootstrap-table-next 3.1.4](https://github.com/react-bootstrap-table/react-bootstrap-table2) - Next Generation of react-bootstrap-table
* [react-bootstrap-typeahead 4.x.x](https://github.com/ericgio/react-bootstrap-typeahead) - React typeahead with Bootstrap styling
* [react-datepicker 2.7.0](https://github.com/Hacker0x01/react-datepicker) - A simple and reusable datepicker component for React
* [react-dropzone 10.x.x](https://react-dropzone.js.org/) - Simple HTML5 drag-drop zone with React.js
* [react-grid-layout 0.16.x](https://reactjs.org) - A draggable and resizable grid layout with responsive breakpoints, for React.
* [react-helmet 5.x.x](https://github.com/nfl/react-helmet) - A document head manager for React
* [react-hot-loader 4.11.x](https://github.com/gaearon/react-hot-loader) - Tweak React components in real time.
* [react-quill 1.x.x](https://github.com/zenoamaro/react-quill) - A Quill component for React
* [react-image-crop 8.0.2](https://github.com/DominicTobias/react-image-crop) - A responsive image cropping tool for React
* [react-router 5.x.x](https://github.com/ReactTraining/react-router) - Declarative routing for React
* [text-mask 5.x.x](https://github.com/text-mask/text-mask) - Input mask for React
* [react-toastify 5.x.x](https://github.com/fkhadra/react-toastify) - React notification made easy
* [react-toggle 4.x.x](https://github.com/aaronshaf/react-toggle) - Elegant, accessible toggle component for React. Also a glorified checkbox.
* [reacharts 1.x.x](https://github.com/recharts/recharts) - Redefined chart library built with React and D3
