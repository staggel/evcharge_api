# Back-end directory

- In order to start the api server you have to create a file "config.js" in this folder with the following structure:
```javascript
module.exports = {
    ssl_key_path: "path/to/ssl/key",
    ssl_cert_path: "path/to/ssl/cert",
    base_url:   "api/base/url",
    port:   ${PORT_NUMBER},
    secret: "secret string",
    db_host: "database hostname",
    db_username: "database username",
    db_password: "database password",
    db_database: "database name",
    admin_default_username: "admin default username",
    admin_default_password: "admin default password"
};
```

- Then you run ```npm install``` and then ```node app.js```
- Your api must be up and running