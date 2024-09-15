[Middleware](#middleware)

[Router](#router)

[json](#json)

[static files](#static-files)

[status codes](#status-codes)



# Middleware
To write middleware in Express.js, follow these guidelines:
1. **Function Signature:** Middleware functions have the signature `(req, res, next)`, where `req` is the request object, `res` is the response object, and `next` is a callback that passes control to the next middleware in the stack.


    ```js
    function myMiddleware(req, res, next) {
    // Your code here
    next(); // Call next() to pass control
    }
    ```

2. Use `next():` Always call `next()` when you're done with your middleware logic, unless you are sending a response, to ensure the request continues through the middleware chain.

    ```js
    function myMiddleware(req, res, next) {
    console.log('Middleware executed');
    next();
    }
    ```

3. **Modify** `req` or `res`: You can modify the req or res objects to add custom data or logic before passing control to the next middleware.

    ```js
    function myMiddleware(req, res, next) {
    req.customData = "Hello";
    next();
    }
    ```


4.  **Error Handling Middleware:** If you're writing error-handling middleware, use a function with four arguments: (err, req, res, next).


    ```js
    function errorHandler(err, req, res, next) {
    console.error(err.stack);
    res.status(500).send('Something broke!');
    }
    ```

5. **Mount Middleware Globally or on Specific Routes:**

    - **Global Middleware:** Use `app.use()` to apply middleware globally.

        ```js
        app.use(myMiddleware);
        ```

    - **Route-Specific Middleware:** Apply middleware to specific routes.

        ```js
        app.get('/route', myMiddleware, (req, res) => {
        res.send('Hello from route');
        });
        ```

6. **Middleware Chaining:** You can use multiple middleware functions in sequence for a route. Each function will be called one after the other if `next() `is used.


    ```javascript
    app.get('/route', middleware1, middleware2, (req, res) => {
    res.send('Hello after middlewares');
    });
    ```

7. Avoid Response Duplication: Ensure that you send a response (`res.send()`, `res.json()`, etc.) or call `next()` only once to prevent unexpected behavior.


<br></br>

# Router

1. **Basic Route Syntax**

- Define routes using HTTP methods like `GET`, `POST`, `PUT`, `DELETE`, etc. Each route specifies a path and a callback function.


    ```js
    app.get('/path', (req, res) => {
    res.send('GET request to /path');
    });

    app.post('/path', (req, res) => {
    res.send('POST request to /path');
    });
    ```

2.**Route Parameters**

- Use route parameters to capture values from the URL. Parameters are defined with` :paramName` and accessed via `req.params`.


    ```js
    app.get('/users/:id', (req, res) => {
    const userId = req.params.id;
    res.send(`User ID: ${userId}`);
    });
    ```


3. **Query Parameters**

- Access query parameters using `req.query` for optional parameters in the URL.


    ```js
    app.get('/search', (req, res) => {
    const query = req.query.q;
    res.send(`Search query: ${query}`);
    });
    ```


4. **Handling Multiple HTTP Methods**

- Handle multiple HTTP methods for the same route by chaining methods using `.route()`.


    ```js
    app.route('/book')
    .get((req, res) => {
        res.send('Get a book');
    })
    .post((req, res) => {
        res.send('Add a book');
    });
    ```


5. **Modular Routes with `express.Router()`**

- Use `express.Router()` to modularize routes into separate files. This keeps your code clean and maintainable.

    Example:

    - routes/users.js


        ```js
        const express = require('express');
        const router = express.Router();

        router.get('/', (req, res) => {
        res.send('List of users');
        });

        router.get('/:id', (req, res) => {
        res.send(`User ID: ${req.params.id}`);
        });

        module.exports = router;
        ```


    - app.js


        ```js
        const userRoutes = require('./routes/users');
        app.use('/users', userRoutes);
        ```


6. **Use Middleware for Routes**
- Apply middleware to routes by passing it as an argument. This allows you to control access, handle validation, or modify requests.


    ```js
    app.get('/dashboard', authMiddleware, (req, res) => {
    res.send('Dashboard');
    });
    ```


7. **Error Handling in Routes**

- Use `try-catch` for async route handlers or pass errors to Express's built-in error handling using `next()`.


    ```js
    app.get('/data', async (req, res, next) => {
    try {
        const data = await fetchData();
        res.json(data);
    } catch (error) {
        next(error); // Pass the error to the error handler
    }
    });
    ```


8. **Route Naming Conventions**

- Keep route names intuitive and meaningful. Use plural names for collections and singular names for individual items.


    ```js
    app.get('/users', (req, res) => { /* list all users */ });
    app.get('/users/:id', (req, res) => { /* get user by id */ });
    ```


9. **Avoid Deep Nesting**

- Keep your routes as shallow as possible to avoid complexity. For deeply nested paths, consider restructuring your routes or using query parameters.


    ```js
    // Instead of this:
    app.get('/users/:userId/posts/:postId/comments/:commentId', ...);

    // You can break it into simpler routes:
    app.get('/comments/:commentId', ...);
    ```


10. **404 Handling for Undefined Routes**

- Handle 404 errors by defining a catch-all route at the end of your routes stack.


    ```js
    app.use((req, res) => {
    res.status(404).send('Not Found');
    });
    ```




11. **Versioning Routes**

- When building APIs, version your routes to ensure backward compatibility for clients using older versions of your API.

```js
app.get('/api/v1/users', (req, res) => {
  res.send('Version 1: List of users');
});

app.get('/api/v2/users', (req, res) => {
  res.send('Version 2: List of users');
});
```

12. **Group Routes by Feature or Resource**

- Group related routes by features or resources. This helps keep code organized, especially in large applications.

Example:

- Routes for user management (`routes/users.js`):

```js
router.get('/', (req, res) => { /* list users */ });
router.post('/', (req, res) => { /* create a user */ });
router.get('/:id', (req, res) => { /* get user by id */ });
router.put('/:id', (req, res) => { /* update user by id */ });
router.delete('/:id', (req, res) => { /* delete user by id */ });
```

13. **Use Descriptive HTTP Status Codes**

- Always return the appropriate HTTP status codes in your routes to help the client understand the result of their 
request.


```js
app.post('/users', (req, res) => {
  // On success
  res.status(201).send('User created');
  
  // On client error
  res.status(400).send('Invalid user data');
});
```


14. **Route Caching and Optimization**

- For routes that handle static data or data that doesn't change frequently, consider caching the response to improve performance.


```js
const cache = {}; // Simplified cache example

app.get('/data', (req, res) => {
  if (cache.data) {
    return res.send(cache.data);
  }
  // Simulate data fetching
  const data = fetchData();
  cache.data = data;
  res.send(data);
});
```


15. **Pagination and Filtering**

- When building routes that return lists or collections (e.g., users, posts), implement pagination and filtering to improve performance.


```js
app.get('/users', (req, res) => {
  const page = parseInt(req.query.page) || 1;
  const limit = parseInt(req.query.limit) || 10;
  const users = getUsers(page, limit);
  res.json({ page, limit, users });
});
```

16. **Securing Routes**

- Secure sensitive routes using authentication and authorization middleware. This ensures only authorized users can access specific routes.


```js
const authMiddleware = (req, res, next) => {
  if (!req.isAuthenticated()) {
    return res.status(403).send('Forbidden');
  }
  next();
};

app.get('/admin', authMiddleware, (req, res) => {
  res.send('Admin dashboard');
});

```

17. **Error Handling in Async Routes**

- In async route handlers, use a wrapper to handle errors gracefully. This prevents unhandled promise rejections.


```js
const asyncHandler = fn => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);

app.get('/data', asyncHandler(async (req, res) => {
  const data = await fetchData();
  res.json(data);
}));
```

18. **Chaining Middleware Functions in Routes**

- You can chain multiple middleware functions for more complex routes. This is useful when splitting logic into smaller, reusable functions.


```js
function logger(req, res, next) {
  console.log(`Request received: ${req.method} ${req.url}`);
  next();
}

function auth(req, res, next) {
  if (!req.isAuthenticated()) {
    return res.status(401).send('Unauthorized');
  }
  next();
}

app.get('/dashboard', logger, auth, (req, res) => {
  res.send('User Dashboard');
});

```


19. **Rate Limiting**

- Implement rate limiting to protect routes from excessive requests, which can help prevent denial of service (DoS) attacks.


```js
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
});

app.use('/api/', limiter); // Apply rate limiting to all API routes

```

20. **Route Validation**

- Use validation middleware like express-validator or Joi to validate incoming requests, ensuring data integrity and security.


```js
const { body, validationResult } = require('express-validator');

app.post('/users', [
  body('email').isEmail(),
  body('password').isLength({ min: 6 })
], (req, res) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).json({ errors: errors.array() });
  }
  // Proceed with user creation
  res.send('User created');
});

```

21. **Centralized Error Handling**

- Set up centralized error handling middleware to capture all errors across your routes and return proper responses.


```js
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send('Something went wrong!');
});
```






<br></br>


# JSON

In Express.js, working with JSON is common for handling APIs. Here are key guidelines and practices for dealing with JSON:

1. **Sending JSON Responses**

- Use `res.json()` to send a JSON response to the client.


```js
app.get('/user', (req, res) => {
  const user = { name: 'John Doe', age: 30 };
  res.json(user);
});
```


2. **Parsing Incoming JSON Data**

- Use Express’s built-in middleware `express.json()` to parse incoming JSON requests. This middleware automatically parses the JSON body of incoming requests and makes it available on `req.body`.



```js
app.use(express.json()); // To parse JSON request body

app.post('/user', (req, res) => {
  const { name, age } = req.body;
  res.send(`Received user data: ${name}, age: ${age}`);
});
```


3. **Handling JSON Errors**

- Be sure to handle any errors from malformed JSON by using error-handling middleware. If the request contains invalid JSON, Express will throw an error.


```js
app.use(express.json());

app.use((err, req, res, next) => {
  if (err instanceof SyntaxError && err.status === 400 && 'body' in err) {
    return res.status(400).send({ error: 'Invalid JSON' });
  }
  next();
});
```



4. **Sending Custom Status Codes with JSON**

- You can send custom HTTP status codes along with the JSON response.


```js
app.post('/user', (req, res) => {
  if (!req.body.name) {
    return res.status(400).json({ error: 'Name is required' });
  }
  res.status(201).json({ message: 'User created successfully' });
});
```


5. **Deeply Nested JSON Objects**

- When working with deeply nested JSON objects, make sure to structure your data appropriately to maintain readability and organization.


```js
app.get('/user-profile', (req, res) => {
  const profile = {
    name: 'Jane Doe',
    age: 32,
    address: {
      street: '123 Main St',
      city: 'Anytown',
      zip: '12345'
    },
    preferences: {
      theme: 'dark',
      notifications: true
    }
  };
  res.json(profile);
});
```


6. **Setting JSON Response Headers**

- By default, Express sets the `Content-Type: application/json` header when using `res.json()`. However, you can set custom headers if needed.



```js
app.get('/data', (req, res) => {
  res.setHeader('Custom-Header', 'Value');
  res.json({ message: 'Custom headers added' });
});
```


7. **Handling Arrays in JSON**

- You can send and receive arrays as part of your JSON responses or requests.


```js
app.get('/users', (req, res) => {
  const users = [
    { id: 1, name: 'John Doe' },
    { id: 2, name: 'Jane Smith' }
  ];
  res.json(users);
});

app.post('/users', (req, res) => {
  const users = req.body; // Expecting an array of users
  res.json({ message: 'Users received', users });
});
```


8. **Nested JSON Validation**


- When handling nested JSON objects or arrays, validate incoming data to ensure all required fields are present and correctly formatted.


```js
app.post('/order', (req, res) => {
  const { orderId, items } = req.body;
  if (!orderId || !Array.isArray(items) || items.length === 0) {
    return res.status(400).json({ error: 'Invalid order data' });
  }
  res.status(200).json({ message: 'Order received' });
});
```


9. **Handling JSON Arrays with Validation**

- Ensure that you validate each item in a JSON array before processing it.


```js
app.post('/batch-users', (req, res) => {
  const users = req.body; // Expect an array of users
  if (!Array.isArray(users)) {
    return res.status(400).json({ error: 'Expected an array of users' });
  }

  const invalidUsers = users.filter(user => !user.name || !user.age);
  if (invalidUsers.length) {
    return res.status(400).json({ error: 'Some users are invalid', invalidUsers });
  }

  res.json({ message: 'All users are valid' });
});
```


10. **Pretty-print JSON Responses**

- You can enable pretty-printing of JSON responses (useful for development purposes) by setting the `app.set()` configuration.


```js

app.set('json spaces', 2); // Pretty-print JSON responses with 2 spaces indentation
```



<br></br>

# Static Files

Serving static files (like HTML, CSS, images, JavaScript, etc.) in an Express.js application is common for creating websites or providing assets for your front-end. Here’s a guide on how to handle static files effectively in Express.js.

1. **Use `express.static` Middleware**

- The `express.static()` middleware is used to serve static files in Express. You can specify a directory where the static files are stored, and Express will serve them automatically.


```js
const express = require('express');
const app = express();

// Serve static files from the 'public' directory
app.use(express.static('public'));

app.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
});
```

-   Directory structure example:


```js
project-folder/
├── public/
│   ├── css/
│   │   └── styles.css
│   ├── images/
│   │   └── logo.png
│   └── js/
│       └── script.js
└── app.js
```


- After setting up, you can access static files directly in the browser:

  - CSS: `http://localhost:3000/css/styles.css`
  - Images: `http://localhost:3000/images/logo.png`
  - JavaScript: `http://localhost:3000/js/script.js`


2. **Serving Multiple Static Directories**

- You can serve static files from multiple directories by calling `express.static()` for each directory.


```js
app.use(express.static('public'));
app.use(express.static('assets'));
```

Now, Express will check both the public and assets directories for requested static files.

3. **Setting a Virtual Path**

- You can set a virtual path prefix for serving static files. This means the URL path will include the prefix before accessing the file.

```js
app.use('/static', express.static('public'));

// Access files like this:
// http://localhost:3000/static/css/styles.css
```


4. **Using Cache Control with Static Files**

- To improve performance, you can add cache control for static files. This helps browsers cache static files like images, CSS, and JavaScript.

```js
app.use(express.static('public', {
  maxAge: '1d', // Cache files for 1 day
}));
```

This adds cache headers to the response, allowing the browser to cache the files for the specified time.

5. **Custom 404 Page for Static Files**

- If a static file is not found, you can create a custom 404 response for missing static files by adding middleware after your static middleware.


```js
app.use(express.static('public'));

app.use((req, res, next) => {
  res.status(404).send('File not found!');
});
```


6. **Handling File Extensions**

- Express will serve the file without requiring you to specify the extension in the URL. For example, you can request `/about` instead of /`about.html`.



```js
// Serve files without the extension (works only with .html by default)
app.use(express.static('public'));
```

You can access:

- `http://localhost:3000/about` for `about.html`.


7. **Serving Static Files in Development vs. Production**

- In development, serving static files as they are is fine. However, in production, it's a good practice to use compression for static files (CSS, JS) for performance optimization.

Example using `compression` middleware for production:


```js
const compression = require('compression');

app.use(compression()); // Compress all responses
app.use(express.static('public'));
```


This compresses static assets before sending them to the client, reducing load time.

8. **Serving Static Files from Different Domains**

- Sometimes, your static files may be hosted on a separate domain or CDN (Content Delivery Network). In this case, ensure your URLs correctly point to those domains, or serve them via Express by proxying the requests.

9. Security Considerations

- Ensure your static directory does not expose sensitive files or directories (like config files, environment variables, etc.). It's best to store only publicly accessible files (CSS, JS, images) in the static directory.

- You can limit the types of files served by using serve-static options:


```js
const serveStatic = require('serve-static');

app.use(serveStatic('public', {
  extensions: ['html', 'htm'], // Only serve HTML files
}));
```

**Example of Full Express Static File Setup:**


```js
const express = require('express');
const path = require('path');
const app = express();

// Use static middleware to serve files from 'public' folder
app.use('/static', express.static(path.join(__dirname, 'public')));

// Serve index.html as the default page
app.get('/', (req, res) => {
  res.sendFile(path.join(__dirname, 'public', 'index.html'));
});

// 404 for static files not found
app.use((req, res) => {
  res.status(404).send('Sorry, file not found!');
});

app.listen(3000, () => {
  console.log('Server is running at http://localhost:3000');
});
```

This code serves static files from the `public` folder, allows serving static files under the `/static` URL path, and returns a `404` if a static file is not found.

# Examples

Here are more examples to help you understand how to serve and handle static files in an Express.js application:

1. **Serving Static Files from Multiple Directories**

- You can serve static files from multiple directories. For example, you might want to serve files from both a `public` directory and an `assets` directory.

```js

const express = require('express');
const app = express();

// Serve static files from 'public' directory
app.use(express.static('public'));

// Serve static files from 'assets' directory
app.use(express.static('assets'));

app.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```

Now, files can be accessed from both directories, e.g., `/css/styles.css` from `public` and` /images/logo.png` from `assets`.

2. **Serving Static Files with Virtual Path Prefix**

- You can create a virtual path prefix for serving static files, meaning the files will appear to be served from a different URL path.


```js
const express = require('express');
const app = express();

// Serve static files from 'public' directory under the '/static' path
app.use('/static', express.static('public'));

app.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```
Now, instead of accessing the file like `/css/styles.css`, it will be served at `/static/css/styles.css`.



3. `Cache Control for Static Files`

- You can configure cache control by setting the `maxAge` option when serving static files. This instructs the browser to cache files for a certain amount of time, improving performance.


```js
const express = require('express');
const app = express();

// Serve static files with a 30-day cache
app.use(express.static('public', {
  maxAge: '30d'
}));

app.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```


This setup allows the browser to cache static assets for 30 days, reducing the number of requests to the server.

4. **Serving HTML and Static Files Together**

- You can serve both HTML files and other static assets like CSS, JavaScript, and images from a static directory.

```js
const express = require('express');
const path = require('path');
const app = express();

// Serve static files from the 'public' folder
app.use(express.static('public'));

// Serve the index.html file for the root URL
app.get('/', (req, res) => {
  res.sendFile(path.join(__dirname, 'public', 'index.html'));
});

app.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```

In this example, when the user accesses the root URL `/`, they are served the `index.html` file, and all static files (CSS, JS) are also available under the `public` directory.

5. **Using Custom 404 Page for Static Files Not Found**

- You can create a custom 404 page for when a static file is not found.


```js
const express = require('express');
const app = express();

// Serve static files
app.use(express.static('public'));

// Custom 404 response for missing static files
app.use((req, res) => {
  res.status(404).send('Sorry, we could not find that file!');
});

app.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```


If a user requests a static file that doesn’t exist, such as `/images/missing.jpg`, they will get a custom `404` response: "Sorry, we could not find that file!".


6. **Serving Static Files Conditionally Based on Environment**

- You may want to serve different static files in development and production environments. For instance, in development, you serve raw files, but in production, you serve minified versions.


```js
const express = require('express');
const app = express();

if (process.env.NODE_ENV === 'production') {
  // Serve minified files in production
  app.use('/static', express.static('dist'));
} else {
  // Serve raw files in development
  app.use('/static', express.static('public'));
}

app.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```


7. **Serving Files Without Extension**

- By default, Express serves `.html` files without needing to specify the extension in the URL. For example, you can request `/about` and it will serve `about.html`.


```js
const express = require('express');
const app = express();

// Serve static files from 'public' directory
app.use(express.static('public'));

// Serve HTML files without the .html extension
app.get('/about', (req, res) => {
  res.sendFile(__dirname + '/public/about.html');
});

app.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```


In this case, requesting `http://localhost:3000/about` serves the `about.html` file from the `public` directory.

8. **Serve Files with MIME Type and Custom Headers**

- You can manually set the `Content-Type` or any custom headers when serving static files.

```js
const express = require('express');
const path = require('path');
const app = express();

// Serve static files from 'public' directory
app.use(express.static('public'));

// Serve a file with a custom header
app.get('/report', (req, res) => {
  res.setHeader('Content-Type', 'application/pdf');
  res.sendFile(path.join(__dirname, 'public', 'report.pdf'));
});

app.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```


In this example, the PDF file is served with the correct `Content-Type`, ensuring browsers handle it as a PDF document.

9. **Serve JSON Files as Static Content**

- You can also serve static JSON files by placing them in your static directory.

```js

const express = require('express');
const app = express();

// Serve JSON files from the 'public' directory
app.use(express.static('public'));

app.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```


If you have a `data.json` file in the `public` directory, it can be accessed by visiting `http://localhost:3000/data.json`.


10. **Serve Files with Compression**
- To further optimize your static file delivery, you can use middleware like `compression` to serve compressed versions of your files.

```js

const express = require('express');
const compression = require('compression');
const app = express();

// Enable compression for all responses
app.use(compression());

// Serve static files from the 'public' directory
app.use(express.static('public'));

app.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```
This reduces the size of the files before sending them to the client, improving load times and bandwidth efficiency.


<br></br>

# STATUS CODES

## 1. Use Correct Status Codes for Different Scenarios
- __2xx Success Codes:__

  - **200 OK:** Use when the request has been successfully processed. This is the most common success status code.

  - **201 Created:** Use when a new resource has been created, e.g., after a POST request.

  - **204 No Content:** Use when the request was successful, but there is no content to return (often used for DELETE requests).

- **3xx Redirection Codes:**

  - **301 Moved Permanently:** Use when a resource has been permanently moved to a new URL.

  - **302 Found:** Use when a resource has been temporarily moved to a new URL.
  - **304 Not Modified:** Use when the resource has not changed, allowing the client to use its cached version.

- **4xx Client Error Codes:**

  - **400 Bad Request:** Use when the server cannot process the request due to  - client error (e.g., malformed request).

  - **401 Unauthorized:** Use when the request requires user authentication.

  - **403 Forbidden:** Use when the client does not have permission to access the   - resource.

  - **404 Not Found:** Use when the requested resource is not available.

  - **405 Method Not Allowed:** Use when the HTTP method is not allowed for the requested resource.

- **5xx Server Error Codes:**

  - **500 Internal Server Error:** Use for general server errors.
  - **502 Bad Gateway:** Use when the server, acting as a gateway or proxy,   - receives an invalid response from the upstream server.
  - **503 Service Unavailable:** Use when the server is temporarily unavailable due to overload or maintenance.

## 2. Consistency in Response Format

- **Standardize Responses:** Ensure that success and error responses follow a consistent format across your API. A typical JSON response may include:


```json
{
  "status": 200,
  "data": {...},
  "message": "Operation successful"
}

```


3. **Set Response Status Explicitly**

- Use `res.status()` to set the correct status code before sending a response. For example:


```js
res.status(200).json({ message: 'Request successful', data });
```

4. **Respond to Missing Resources with 404**

- Handle 404 Errors: If a resource is not found, always respond with a 404 Not Found status:


```js
app.get('/resource/:id', (req, res) => {
  const resource = getResource(req.params.id);
  if (!resource) {
    return res.status(404).json({ message: 'Resource not found' });
  }
  res.status(200).json(resource);
});
```


5. **Handle Validation Errors with 400**
- **Send Detailed Error Messages:** If input validation fails, return a 400 Bad Request and provide information on what caused the error:


```js
res.status(400).json({ message: 'Invalid request', errors: validationErrors });
```


6. **Return 401 for Authentication Failures**

- **Unauthorized Access:** If authentication is required and fails (e.g., missing or invalid token), return a 401 Unauthorized:


```js
res.status(401).json({ message: 'Authentication required' });
```


7. **Return 403 for Forbidden Access**

- **Insufficient Permissions:** When the user is authenticated but does not have the necessary permissions, return a 403 Forbidden:


```js
res.status(403).json({ message: 'Access forbidden' });
```

8. **Use 201 for Successful Creation**

- **Indicate Resource Creation:** When a new resource is created, return a 201 Created and optionally include a `Location` header pointing to the new resource:


```js
res.status(201).json({ message: 'Resource created', data: newResource });
```


9. **Return 204 for Successful Deletions**

- **No Content:** After a DELETE operation, return 204 No Content to indicate the resource was successfully deleted:


```js
res.status(204).send();
```

10. **Handle Server Errors Gracefully**

- **Log and Respond with 500:** For unexpected server errors, return a 500 Internal Server Error and log the issue for debugging:\

```js

app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ message: 'Internal server error' });
});
```

11. **Handle Rate Limiting with 429**

- **Too Many Requests:** If your application implements rate limiting, return a 429 Too Many Requests with a `Retry-After` header:

```js

res.status(429).set('Retry-After', '30').json({ message: 'Too many requests, please try again later' });
```

12.**Use `Location` Header for Redirection**

- **Include Redirect Information:** When returning 3xx status codes, include the `Location` header to specify the new URL:


```js
res.redirect(301, '/new-url');
```


13. **Send 503 for Service Unavailability**
- **Temporary Downtime:** If your service is temporarily unavailable (e.g., during maintenance), return a 503 Service Unavailable and include a `Retry-After` header:


```js
res.status(503).set('Retry-After', '60').json({ message: 'Service temporarily unavailable, please try again later' });
```

14. **Log 5xx Errors for Debugging**

- **Critical Server Errors:** Log 5xx status code 


## More Examples

1. **Standardize Error Responses**
- **Send Consistent Error Messages:** When sending error responses, ensure that the format of error messages is standardized. Typically, this should include a status code, an `error message`, and any additional details that may help the client debug the issue.

```json
{
  "status": 400,
  "message": "Invalid input data",
  "details": "The 'email' field is required"
}
```

2. **Use `res.status() `to Set Response Codes**

- **Set Status Explicitly:** Always explicitly set the status code using **res.status().** This ensures that the correct code is returned even if you're sending a custom response.


```js
app.get('/users/:id', (req, res) => {
  const user = findUserById(req.params.id);
  if (!user) {
    return res.status(404).json({ message: 'User not found' });
  }
  res.status(200).json(user); // Explicitly send 200 OK with the user data
});
```


3. **Handle 4xx and 5xx Responses Separately**

- **Differentiate Client and Server Errors:** When dealing with errors, distinguish between `client errors (4xx)` and `server errors (5xx)`. Log server-side errors (5xx) in your system for further investigation, but client-side errors (4xx) may not require detailed logging.

```js
app.post('/login', (req, res) => {
  const { username, password } = req.body;
  if (!username || !password) {
    return res.status(400).json({ message: 'Username and password are required' });
  }
  // Login logic here
  res.status(200).json({ message: 'Login successful' });
});
```

4. **Handle 500 Errors Gracefully**

- **Log 500 Errors with Detailed Information:** When an internal server error occurs, log the error details and return a generic error message to the client. Do not expose sensitive information in the error response.


```js
app.use((err, req, res, next) => {
  console.error(err.stack); // Log the stack trace for debugging
  res.status(500).json({ message: 'Something went wrong! Please try again later.' });
});
```



5. **Include WWW-Authenticate Header with 401 Responses**

- **Prompt for Authentication on 401:** When returning a 401 Unauthorized status, include a `WWW-Authenticate` header in the response to indicate what kind of authentication is required (e.g., Basic, Bearer).


```js
res.status(401).set('WWW-Authenticate', 'Bearer').json({ message: 'Authentication required' });

```

6. **Send Descriptive Error Messages for 400 Bad Requests**

- **Provide Clear Feedback for Bad Requests:** When returning a 400 Bad Request, provide clear feedback on what went wrong with the request, such as missing or invalid data, to help the client fix the issue.

```js
res.status(400).json({ message: 'Invalid email format' });
```


7. **Use 405 Method Not Allowed for Unsupported Methods**

- **Return 405 for Invalid HTTP Methods:** If a resource does not support a particular HTTP method (e.g., POST to a GET-only route), return a 405 Method Not Allowed and include an Allow header with the allowed methods.

```js
res.status(405).set('Allow', 'GET, POST').json({ message: 'Method not allowed' });
```


8. **Redirect with 3xx Status Codes**
- **Use 301 or 302 for Redirections:** When redirecting a client to a different URL, use 3xx status codes (e.g., 301 Moved Permanently or 302 Found) to guide the client to the new location.

```js
res.redirect(301, '/new-location');
```

9. **Return 429 Too Many Requests for Rate Limiting**

- **Limit Requests to Prevent Abuse:**If your application implements rate-limiting, return a 429 Too Many Requests status code when a client exceeds their allowed number of requests in a given timeframe. Optionally, include a `Retry-After` header to tell the client when they can try again.

```js

res.status(429).set('Retry-After', '3600').json({ message: 'Too many requests, please try again later' });
```

10. **Return 422 Unprocessable Entity for Validation Failures**

- **Handle Validation Errors:** Use the 422 Unprocessable Entity status code when the request contains well-formed data but fails validation (e.g., invalid email format, or missing required fields).


```js
res.status(422).json({ message: 'Email is invalid or missing' });
```

11. **Return 503 Service Unavailable for Downtime**

- **Handle Downtime with 503:** When your service is temporarily unavailable due to maintenance or overload, return a 503 Service Unavailable and optionally include a Retry-After header to indicate when the service will be back online.


```js
res.status(503).set('Retry-After', '60').json({ message: 'Service temporarily unavailable, try again in a minute' });
```

12. **Log Critical Response Codes for Monitoring**

- **Monitor Critical Codes (4xx/5xx):** Always log 5xx errors (server issues) and optionally log important 4xx errors (such as 403 Forbidden or 401 Unauthorized) to monitor your application’s performance and security incidents.


```js
if (res.statusCode >= 500) {
  logger.error(`Server error with status ${res.statusCode}: ${errorMessage}`);
}
```