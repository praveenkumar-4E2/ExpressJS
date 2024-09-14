# 1. Understanding Query Strings

## 1.1 What is a Query String?
A query string is a part of a URL that contains data to be sent to the server. It follows the `?` character and consists of key-value pairs separated by `&`.

Basic Structure:

```js
http://example.com/path?key1=value1&key2=value2
```

## 1.2 Query String Components
- **Base URL:** `http://example.com/path`

- **Query String:** `key1=value1&key2=value2`


<br></br>


# 2. Basic Query Strings
## 2.1 Single Key-Value Pair
- **Example URL:** `http://example.com/search?query=books`
- **Explanation:**  `query` is the key, and `books` is the value.

## 2.2 Multiple Key-Value Pairs

- **Example** URL: http://example.com/search?query=books&sort=price&limit=10
- **Explanation:**
    - `query` is `books`
    - `sort` is `price`
    - `limit` is `10`



<br></br>


# 3. Advanced Query Strings

## 3.1 Array Parameters

You can represent arrays in query strings using square brackets or multiple parameters with the same key.

- **Square Brackets:**

    - **Example URL:** `http://example.com/filter?category[]=books&category[]=electronics`

    - **Explanation**: `category[]` has values `books`, `electronics`.

    
- **Multiple Parameters:**

    - **Example URL:** `http://example.com/filter?category=books&category=electronics`

    - **Explanation**: **Multiple** category keys with different values.

## 3.2 Nested Parameters

Nested parameters can be represented using dot notation or brackets.

- Dot Notation:

    - **Example URL:** `http://example.com/user?profile.name=John&profile.age=30`
    - **Explanation:** `profile.name` is `John`, `profile.age` is `30`.
- Brackets:

    - **Example URL:** `http://example.com/user?profile[name]=John&profile[age]=30`

    - **Explanation:** `profile[name]` is `John`, `profile[age]` is `30`.


## 3.3 Encoded Query Strings

Special characters in query strings must be URL-encoded.

- **Example URL:** http://example.com/search?query=hello%20world

- Explanation: `%20` represents a space `character`.

## 3.4 Boolean Parameters
Boolean values can be included as `true` or `false`.

- **Example URL:** `http://example.com/settings?darkMode=true`

- **Explanation:** `darkMode` is `true`.

## 3.5 Mixed Types
Combine different data types in query strings.

- ** Example URL:** `http://example.com/profile?name=JohnDoe&age=25&active=true`

- **Explanation:** `name` is `JohnDoe`, `age` is `25`, `active` is `true`.

## 3.6 Optional Parameters

Some parameters might be optional and not included in every request.

- **Example URL:** `http://example.com/products?category=electronics`
- **Explanation:** `category` is `electronics`, other parameters might be omitted.


<br></br>

# 4. Using Query Strings in Express.js
Express.js makes it easy to work with query strings. You can access query parameters using `req.query`.

4.1 Basic Example

```js

const express = require('express');
const app = express();
const port = 3000;

app.get('/search', (req, res) => {
  const query = req.query.query; // e.g., 'books'
  res.send(`Query: ${query}`);
});

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```


4.2 Multiple Query Parameters

```js
app.get('/search', (req, res) => {
  const query = req.query.query;   // e.g., 'books'
  const sort = req.query.sort;     // e.g., 'price'
  const limit = req.query.limit;   // e.g., '10'
  res.send(`Query: ${query}, Sort: ${sort}, Limit: ${limit}`);
});
```


4.3 Array Query Parameters

```js
app.get('/filter', (req, res) => {
  const categories = req.query['category[]']; // e.g., ['books', 'electronics']
  res.send(`Categories: ${categories.join(', ')}`);
});
```

4.4 Nested Query Parameters

```js

app.get('/user', (req, res) => {
  const name = req.query['profile[name]']; // e.g., 'John'
  const age = req.query['profile[age]'];   // e.g., '30'
  res.send(`Name: ${name}, Age: ${age}`);
});

```

4.5 Encoded Query Strings

```js
app.get('/search', (req, res) => {
  const query = req.query.query; // e.g., 'hello world'
  res.send(`Query: ${decodeURIComponent(query)}`);
});

```

4.6 Boolean Query Parameters

```js
app.get('/settings', (req, res) => {
  const darkMode = req.query.darkMode === 'true'; // e.g., true
  res.send(`Dark Mode: ${darkMode}`);
});

```

4.7 Mixed Types

```js
app.get('/profile', (req, res) => {
  const name = req.query.name;   // e.g., 'JohnDoe'
  const age = req.query.age;     // e.g., '25'
  const active = req.query.active === 'true'; // e.g., true
  res.send(`Name: ${name}, Age: ${age}, Active: ${active}`);
});

```

4.8 Optional Parameters
```js
app.get('/products', (req, res) => {
  const category = req.query.category; // e.g., 'electronics'
  res.send(`Category: ${category || 'not specified'}`);
});

```

<br></br>

# 5. Best Practices
- **Validate Input:** Always validate and sanitize query parameters to prevent security issues.

- **Use Encoding:** Properly encode and decode query parameters to handle special characters.

- **Handle Optional Parameters:** Provide default values or handle cases where parameters are missing.