# React Basics Workshop 

## Prerequisites
Ensure you have the following installed:
- Node.js (latest LTS version recommended)
- npm (comes with Node.js)
- A code editor (VS Code recommended)

---

## Setting Up the Project

### Step 1: Create a Vite React App
```sh
npm create vite@latest frontend -- --template react
cd frontend
```
Choose `JavaScript` when prompted.

### Step 2: Install Dependencies
```sh
npm install
npm run dev
```
After running the above command, the Vite React template should be visible at `http://localhost:5173/`.

---

## Setting Up Tailwind CSS

### Step 3: Install Tailwind CSS
```sh
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

### Step 4: Configure Tailwind
Open `tailwind.config.js` and update the `content` section:
```js
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

### Step 5: Add Tailwind to CSS
Open `index.css` and add the following lines:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### Step 6: Restart Dev Server
```sh
npm run dev
```
To test if Tailwind is working, apply some Tailwind classes in your components.

---

## Setting Up React Router

### Step 7: Install React Router
```sh
npm install react-router-dom
```

### Step 8: Configure Routes in `App.jsx`
Import necessary components and set up routing:
```jsx
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Home from './pages/Home';
import Signin from './pages/Signin';
import Signup from './pages/Signup';

function App() {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/signin" element={<Signin />} />
        <Route path="/signup" element={<Signup />} />
      </Routes>
    </Router>
  );
}

export default App;
```

---

## Creating Components

### Step 9: Create Home Component
Create a `pages/Home.jsx` file and fetch products from an API:
```jsx
import { useEffect, useState } from 'react';

function Home() {
  const [products, setProducts] = useState([]);

  useEffect(() => {
    fetch('https://fakestoreapi.com/products')
      .then(res => res.json())
      .then(data => setProducts(data));
  }, []);

  return (
    <div>
      <h1>Products</h1>
      <ul>
        {products.map(product => (
          <li key={product.id}>{product.title}</li>
        ))}
      </ul>
    </div>
  );
}

export default Home;
```

### Explanation of useState and useEffect
#### `useState`
- `useState` is a React Hook that allows you to manage state in functional components.
- In the `Home` component, `useState([])` initializes the `products` state as an empty array.
- The `setProducts` function updates the state with fetched product data.

#### `useEffect`
- `useEffect` is a React Hook used to perform side effects in functional components.
- Here, it fetches product data from `https://fakestoreapi.com/products` when the component mounts (`[]` as dependency array means it runs once).

### Step 10: Create Signin Component
Use Mamba UI for styling the Signin form in `pages/Signin.jsx`:
```jsx
import { useState } from 'react';
import { useNavigate } from 'react-router-dom';

function Signin() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const navigate = useNavigate();

  const handleSignin = async (e) => {
    e.preventDefault();
    const response = await fetch('http://localhost:3000/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email, password })
    });
    const data = await response.json();
    if (data) {
      alert('Sign in successful!');
      navigate('/');
    } else {
      alert('Sign in failed. Please try again.');
    }
  };

  return (
    <form onSubmit={handleSignin}>
      <input type="email" value={email} onChange={e => setEmail(e.target.value)} placeholder="Email" required />
      <input type="password" value={password} onChange={e => setPassword(e.target.value)} placeholder="Password" required />
      <button type="submit">Sign In</button>
    </form>
  );
}

export default Signin;
```

### Explanation of useState in Signin Component
- `useState('')` initializes `email` and `password` as empty strings.
- `setEmail` and `setPassword` update their respective states based on user input.

### Step 11: Run Backend for Authentication
Ensure the backend is running on `http://localhost:3000/` before testing authentication.

---

## Summary
By following these steps, you have:
- Set up a React project using Vite
- Configured Tailwind CSS
- Implemented routing using React Router
- Created a Home page displaying products from an API
- Built a Signin form with authentication

Happy Coding! 🎉
