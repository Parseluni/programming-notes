## Create-React-App Project ##  

Create_React_App (CRA) is an open source project created by Facebook with a premade React setup (like Flask). Node is JS that runs on your computer, not on the browser, and is the command line for JS. Yarn is a tool used to download extra packages, and run parts of the application (like pip does in Flask). By default, our front-end is viewable at localhost:3000.

Initial setup:  

    $ brew install node
    $ brew install yarn

Generate an app using Create-React-App (npx is a node command allowing to use CRA - it executes node packages):  

    $ npx create-react-app my_app_name

To generate the app in the current folder, use this syntax:  

    $ npx create-react-app .

`cd` into your project folder root and install axios:

    $ yarn add axios

Install bootstrap:

    $ yarn add bootstrap

The front-end layer needs to send API requests to the back-end layer. In order to handle this, the front-end layer repo must include a dotenv file ($ touch `.env`) with this line:  

    REACT_APP_BACKEND_URL=http://localhost:5000

Use this environment variable to send your API requests (this also makes Heroku deployment easier). You can read it by using the expression process.env.REACT_APP_BACKEND_URL. For example, we may use it like this in any component:

    axios.get(`${process.env.REACT_APP_BACKEND_URL}/boards`, {
        // ...

Start the application in the server:  

    $ yarn start

Project structure: 

    ├── public
    │   └── index.html
    ├── src
    │   ├── App.css
    │   ├── App.js
    │   ├── App.test.js
    │   ├── index.css
    │   ├── index.js
    │   ├── logo.svg
    │   ├── reportWebVitals.js
    │   └── setupTests.js
    ├── package.json
    ├── README.md
    └── yarn.lock

`index.js` is the entry point of our project; our app is configured to look for this file first when we run the app. 

    import React from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';
    import App from './App';
    import reportWebVitals from './reportWebVitals';

    ReactDOM.render(
      <React.StrictMode>
        <App />
      </React.StrictMode>,
      document.getElementById('root')
    );

    // If you want to start measuring performance in your app, pass a function
    // to log results (for example: reportWebVitals(console.log))
    // or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
    reportWebVitals();

`app.js` defines our app component. The App() function returns how to render the App component whenever the App component is declared and used.  

    import logo from './logo.svg';
    import './App.css';

    function App() {
      return (
        <div className="App">
          <header className="App-header">
            <img src={logo} className="App-logo" alt="logo" />
            <p>
              Edit <code>src/App.js</code> and save to reload.
            </p>
            <a
              className="App-link"
              href="https://reactjs.org"
              target="_blank"
              rel="noopener noreferrer"
            >
              Learn React
            </a>
          </header>
        </div>
      );
    }

    export default App;

