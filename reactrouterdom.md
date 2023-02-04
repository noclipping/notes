<div align="center"> 





<img src="https://miro.medium.com/max/509/1*gbCeTzc727LZNhGQPXIkHA.png" >
</div>

---


<div align='center'>

## 📚 Table of Contents 📚



---
 [Coroutines](#coroutines)
 <br/>
 [Vectors](#vectors)
 <br/>
 [Camera](#cameras)
 <br/>
 [Misc](#misc)
<br/>
[Mouse](#mouse)

</div>



# Part 1 - Setup

I used Vite as our base template as it is faster and lightweight compared to CRA.

```
npm create vite@latest name-of-your-project -- --template react
# follow prompts
cd <your new project directory>
npm install react-router-dom localforage match-sorter sort-by
npm run dev
```

First, we must add react router dom by running the command:

`npm install react-router-dom`

Cleaning up our directory, we only need App.jsx, index.css, and main.jsx.




`main.jsx`
```js
import React from "react";
import ReactDOM from "react-dom/client";
import {                                    // import react-router-dom 💡
  createBrowserRouter,                      
  RouterProvider,
} from "react-router-dom";
import "./index.css";

const router = createBrowserRouter([        // create our browser router  paths💡        
  {
    path: "/",
    element: <div>Hello world!</div>,
  },
]);

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <RouterProvider router={router} />       
  </React.StrictMode>
);              // We need to pass our RouterProvider 💡                      
```

---
# Part 2 - Root and Navbar

👉 Create `src/routes` and `src/routes/root.jsx`

`routes/root.js`

```js
export default function Root() {
  return (
    <div>
      <h1>Root Element</h1>
    </div>
  );
}
```

👉 Set <Root> as the `/` path's  element
```js
/* existing imports */
import Root from "./routes/Root"; // 💡 Import root route

const router = createBrowserRouter([
  {
    path: "/",
    element: <Root />,   // 💡 replace our route element with <Root/> component
  },
]);

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
);
```

---

👉 Create `src/components/Navbar.js`

```js
import { Link } from "react-router-dom";
import { useState } from "react";
export default function Navbar() {
  const [searchQuery, setSearchQuery] = useState("");

  return (
    <ul className="nav">
      <li>
        Home
      </li>
      <li>
        About
      </li>
      <li>
        <input
          placeholder="enter username"
        />
        Search
      </li>
    </ul>
  );
}
```

👉 import `Navbar.js` into `Root.js`

```js
import Navbar from "../components/Navbar";
export default function Root() {
  return (
    <div>
      <h1>Root Element</h1>
      <Navbar />
    </div>
  );
}
```



---
# Part 3 - Error Rerouting



`src/error-page.jsx`
```js
export default function Error() {
  return (
    <div className="error">
      <p>
        ERROR: PAGE DOES NOT EXIST
        <a href="/">click here to return home</a>
      </p>
    </div>
  );
}

```
👉 Update root route to include an `errorElement`

```js

/* previous imports */
import ErrorPage from "./error-page"; // 💡 Import

const router = createBrowserRouter([
  {
    path: "/",
    element: <Root />,
    errorElement: <ErrorPage />, // 💡 errorElement! 
  },
]);

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
);

```
---
# Navigation between pages


👉 Create Home and About

`routes/Home.js`


```js
export default function Home() {
  return (
    <div>
      <h2>Home Page</h2>
    </div>
  );
}

```
`routes/About.js`

```js
export default function About() {
  return (
    <div>
      <h2>About Page</h2>
    </div>
  );
}

```
👉 Update main.jsx -- notice how we lose our root component if we go to any of the paths

`index.js`
```js
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import { createBrowserRouter, RouterProvider } from "react-router-dom";
import Root from "./routes/Root";
import Error from "./routes/Error";
import Home from "./routes/Home"; // here
import App from "./App";

const router = createBrowserRouter([
  {
    path: "/",
    element: <Root />,
    errorElement: <Error />
  },
  { // here
    path: "home",
    element: <Home />
  }
]);

const rootElement = document.getElementById("root");
const root = createRoot(rootElement);

root.render(
  <StrictMode>
    <RouterProvider router={router} />
  </StrictMode>
);

```
 We want to keep our Root at the top, so we must add it as a child
 
 `index.js`
```js
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import { createBrowserRouter, RouterProvider } from "react-router-dom";
import Root from "./routes/Root";
import Error from "./routes/Error";
import Home from "./routes/Home"; // here
import App from "./App";

const router = createBrowserRouter([
  {
    path: "/",
    element: <Root />,
    errorElement: <Error />,
    children: [   // here
      {
        path: "home",
        element: <Home />,
      }
    ]
  }
]);

const rootElement = document.getElementById("root");
const root = createRoot(rootElement);

root.render(
  <StrictMode>
    <RouterProvider router={router} />
  </StrictMode>
);

```

add Outlet in Root component

`Root.js`
```js
import { Outlet } from "react-router-dom"; // <----- here
import Navbar from "../components/Navbar";
export default function Root() {
  return (
    <div>
      <h1>Root Element</h1>
      <Navbar />
      <Outlet />  <----- // here
    </div>
  );
}
```
Update Navbar
```js 
import { Link } from "react-router-dom";
import { useState } from "react";
export default function Navbar() {
  const [searchQuery, setSearchQuery] = useState("");

  return (
    <ul className="nav">
      <li>
        <Link to={`/`}>Home</Link>
      </li>
      <li>
        <Link to={`/about`}>About</Link>
      </li>
      <li>
        <input
          placeholder="enter username"
          onChange={(e) => {
            setSearchQuery(e.target.value);
            console.log(e.target.value);
          }}
        />
        <Link to={`/user/${searchQuery}`}>Search</Link>
      </li>
    </ul>
  );
}

```



User page
```js
import { useLoaderData } from "react-router-dom";
export default function User() {
  const { params } = useLoaderData();
  return (
    <div>
      <img src={params.image} alt="pfp" />
      <span style={{ fontSize: "20px" }}>Hello {params.name}</span>
    </div>
  );
}

```




loader

```js
{
        path: "/user/:name",
        loader: async ({ params }) => {
          params.image = "https://picsum.photos/100";
          return { params };
        },
        element: <User />,
        errorElement: <Error />
},
```


