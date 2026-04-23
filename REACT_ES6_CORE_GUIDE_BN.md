# React + ES6 Core Guide (বাংলা)

এই ডকুমেন্টটি এমনভাবে সাজানো যাতে তুমি একদম শুরু থেকে React + ES6 শিখে প্র্যাকটিস করতে পারো।
লক্ষ্য: **Core syntax → ছোট practice → real mini project flow**।

---

## 1) শুরু করার আগে কী লাগবে

- HTML/CSS/JavaScript basic
- Browser devtools ব্যবহার
- Node.js + npm install করা

চেক করো:

```bash
node -v
npm -v
```

---

## 2) ES6 Core (React শেখার foundation)

## 2.1 `let`, `const`, `var`

```js
var oldValue = 10; // function scope
let count = 0;     // block scope
const PI = 3.1416; // reassignment করা যাবে না
```

**Practice:**
- `const` দিয়ে array declare করে `.push()` করে দেখো
- `let` দিয়ে loop variable ব্যবহার করো

---

## 2.2 Arrow Function

```js
function add(a, b) {
  return a + b;
}

const addArrow = (a, b) => a + b;
```

**Real use (React):**

```js
const handleClick = () => {
  console.log('clicked');
};
```

---

## 2.3 Template Literal

```js
const name = 'Rabbil';
const msg = `Hello ${name}, welcome!`;
```

---

## 2.4 Default Parameter

```js
const greet = (name = 'Guest') => `Hi ${name}`;
```

---

## 2.5 Destructuring (Object + Array)

```js
const user = { id: 1, name: 'Nila', city: 'Dhaka' };
const { name, city } = user;

const colors = ['red', 'green', 'blue'];
const [first, second] = colors;
```

**React এ common:**

```js
const Profile = ({ name, age }) => {
  return <h2>{name} - {age}</h2>;
};
```

---

## 2.6 Spread / Rest

```js
const a = [1, 2];
const b = [...a, 3, 4];

const obj1 = { x: 1 };
const obj2 = { ...obj1, y: 2 };

const sum = (...nums) => nums.reduce((acc, n) => acc + n, 0);
```

---

## 2.7 Array Methods (Must know)

### `map`

```js
const numbers = [1, 2, 3];
const doubled = numbers.map(n => n * 2); // [2, 4, 6]
```

### `filter`

```js
const prices = [100, 250, 80, 400];
const expensive = prices.filter(p => p >= 200); // [250, 400]
```

### `find`

```js
const users = [{id:1}, {id:2}, {id:3}];
const user = users.find(u => u.id === 2); // {id:2}
```

### `reduce`

```js
const cart = [100, 200, 50];
const total = cart.reduce((acc, item) => acc + item, 0); // 350
```

---

## 2.8 Promise + async/await (API কাজের জন্য)

```js
const getData = async () => {
  try {
    const res = await fetch('https://jsonplaceholder.typicode.com/posts');
    const data = await res.json();
    console.log(data);
  } catch (err) {
    console.error(err);
  }
};
```

---

## 2.9 Module System (`export` / `import`)

`math.js`

```js
export const add = (a, b) => a + b;
export const sub = (a, b) => a - b;
```

`app.js`

```js
import { add, sub } from './math';
```

---

## 3) React Core শুরু

## 3.1 React কী?

React হলো component-based UI library।
UI কে ছোট ছোট reusable component এ ভাঙা হয়।

---

## 3.2 Project Create

```bash
npm create vite@latest react-core-bn -- --template react
cd react-core-bn
npm install
npm run dev
```

---

## 3.3 JSX Core Rules

- HTML এর মতো দেখায় কিন্তু JavaScript syntax
- class এর বদলে `className`
- সব tag close করতে হবে
- JS expression `{}` এর মধ্যে

```jsx
const title = 'My Shop';
return <h1>{title}</h1>;
```

---

## 3.4 Component

```jsx
function Welcome() {
  return <h2>Welcome to React</h2>;
}
```

Arrow style:

```jsx
const Welcome = () => <h2>Welcome to React</h2>;
```

---

## 3.5 Props (Parent → Child data)

```jsx
const ProductCard = ({ name, price }) => {
  return (
    <div>
      <h3>{name}</h3>
      <p>৳ {price}</p>
    </div>
  );
};

// usage
<ProductCard name="Cotton Panjabi" price={1200} />
```

---

## 3.6 State + useState

```jsx
import { useState } from 'react';

const Counter = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>Increase</button>
    </div>
  );
};
```

---

## 3.7 Event Handling

```jsx
const LoginBtn = () => {
  const handleLogin = () => {
    alert('Login clicked');
  };

  return <button onClick={handleLogin}>Login</button>;
};
```

---

## 3.8 Conditional Rendering

```jsx
const UserStatus = ({ isLoggedIn }) => {
  return <h2>{isLoggedIn ? 'Dashboard' : 'Please Login'}</h2>;
};
```

---

## 3.9 List Rendering + key

```jsx
const items = [
  { id: 1, name: 'Shirt' },
  { id: 2, name: 'Pants' }
];

const ItemList = () => (
  <ul>
    {items.map(item => (
      <li key={item.id}>{item.name}</li>
    ))}
  </ul>
);
```

---

## 3.10 `useEffect` (side effect)

```jsx
import { useEffect, useState } from 'react';

const Posts = () => {
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    const loadPosts = async () => {
      const res = await fetch('https://jsonplaceholder.typicode.com/posts?_limit=5');
      const data = await res.json();
      setPosts(data);
    };

    loadPosts();
  }, []);

  return (
    <ul>
      {posts.map(post => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
};
```

---

## 3.11 Form Handling

```jsx
import { useState } from 'react';

const ContactForm = () => {
  const [form, setForm] = useState({ name: '', email: '' });

  const handleChange = (e) => {
    const { name, value } = e.target;
    setForm(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(form);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="name" value={form.name} onChange={handleChange} />
      <input name="email" value={form.email} onChange={handleChange} />
      <button type="submit">Submit</button>
    </form>
  );
};
```

---

## 3.12 Component Reuse Pattern

একটা UI বারবার লিখবে না, data পাঠিয়ে reuse করো।

```jsx
const Badge = ({ text, color }) => (
  <span style={{ background: color, padding: '4px 8px' }}>{text}</span>
);
```

---

## 4) Real Mini Project Practice (Production mindset)

## Project: Simple Product Catalog

### Features
- Product list show
- Search by name
- Category filter
- Add to cart (count)
- Total price

### Step-by-step কাজ
1. `products` array তৈরি (id, name, price, category)
2. List render with `map`
3. Search input + `filter`
4. Category dropdown + `filter`
5. `cart` state এ product add
6. `reduce` দিয়ে total বের করা
7. Empty state, loading state handle করা
8. Component ভাগ করা:
   - `ProductFilter`
   - `ProductList`
   - `CartSummary`

---

## 5) Production এ যাওয়ার আগে যা লাগবে

- Clean folder structure
- Reusable components
- API error handling (`try/catch`)
- Loading/error UI
- Form validation
- Environment variables (`.env`)
- Git workflow (feature branch, PR)
- Basic performance (`memo`, unnecessary render avoid)

---

## 6) 30 দিনের প্র্যাকটিস রোডম্যাপ

### Week 1 (ES6 Core)
- প্রতিদিন 2-3টা topic + coding
- `map/filter/reduce` 20+ problem

### Week 2 (React Basics)
- JSX, component, props, state, event
- 5টি mini UI বানাও

### Week 3 (Data + Effects)
- `useEffect`, API fetch, loading/error
- 2টি API based app

### Week 4 (Project + Polish)
- Product catalog project complete
- Code refactor + deploy (Vercel/Netlify)

---

## 7) Daily Practice Template

প্রতিদিন এই ৫টা করবে:
1. 45 মিনিট concept
2. 60 মিনিট coding
3. 30 মিনিট small feature build
4. 15 মিনিট bug fixing
5. 10 মিনিট recap note

---

## 8) Common Mistakes (শুরুতে যেগুলো হয়)

- State direct mutate করা (`state.push()`)
- `key` হিসেবে index ব্যবহার করা (dynamic list এ)
- `useEffect` dependency ভুল দেওয়া
- একই logic বহু component এ copy-paste
- error state handle না করা

---

## 9) Final Target Checklist

- [ ] ES6 core syntax confidently use করতে পারি
- [ ] JSX, props, state, useEffect clear
- [ ] API fetch + loading + error handle করতে পারি
- [ ] Form + validation করতে পারি
- [ ] Small production-like project নিজে বানাতে পারি

---

## 10) তোমার পরের কাজ (Action Now)

1. Vite দিয়ে React app create করো
2. এই ডকুমেন্টের 2 নম্বর section (ES6) পুরো practice করো
3. এরপর 3 নম্বর section এর প্রতিটি topic আলাদা component এ করো
4. 4 নম্বর mini project complete করো
5. শেষে কোড GitHub এ push করে review নাও

শুধু পড়লে হবে না — **প্রতিটি syntax হাতে লিখে ৩-৫ বার practice** করলে দ্রুত production-ready হওয়া সম্ভব।
