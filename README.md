# Documentation
## Error codes
| Error Code | Description           |
|------------|-----------------------|
| 400        | Bad Request           |
| 401        | Not authorized        |
| 402        | No data               |
| 500        | Internal server error |
<br>

### - Base url: /evcharge/api
## Login & Logout 
| Endpoint | Method | Parameters                                                                 | Description                                                                                 | Response                                                         |
|----------|--------|----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|------------------------------------------------------------------|
| /login   | POST   | username and password as application/x-www-form-urlencoded                 | If the credentials are valid the user will be logged in and a new JWT token will be created | Status Code: 200 <br> JSON:<br>{<br> "token":"valid_token"<br>} |
| /logout  | POST   | A valid JWT token in a custom HTTP header field named "X-OBSERVATORY-AUTH" | If the token is valid the user will be logged out                                           | Status Code: 200                                                 |
## Helper endpoints
| Endpoint             | Method | Parameters | Description                                                                                                                                | Response                                                                                                                                                     |
|----------------------|--------|------------|--------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /admin/healthcheck   | GET    | None       | Checks if there is an alive connection with the database                                                                    | On success:<br> Status Code: 200<br> JSON:<br> {<br>"status":"OK"<br>}<br><br>On failure: <br> Status Code: 500 <br> JSON: <br> {<br>"status":"failed"<br>}     |
| /admin/resetsessions | POST   | None       | Delete all the charging sessions from the database and initialise default admin user account with the default credentials from config.json | On success: <br> Status Code: 200 <br> JSON: <br> {<br>"status":"OK"<br>}<br><br> On failure: <br> Status Code: 500 <br> JSON: <br> {<br>"status":"failed"<br> } |
## Admin endpoints
| Endpoint                             | Method | Parameters                      | Descritpion                                                                      | Response                                                                                                                                           |
|--------------------------------------|--------|---------------------------------|----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| /admin/usermod/{username}/{password} | POST   | username<br>password            | Registers a new user or changes its password if the user exists already          | Status Code: 200                                                                                                                                   |
| /admin/users/{username}              | GET    | username                        | Returns information about the given user                                         | Status Code: 200<br>JSON:<br>{<br>"username":"username"<br>"password":"encrypted_password"<br>"role":"admin_or_user"<br>"token":"login_token"<br>} |
| /admin/system/sessionsupd            | POST   | csv file as multipart/form-data | Takes a csv file with charging sessions events and inserts them into the database | Status Code: 200                                                                                                                                   |
# Configuration and installation
- In order to start the api server you have to create a file "config.json" in this folder with the following structure:
```json
{   
    "ssl_key_path": "path/to/ssl/key",
    "ssl_cert_path": "path/to/ssl/cert",
    "base_url":   "api/base/url",
    "port":   "port_number",
    "secret": "secret string",
    "db_host": "database hostname",
    "db_username": "database username",
    "db_password": "database password",
    "db_database": "database name",
    "admin_default_username": "admin default username",
    "admin_default_password": "admin default password"
};
```

- Then you run ```npm install``` and then ```node app.js```
- Your api must be up and running