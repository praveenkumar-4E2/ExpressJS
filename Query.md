[Query+](#guidelines)

[Query REGEX](#regex)

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


<br></br>


# Guidelines


1. Keep URLs Short and Descriptive

- **Avoid Long URLs:** Long query strings can be difficult to read and may break on some systems.

- **Use Meaningful Names:** Choose descriptive and meaningful key names.
    - Bad: `?x=123`
    - Good:` ?product_id=123`

<br></br>


2. Use Lowercase and Hyphens
- URLs are case-insensitive and lowercase is preferred. Use hyphens `(-)` to separate words for better readability.
    - **Good:** `/search?sort=price-low-to-high`
    - **Bad:**` /search?Sort=PriceLowToHigh`

<br></br>


3. Limit the Number of Query Parameters

- Only include necessary parameters to avoid making URLs overly complicated. Keep the number of query parameters small to improve readability and performance.

    - **Good**: `/search?category=books&sort=price`
    - **Bad**: `/search?category=books&sort=price&limit=10&user_id=123&seller=abc`

<br></br>


4. Encode Special Characters

- Use URL encoding for special characters (e.g., spaces, &, =). This ensures URLs are transmitted properly over the web.
    - **Example**: `A space should be encoded as %20.`
    - **Bad**: `/search?query=hello world`
    - **Good**: `/search?query=hello%20world`

<br></br>


5. Handle Arrays and Nested Objects Properly

- When passing arrays or objects, structure the query string so that the server can easily interpret it.
    - For Arrays:
        - **Good**: `/products?category=books,electronics`
        - **Good (Alternative):** `/products?category[]=books&category[]=electronics`
    - For Nested Objects:
        - **Good:** `/user?profile[name]=john&profile[age]=30`

<br></br>


6. Use Consistent Naming Conventions

- Be consistent in how you name your query parameters across the application. Stick to one naming style, such as camelCase or snake_case.

    - **Good:**` /search?category_id=123`
    - **Bad:** `/search?CategoryID=123`

<br></br>


7. Use Pagination for Large Data Sets

- Instead of fetching large amounts of data in a single request, use pagination in query strings.
    - **Example:** `/products?page=2&limit=20`

<br></br>


8. Avoid Sensitive Information in Query Strings
- Never include sensitive information like passwords, tokens, or personal identifiers in query strings. These can be easily logged or exposed in browser history.
    - **Bad:** `/user?password=mysecretpassword`
    - **Good:** `Use POST requests with parameters in the body for sensitive data.`

<br></br>


9. Ensure Backward Compatibility

- If you update query parameters, ensure that older versions of your URLs (e.g., used in bookmarks) still function correctly. If necessary, redirect old URLs to the new ones.

<br></br>


10. Use Default Values for Missing Parameters

- Handle missing or optional parameters by setting default values on the server-side.
    - **Example**: If `limit` is missing in` /products?sort=price`, set a default value for `limit`.

<br></br>

11. Avoid Redundant Parameters

- Avoid passing the same data multiple times in different query parameters.
    - **Bad:**` /search?query=javascript&term=javascript`
    - **Good:** `/search?query=javascript`

<br></br>


12. Use HTTPS
- Always use https to encrypt the transmission of query strings and ensure security.


<br></br>


# REGEX

1. Basics

- Literal Match



```js
^abc$     # Matches "abc"
```

- Any Character

```js

^a.c$     # Matches "abc", "a1c", etc.
```

- Zero or More Characters


```js
^a.*c$    # Matches "ac", "abc", "a123c", etc.
```


- One or More Characters


```js
^a.+c$    # Matches "abc", "a123c" but not "ac"
```

- Zero or One Character


```js
^a?c$     # Matches "c", "ac"
```

- Character Set


```js
^[abc]$   # Matches "a", "b", or "c"
```

- Negated Character Set


```js
^[^abc]$  # Matches any character except "a", "b", or "c"
```

- Group and Capture


```js
^(abc|def)$  # Matches "abc" or "def"
```

2. Dynamic Segments

- Capture a Single Number


```js
^\/users\/(\d+)$  # Matches "/users/123", capturing "123"
```

- Capture Alphanumeric String

```js
^\/items\/([a-zA-Z0-9_-]+)$  # Matches "/items/item-123", capturing "item-123"
```

- Capture Multiple Segments


```js
^\/users\/(\d+)\/posts\/(\d+)$  # Matches "/users/123/posts/456", capturing "123" and "456"
```

3. **Optional and Named Parameters**

- Optional Segment


```js
^\/users(?:\/(\d+))?$  # Matches "/users" or "/users/123", capturing "123" if it exists
```

- Named Groups


```js
^\/users\/(?<userId>\d+)\/posts\/(?<postId>\d+)$  # Matches "/users/123/posts/456", capturing "123" as userId and "456" as postId
```

4. **Complex Patterns**

- Optional Parameter with Default Value


```js
^\/products(?:\/(\d+))?$  # Matches "/products" or "/products/123"
```

- Path with Query Parameters


```js
^\/search\?query=([^&]+)$  # Matches "/search?query=example", capturing "example"
```

- File Extension


```js
^\/files\/.*\.(jpg|png|gif)$  # Matches files with .jpg, .png, or .gif extension
```

5. **Anchors and Boundaries**

- Start of String


```js
^\/api\/  # Matches paths starting with "/api/"
```

- End of String


```js
\/end$  # Matches paths ending with "/end"
```

- Word Boundaries


```js
\bword\b  # Matches "word" as a whole word
```

6. **Advanced Matching**

- Lookahead (Positive)


```js
^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{6,}$  # Matches passwords with at least one digit, one lowercase letter, and one uppercase letter, and at least 6 characters long
```

- Lookahead (Negative)


```js
^(?!.*password).*  # Matches strings that do not contain "password"
```

- Lookbehind (Positive)


```js
(?<=@)\w+  # Matches words preceded by "@"
```

- Lookbehind (Negative)


```js
(?<!@)\w+  # Matches words not preceded by "@"
```

7. **Common Path Patterns**

- Match Root Path


```js
^\/$  # Matches "/"
```

- Match Subdirectories


```js
^\/([^\/]+)\/([^\/]+)$  # Matches "/dir/subdir", capturing "dir" and "subdir"
```

- Match Multiple Levels

```js

^\/(.+)\/(.+)\/(.+)$  # Matches "/level1/level2/level3", capturing each level
```

- Match Dynamic Segments with Constraints


```js
^\/items\/([a-zA-Z0-9]{5,10})$  # Matches "/items/abc123", capturing alphanumeric strings of length 5 to 10
```

8. **Example Implementations**

- Node.js Example


```js
const url = '/users/123/posts/456';
const regex = /^\/users\/(\d+)\/posts\/(\d+)$/;
const match = regex.exec(url);

if (match) {
  const [fullMatch, userId, postId] = match;
  console.log(`User ID: ${userId}, Post ID: ${postId}`);
}
```


- Express.js Example

```js
const express = require('express');
const app = express();

// Route with regex
app.get(/^\/items\/([a-zA-Z0-9_-]+)\/([0-9]+)$/, (req, res) => {
  const [_, itemId, quantity] = req.params;
  res.send(`Item ID: ${itemId}, Quantity: ${quantity}`);
});

app.listen(3000, () => {
  console.log('Server running on http://localhost:3000');
});
```



**Summary**

- **Basics**: Literal matches, wildcards, and character sets.
- **Dynamic Segments:** Capture numbers, alphanumeric strings, and multiple segments.
- **Optional and Named Parameters:** Use optional segments and named groups for clarity.
- **Complex Patterns:** Handle query parameters, file extensions, and more.
- **Advanced Matching: **Use lookahead/lookbehind assertions for complex validatio
ns.
- **Common Paths:** Match roots, subdirectories, and multiple levels.