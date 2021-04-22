# Documentation
## Error codes
| Error Code | Description           |
|------------|-----------------------|
| 400        | Bad Request           |
| 401        | Not authorized        |
| 402        | No data               |
| 500        | Internal server error |
<br>

## Login & Logout 
| Endpoint | Method | Parameters                                                                 | Description                                                                                 | Response                                                         |
|----------|--------|----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|------------------------------------------------------------------|
| /login   | POST   | username and password as application/x-www-form-urlencoded                 | If the credentials are valid the user will be logged in and a new JWT token will be created | Status Code: 200 <br> JSON:<br>{<br> "token":"valid_token"<br>} |
| /logout  | POST   | A valid JWT token in a custom HTTP header field named "X-OBSERVATORY-AUTH" | If the token is valid the user will be logged out                                           | Status Code: 200                                                 |
<br>

## Helper endpoints
| Endpoint             | Method | Parameters | Description                                                                                                                                | Response                                                                                                                                                     |
|----------------------|--------|------------|--------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /admin/healthcheck   | GET    | None       | Checks if there is an alive connection with the database                                                                    | On success:<br> Status Code: 200<br> JSON:<br> {<br>"status":"OK"<br>}<br><br>On failure: <br> Status Code: 500 <br> JSON: <br> {<br>"status":"failed"<br>}     |
| /admin/resetsessions | POST   | None       | Delete all the charging sessions from the database and initialise default admin user account with the default credentials from config.json | On success: <br> Status Code: 200 <br> JSON: <br> {<br>"status":"OK"<br>}<br><br> On failure: <br> Status Code: 500 <br> JSON: <br> {<br>"status":"failed"<br> } |
<br>

## Admin endpoints
### The following endpoints require a valid JWT token  which belongs to an admin account
| Endpoint                             | Method | Parameters                      | Descritpion                                                                      | Response                                                                                                                                           |
|--------------------------------------|--------|---------------------------------|----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| /admin/usermod/{username}/{password} | POST   | username<br>password            | Registers a new user or changes its password if the user exists already          | Status Code: 200                                                                                                                                   |
| /admin/users/{username}              | GET    | username                        | Returns information about the given user                                         | Status Code: 200<br>JSON:<br>{<br>"username":"username"<br>"password":"encrypted_password"<br>"role":"admin_or_user"<br>"token":"login_token"<br>} |
| /admin/system/sessionsupd            | POST   | csv file as multipart/form-data | Takes a csv file with charging sessions events and inserts them into the database | Status Code: 200                                                                                                                                   |
<br>

## Data Endpoints
#### All following endpoints require a valid JWT token 

### **Sessions Per Point**
| Endpoint                                                           | Method | Parameters                                        | Description                                                                  |
|--------------------------------------------------------------------|--------|---------------------------------------------------|------------------------------------------------------------------------------|
| /SessionPerPoint/{pointID}/{yyyymmdd_date_from}/{yyyymmdd_date_to} | GET    | pointID<br>yyyymmdd_date_from<br>yyyymmdd_date_to | Returns a JSON with a list of charging sessions for given point ID and dates |
<br>

**JSON response**
| Field                    | Description                                                  |
|--------------------------|--------------------------------------------------------------|
| Point                    | Point ID                                                     |
| PointOperator            | Point Operator                                               |
| RequestTimestamp         | Timestamp of the API call                                    |
| PeriodFrom               | Date_from parameter                                          |
| PeriodTo                 | Date_to parameter                                            |
| NumberOfChargingSessions | Number of charging sessions for the given parameters         |
| ChargingSessionsList:    | List of charging sessions with size NumberOfChargingSessions |
| -------------------     |                 -------------------      |
| **ChargingSessionsList Field** | **Description**                    |      
| SessionIndex               | Index of session in the list           |
| SessionID                  | Session ID                             |
| StartedOn                  | Starting timestamp of the session      |
| FinishedOn                 | Finish timestamp of the session        |
| Protocol                   | Charging protocol used                 |
| EnergyDelivered            | Energy delivered in this session in kWh |
| Payment                    | Payment Method                         |
| VehicleType                | Type of the charged vehicle            |
<br>

### **Sessions Per Station**
| Endpoint                                                           | Method | Parameters                                        | Description                                                                  |
|--------------------------------------------------------------------|--------|---------------------------------------------------|------------------------------------------------------------------------------|
| /SessionPerStation/{stationID}/{yyyymmdd_date_from}/{yyyymmdd_date_to} | GET    | stationID<br>yyyymmdd_date_from<br>yyyymmdd_date_to | Returns a JSON with a list of charging sessions for given station ID and dates |
<br>

**JSON response**
| Field                    | Description                                                  |
|--------------------------|--------------------------------------------------------------|
| StationID                    | Station ID                                                     |
| Operator            | Station Operator                                               |
| RequestTimestamp         | Timestamp of the API call                                    |
| PeriodFrom               | Date_from parameter                                          |
| PeriodTo                 | Date_to parameter                                            |
| TotalEnergyDelivered | Total energy delivered from this station in the given time interval         |
| NumberOfChargingSessions   | Number of charging sessions in the given time interval |
| NumberOfActivePoints    | Number of distinct charging points used |
| SessionsSummaryList:    | List of charging sessions per point with size NumberOfActivePoints  |
|-------------------|-------------------|
| **SessionsSummaryList Field** | **Description**                          
| PointID               | Point ID           |
| PointSessions                  | Number of charging sessions for this point                             |
| EnergyDelivered                  | Total energy delivered from thiw point in kWh      |
<br>

### **Sessions Per EV (Electrical Vehicle)**
| Endpoint                                                           | Method | Parameters                                        | Description                                                                  |
|--------------------------------------------------------------------|--------|---------------------------------------------------|------------------------------------------------------------------------------|
| /SessionPerEV/{vehicleID}/{yyyymmdd_date_from}/{yyyymmdd_date_to} | GET    | vehicleID<br>yyyymmdd_date_from<br>yyyymmdd_date_to | Returns a JSON with a list of charging sessions for given vehicle ID and dates |
<br>

**JSON response**
| Field                    | Description                                                  |
|--------------------------|--------------------------------------------------------------|
| VehicleID                    | Vehicle ID                                                     |
| RequestTimestamp         | Timestamp of the API call                                    |
| PeriodFrom               | Date_from parameter                                          |
| PeriodTo                 | Date_to parameter                                            |
| TotalEnergyConsumed | Total energy consumed from this vehicle in the given time interval         |
| NumberOfVisitedPoints   | Number of distinct points where this vehicle has been charged |
| NumberOfVehicleChargingSessions    | Number of charging sessions for this vehicle in the given time interval |
| VehicleChargingSessionsList:    | List of charging sessions size NumberOfVehicleChargingSessions  |
|-------------------|-------------------|
| **VehicleChargingSessionsList Field** | **Description**                          
| SessionIndex               | Index of session in the list           |
| SessionID                  | Session ID                             |
| EnergyProvider                  | Energy provider name      |
| StartedOn                  | Starting timestamp of the session      |
| FinishedOn                 | Finish timestamp of the session        |
| EnergyDelivered            | Energy delivered to the vehicle in kWh |
| PricePoliyRef                  | Type of pricing policy applied      |
| CostPerkWh                  | Cost per kWh     |
| SessionCost                  | Total cost of the charging session      |
<br>

### **Sessions Per Provider**
| Endpoint                                                           | Method | Parameters                                        | Description                                                                  |
|--------------------------------------------------------------------|--------|---------------------------------------------------|------------------------------------------------------------------------------|
| /SessionPerProvider/{providerID}/{yyyymmdd_date_from}/{yyyymmdd_date_to} | GET    | providerID<br>yyyymmdd_date_from<br>yyyymmdd_date_to | Returns a list of JSONs of charging sessions for given provider ID and dates |
<br>

**List of JSONs response**
| Field                    | Description                                                  |
|--------------------------|--------------------------------------------------------------|
| ProviderID                    | Provider ID                                                     |
| ProviderName         | Provider Name                                  |
| StationID               | Station ID                                          |
| SessionID                 | Session ID                                            |
| VehicleID | Vehcile ID         |
| StartedOn                  | Starting timestamp of the session      |
| FinishedOn                 | Finish timestamp of the session        |
| EnergyDelivered            | Energy delivered in this session in kWh |            
| PricePoliyRef                  | Type of pricing policy applied      |
| CostPerkWh                  | Cost per kWh     |
| SessionCost                  | Total cost of the charging session      |
<br>

### **Providers Info**
| Endpoint                                                           | Method | Parameters                                        | Description                                                                  |
|--------------------------------------------------------------------|--------|---------------------------------------------------|------------------------------------------------------------------------------|
| /ProvidersInfo | GET    | None | Returns a list of JSONs with info about the energy providers |
<br>

**List of JSONs response**
| Field                    | Description                                                  |
|--------------------------|--------------------------------------------------------------|
| ProviderID                    | Provider ID                                                     |
| ProviderName         | Provider Name                                  |

<br>
*Date format in response objets is "YYYY-MM-DD HH:MM:SS"<br>
*Session in session lists are in ascending order by start and finish times
<br>

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
