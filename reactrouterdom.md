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



# Part 1

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

