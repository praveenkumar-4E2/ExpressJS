```javascript
express() - Application Class
├── Methods:
│   ├── .get(path: string, callback: Function): Application
│   ├── .post(path: string, callback: Function): Application
│   ├── .put(path: string, callback: Function): Application
│   ├── .delete(path: string, callback: Function): Application
│   ├── .use(path: string, callback: Function): Application
│   ├── .listen(port: number, callback?: Function): Server
│   ├── .set(setting: string, value: any): Application
│   ├── .enable(setting: string): Application
│   ├── .disable(setting: string): Application
│   ├── .engine(ext: string, callback: Function): Application
│   ├── .route(path: string): Router
│   ├── .param(name: string, callback: Function): Application
│   └── .all(path: string, callback: Function): Application
│
├── Properties:
│   └── .locals: object
│
└── Events:
    └── 'mount' (parent: Application)

Router Class
├── Methods:
│   ├── .get(path: string, callback: Function): Router
│   ├── .post(path: string, callback: Function): Router
│   ├── .put(path: string, callback: Function): Router
│   ├── .delete(path: string, callback: Function): Router
│   ├── .use(path: string, callback: Function): Router
│   ├── .param(name: string, callback: Function): Router
│   └── .all(path: string, callback: Function): Router
│
└── Events:
    └── 'error' (err: Error)

Request Class (req)
├── Properties:
│   ├── .method: string
│   ├── .url: string
│   ├── .headers: object
│   ├── .body: any
│   ├── .params: object
│   ├── .query: object
│   ├── .cookies: object
│   ├── .path: string
│   ├── .protocol: string
│   ├── .secure: boolean
│   ├── .hostname: string
│   ├── .ip: string
│   ├── .ips: string[]
│   ├── .baseUrl: string
│   ├── .originalUrl: string
│   └── .xhr: boolean
│
├── Methods:
│   ├── .get(field: string): string | undefined
│   ├── .accepts(types: string | string[]): string | false
│   ├── .acceptsLanguages(languages: string | string[]): string | false
│   ├── .acceptsCharsets(charsets: string | string[]): string | false
│   ├── .is(type: string): string | false
│   ├── .range(size: number, options?: object): number[] | undefined
│   └── .param(name: string): string | undefined
│
└── Events:
    └── 'close'

Response Class (res)
├── Properties:
│   ├── .locals: object
│   ├── .headersSent: boolean
│   ├── .statusCode: number
│   └── .statusMessage: string
│
├── Methods:
│   ├── .status(code: number): Response
│   ├── .send(body: string | Buffer | object): Response
│   ├── .json(body: object): Response
│   ├── .jsonp(body: object): Response
│   ├── .sendStatus(statusCode: number): Response
│   ├── .redirect(url: string, status?: number): void
│   ├── .render(view: string, locals?: object, callback?: Function): void
│   ├── .end(data?: string | Buffer): void
│   ├── .set(field: string | object, value?: string): Response
│   ├── .get(field: string): string
│   ├── .cookie(name: string, value: string | object, options?: object): Response
│   └── .clearCookie(name: string, options?: object): Response
│
└── Events:
    └── 'finish'

Next Function
├── Type:
│   └── next(err?: any): void

Middleware Function
├── Signature:
│   └── (req: Request, res: Response, next: Function): void

Error-handling Middleware
├── Signature:
│   └── (err: any, req: Request, res: Response, next: Function): void

Helper Functions (Standalone)
├── express.static(root: string, options?: object): MiddlewareFunction
├── express.json(options?: object): MiddlewareFunction
└── express.urlencoded(options?: object): MiddlewareFunction

```