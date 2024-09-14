# ExpressJS

express() - Application Class

├── Methods:

│    &emsp; &emsp;  ├── .get(path: string, callback: Function): Application

│    &emsp; &emsp;  ├── .post(path: string, callback: Function): Application

│    &emsp; &emsp;  ├── .put(path: string, callback: Function): Application

│    &emsp; &emsp;  ├── .delete(path: string, callback: Function): Application

│    &emsp; &emsp;  ├── .use(path: string, callback: Function): Application

│    &emsp; &emsp;  ├── .listen(port: number, callback?: Function): Server

│    &emsp; &emsp;  ├── .set(setting: string, value: any): Application

│    &emsp; &emsp;  ├── .enable(setting: string): Application

│    &emsp; &emsp;  ├── .disable(setting: string): Application

│    &emsp; &emsp;  ├── .engine(ext: string, callback: Function): Application

│    &emsp; &emsp;  ├── .route(path: string): Router

│    &emsp; &emsp;  ├── .param(name: string, callback: Function): Application

│    &emsp; &emsp;  └── .all(path: string, callback: Function): Application

│

├── Properties:

│    &emsp; &emsp;  └── .locals: object

│

└── Events:

   &emsp; &emsp;  └── 'mount' (parent: Application)

<br><br>

Router Class

├── Methods:

│    &emsp; &emsp;  ├── .get(path: string, callback: Function): Router

│    &emsp; &emsp;  ├── .post(path: string, callback: Function): Router

│    &emsp; &emsp;  ├── .put(path: string, callback: Function): Router

│    &emsp; &emsp;  ├── .delete(path: string, callback: Function): Router

│    &emsp; &emsp;  ├── .use(path: string, callback: Function): Router

│    &emsp; &emsp;  ├── .param(name: string, callback: Function): Router

│    &emsp; &emsp;  └── .all(path: string, callback: Function): Router

│

└── Events:

&emsp; &emsp; └── 'error' (err: Error)

<br><br>


Request Class (req)

├── Properties:

│    &emsp; &emsp;  ├── .method: string

│    &emsp; &emsp;  ├── .url: string

│    &emsp; &emsp;  ├── .headers: object

│    &emsp; &emsp;  ├── .body: any

│    &emsp; &emsp;  ├── .params: object

│    &emsp; &emsp;  ├── .query: object

│    &emsp; &emsp;  ├── .cookies: object

│    &emsp; &emsp;  ├── .path: string

│    &emsp; &emsp;  ├── .protocol: string

│    &emsp; &emsp;  ├── .secure: boolean

│    &emsp; &emsp;  ├── .hostname: string

│    &emsp; &emsp;  ├── .ip: string

│    &emsp; &emsp;  ├── .ips: string[]

│    &emsp; &emsp;  ├── .baseUrl: string

│    &emsp; &emsp;  ├── .originalUrl: string

│    &emsp; &emsp;  └── .xhr: boolean

│

├── Methods:

│    &emsp; &emsp;  ├── .get(field: string): string | undefined

│    &emsp; &emsp;  ├── .accepts(types: string | string[]): string | false

│    &emsp; &emsp;  ├── .acceptsLanguages(languages: string | string[]): string | false

│    &emsp; &emsp;  ├── .acceptsCharsets(charsets: string | string[]): string | false

│    &emsp; &emsp;  ├── .is(type: string): string | false

│    &emsp; &emsp;  ├── .range(size: number, options?: object): number[] | undefined

│    &emsp; &emsp;  └── .param(name: string): string | undefined

│

└── Events:

&emsp; &emsp;└── 'close'

<br><br>

Response Class (res)

├── Properties:

│    &emsp; &emsp;  ├── .locals: object

│    &emsp; &emsp;  ├── .headersSent: boolean

│    &emsp; &emsp;  ├── .statusCode: number

│    &emsp; &emsp;  └── .statusMessage: string

│

├── Methods:

│    &emsp; &emsp;  ├── .status(code: number): Response

│    &emsp; &emsp;  ├── .send(body: string | Buffer | object): Response

│    &emsp; &emsp;  ├── .json(body: object): Response

│    &emsp; &emsp;  ├── .jsonp(body: object): Response

│    &emsp; &emsp;  ├── .sendStatus(statusCode: number): Response

│    &emsp; &emsp;  ├── .redirect(url: string, status?: number): void

│    &emsp; &emsp;  ├── .render(view: string, locals?: object, callback?: Function): void

│    &emsp; &emsp;  ├── .end(data?: string | Buffer): void

│    &emsp; &emsp;  ├── .set(field: string | object, value?: string): Response

│    &emsp; &emsp;  ├── .get(field: string): string

│    &emsp; &emsp;  ├── .cookie(name: string, value: string | object, options?: object): 
Response
│    &emsp; &emsp;  └── .clearCookie(name: string, options?: object): Response

│

└── Events:

 &emsp; &emsp;    └── 'finish'



<br><br>

Next Function

├── Type:

│    &emsp; &emsp; └── next(err?: any): void

<br><br>

Middleware Function

├── Signature:

│    &emsp; &emsp;  └── (req: Request, res: Response, next: Function): void

<br><br>

Error-handling Middleware

├── Signature:

│    &emsp; &emsp;  └── (err: any, req: Request, res: Response, next: Function): void

<br><br>

Helper Functions (Standalone)

├── express.static(root: string, options?: object): MiddlewareFunction

├── express.json(options?: object): MiddlewareFunction

└── express.urlencoded(options?: object): MiddlewareFunction

