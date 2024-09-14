# Good Express.js Practices
### 1. Organize Your Project Structure

**Principle:** Keep your project structure clean and modular to improve maintainability and scalability as your application grows.

- Use an organized directory structure like    `MVC (Model-View-Controller)` or `feature-based grouping`.

Example Project Structure:

```js
/controllers      # Controllers for route handling logic
/models           # Database models
/routes           # Route definitions and middleware
/middlewares      # Custom middleware
/config           # Configuration files
/services         # Business logic, external services
/public           # Static files (HTML, CSS, JS)
app.js            # Main app file
```

<br></br>

### 2. Use Environment Variables

**Principle:** Store sensitive information like API keys, database credentials, and environment-specific configurations in environment variables using `.env` files.

- Use the `dotenv` package to load environment variables from ```.env``` file into ```process.env.```

```js
# .env
DB_HOST=localhost
DB_PORT=5432
JWT_SECRET=supersecretkey
```
```js
// app.js
require('dotenv').config();
const dbHost = process.env.DB_HOST;
```
<br></br>

### 3. Modularize Your Routes
**Principle:** Separate your route logic into different modules based on functionality to keep the code clean and easier to manage.
Example:

```js
// routes/userRoutes.js
const express = require('express');
const router = express.Router();
const userController = require('../controllers/userController');

router.get('/users', userController.getAllUsers);
router.post('/users', userController.createUser);

module.exports = router;
```

```js
// app.js
const userRoutes = require('./routes/userRoutes');
app.use('/api', userRoutes);
```

<br></br>

### 4. Use Middleware Effectively

**Principle:** Middleware is one of Express’s core features, and it’s important to use it properly for tasks like authentication, logging, and validation.

- Common Middlewares:
    - **Body Parser:** Parse incoming request bodies.
    - **Cors:** Handle Cross-Origin Resource Sharing.
    - **Helmet:** Secure HTTP headers.
    - **Morgan:** HTTP request logging.


```js
const express = require('express');
const bodyParser = require('body-parser');
const helmet = require('helmet');

const app = express();

app.use(helmet());             // Secure HTTP headers
app.use(bodyParser.json());     // Parse JSON bodies
app.use('/api', userRoutes);    // Use routes with middleware
```

<br></br>

### 5. Handle Errors Gracefully

**Principle:** Implement centralized error handling so that all errors are managed in a consistent manner across the app.

Example Error Handler:

```js
// middleware/errorHandler.js
function errorHandler(err, req, res, next) {
    console.error(err.stack);
    res.status(500).json({ message: err.message });
}

module.exports = errorHandler;
```

```js
// app.js
const errorHandler = require('./middleware/errorHandler');
app.use(errorHandler);
```

<br></br>

### 6. Use Async-Await with Proper Error Handling

**Principle:** Since Express.js doesn’t handle promise rejections automatically, use `try-catch` or error-handling middleware to prevent unhandled promise rejections.

```js
// controller/userController.js
const getUser = async (req, res, next) => {
  try {
    const user = await User.findById(req.params.id);
    if (!user) return res.status(404).send('User not found');
    res.status(200).json(user);
  } catch (err) {
    next(err);  // Passes error to the error handling middleware
  }
};
```

<br></br>



### 7. Validate and Sanitize Input
**Principle:** Always validate and sanitize user input to prevent security risks such as `SQL injection` and `XSS (Cross-Site Scripting)` attacks.

- Use libraries like `Joi`,    `express-validator`, or `validator.js` for input validation.

```js
const { body, validationResult } = require('express-validator');

app.post('/register',
  body('email').isEmail(),
  body('password').isLength({ min: 6 }),
  (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }
    // Register user
  }
);
```

<br></br>


### 8. Secure Your Application
**Principle:** Implement security measures to safeguard your application from common vulnerabilities like **CSRF**, **XSS**, and **DDoS**.

- Use **Helmet** for security headers.
- Implement **rate limiting** using `express-rate-limit` to prevent DDoS.
- Sanitize user input.
- Ensure that sensitive data like JWT tokens and session data are protected.

```js
const rateLimit = require('express-rate-limit');
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000,  // 15 minutes
  max: 100                   // limit each IP to 100 requests per windowMs
});
app.use(limiter);
```
<br></br>

### 9. Avoid Blocking Code

**Principle:** Express.js is single-threaded, so avoid blocking the event loop by running long-running synchronous tasks.

- Use asynchronous functions for tasks like database operations and external API calls.

```js
// Bad: Blocking I/O
const fs = require('fs');
app.get('/file', (req, res) => {
  const data = fs.readFileSync('/path/to/file');
  res.send(data);
});

// Good: Non-blocking I/O
app.get('/file', async (req, res) => {
  const data = await fs.promises.readFile('/path/to/file');
  res.send(data);
});
```

<br></br>

### 10. Use a Linter for Consistent Code

**Principle:** Use ESLint to enforce consistent coding styles and catch common mistakes.

- Define your coding rules in ```.eslintrc file.```
- Use popular configurations like  ```eslint:recommended``` or extend configurations like ```airbnb```.

```js
# Install ESLint
npm install eslint --save-dev

```



<br></br>

### 11. Use Version Control (Git)

**Principle:** Always use Git for version control and maintain meaningful commit messages. Use ```.gitignore``` to exclude files that shouldn't be in your repository (e.g., `node_modules`, `.env`).

```js
# .gitignore
node_modules/
.env
```

<br></br>

### 12. Implement Logging
**Principle:** Use logging for debugging and monitoring production issues. The ```morgan``` library is commonly used for HTTP request logging.

```js
const morgan = require('morgan');
app.use(morgan('combined'));
```

For production, you can integrate with a logging service like Winston for more robust logging.


<br></br>

### 13. Use CORS Correctly
**Principle:** Configure CORS (Cross-Origin Resource Sharing) properly to allow only authorized domains to access your API.

```js
const cors = require('cors');
const allowedOrigins = ['https://yourwebsite.com', 'https://anotherdomain.com'];

app.use(cors({
  origin: function (origin, callback) {
    if (!origin || allowedOrigins.includes(origin)) {
      callback(null, true);
    } else {
      callback(new Error('Not allowed by CORS'));
    }
  }
}));
```
<br></br>


### 14. Optimize Performance
**Principle:** Implement best practices for improving the performance of your Express.js app.

- **Compression:** Use the `compression` middleware to compress response bodies.
- **Caching:** Use caching headers `(Cache-Control)` or tools like Redis to cache frequently requested data.

```js
const compression = require('compression');
app.use(compression());  // Enable response compression
```

<br></br>

### 15. Write Unit Tests
**Principle:** Write unit tests to ensure the reliability of your code. Use testing frameworks like **Mocha**, **Chai**, or **Jest**.

```js
const request = require('supertest');
const app = require('../app');  // Import your Express app

describe('GET /users', () => {
  it('should return a list of users', (done) => {
    request(app)
      .get('/users')
      .expect(200)
      .expect('Content-Type', /json/)
      .end((err, res) => {
        if (err) return done(err);
        done();
      });
  });
});
```
