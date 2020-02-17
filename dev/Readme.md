# To be written . . .  

### Rough draft of things to be covered.
1. UHC app  
   - DB structure
   - Tables, definitions and relationships.
   - List of API's
   - Graphql usage
   - Babel usage (not mandatory)
   - App structure
   - Migrations, seeders and usage
2. Master registry  
   - DB structure
   - Tables, definitions and relationships.
   - Application usage.
3. Report tool  
   - API's and usage
   - Working.


# 1. Overview  
The application is built in the format of isolating the frontend and backend (Please refer learning section ---).   &nbsp;

The frontend application is a ReactJS app which has redux for state management. The application enforces two layers for networking.  
### a. API/ N/W req:
All the API requests are done through a generic requesting client such as axios.

### b. GraphQL req:  
All the graphql requests are done through the apollo graphql modules.  

### c. API server
The API server is built using an express server ejected through an express-generator without any view handler (considering that this is a completely API server). And the application's migrations are managed through the ORM.
The app uses a postgres server as a data source and redis as a intermediatory data exchange. The API server is connected to the postgresql using an ORM (sequelize). To be explained later. 
&nbsp; 
### d. Environments and transpilers
The application also uses a babel transpiler to convert es6 and above modules.  
The application's environments are switched through the variables in the env files. 
&nbsp;
### e. Master-reg app
The master registry app uses an abstracted graphql layer called as Postgraphile. These produce all the queries out of box from the database. So the layer of ORM is not required till now.  

***

# 2. DB structure:  
The list of tables and their definitions are as follows;
- #### 2.1 user_system_status  
Stores the user's system status.
- #### 2.2 SequelizeMeta  
Stores the list of migrations which has been run. If any migrations are to be manually added, the files names should be added here.
- #### 2.3 blocks_masters  
Stores the list of blocks of a district
- #### 2.4 genders_masters  
Stores all the unique genders with their codes
- #### 2.5 hud_masters  
Stores the list of hud of a district.
- #### 2.6 error_handles  
Stores all the error captured at the application
- #### 2.7 roles_masters  
Specifies all the roles present in the application.
- #### 2.8 specimens_masters  
Stores all the specimens used in lab's.
- #### 2.9 phc_reports  
Stores all the reports generated from PHC/HSC.
- #### 2.10 phc_report_services  
Stores all the service reports generated from PHC/HSC.
- #### 2.11 user_hwc_statuses  
Stores all the HSC status (profile)
- #### 2.12 user_phc_statuses  
Stores all the PHC status (profile)
- #### 2.13 validations_masters  
Stores all the validation messages present.
- #### 2.14 symptoms_masters  
Stores all the symptoms of a disease.
- #### 2.15 syndromes_masters  
Stores all the syndromes (deprecated).
- #### 2.16 users_trans  
Stores all the users and its profile from the application.
- #### 2.17 uhc_tickets  
Stores all the user generated tickets from application.
- #### 2.18 patient_det_trans  
Stores all the entries of patient's visit details.
- #### 2.19 block_masters  
Stores all the blocks of the district(deprecated).
- #### 2.20 countries_masters  
Stores all the countries and their codes.
- #### 2.21 diagnosis_masters  
Stores all the diagnosis conditions of all the diseases.
- #### 2.22 districts_masters  
Stores all the districts of all states.
- #### 2.23 drugs_masters  
Stores all the drugs used in the institutions.
- #### 2.24 labtest_masters  
Stores all the lab tests taken in the visits.
- #### 2.25 phc_masters  
Stores all the phc's list under a hud.
- #### 2.26 services_masters  
Stores all the list of services offered in the institution.
- #### 2.27 states_masters  
Stores all the states present in the country.
- #### 2.28 drugs_inventories  
Stores all the current drug inventory's status.
- #### 2.29 uhc_pds_map_member_data  
Stores the patient pulled from master registry.
- #### 2.30 patient_labs  
Stores the lab tests taken during a patient visit.
- #### 2.31 user_offline_status  
Stores all the user's offline status.
- #### 2.32 patient_drugs  
Stores the drugs offered during a patient visit.
- #### 2.33 patient_diagnoses  
Stores the diagnosis conditions patient has informed during a patient visit.

***


# 3. List of API's:  
- #### 3.1. /lineentry  
This API is to save the patient'
s visit details. The visit details are saved into the following tables. The primary details of the visit are saved into the ***patient_det_trans***. Also the patient profile and ID's are saved into ***uhc_pds_map_member_data*** Entry of the visit is generated and are being referenced to other tables.  
The patient's lab test details are saved into ***patient_labs***.  
The drugs issued to the patient's are saved into ***patient_drugs***.   
The diagnosis condition's found with the patient are saved into ***patient_diagnoses***.   
If the visit is referred out to another institution, then a copy of all the visit details are being copied and saved into the respective tables with their associated ID's.
- #### 3.2. /lineentry/update  
The data received from the ***/lineentry*** data is being modified using this API.  
The data is modified in the similar fashion to that of creation of the line entry.
- #### 3.3. /captcha  
This API is to generate an captcha to be displayed in the frontend along with the hash. An SVG format image is being generated to be displayed. It uses a npm package ***"svg-captcha"***.
- #### 3.4. /errorHandler  
All the errors which are captured in the frontend is being captured is being saved into the table ***error_handles***. 
- #### 3.5. /api/dailyreport/drug  
This API is to generate save all the report of the current day's drug issue is saved. It is saved into the table ***phc_report***.
- #### 3.6. /api/dailyreport/services  
This API is to generate save all the report of the current service offered to all the patient. It is saved into the table ***phc_report_services***.
- #### 3.7. /api/druginventory  
This API saves all the current day's inventory supply. All the data is being saved into the table ***drugs_inventory***. Current day's supply is being displayed in the drug utilization page. And the calculation happens as this.  
The drug issued is displayed alongside with the current day's issue calculated from the line entries received.
- #### 3.8. /:lang/:pagename(deprecated) :
This API is used to pull the page's translated content from the server based on the languages and pagename sent as query parameters.
- #### 3.9. /api/phcchange  :
This API works in the similar fashion to ***'/lineentry'*** where only the institution which created it alone is changed and created as new visit. This happens when institution "A" sends a patient from his institution to insitution "B".
- #### 3.10. /api/users  :
This API is to get a login token, by sending out a username and password. The bearer token sent out acts as the authentication for all the API's. It is used in the Authorization header.
- #### 3.11. /api/users/reset-password
This API is used to reset the password for the particular user. A randomly generated password is sent out to the user's email. And the user can update it in his profile section.
- #### 3.12 /graphql
This API acts as the graphql endpoint for the application (refer the connection dir for further details).


> ## For internal details, check the application which says method wise, route wise, file wise details about them.

# 4. GraphQL usage:
Throughtout the application all of the queries and mutations are being done throught graphql. Only a very few custom queries are written as API's to involve more custom logics.

All the queries and mutations are written in the api/connection/resolvers file. 
The queries and mutations are converted into a graphql schema and is passed into a middleware of express-graphql which also takes in a parameter of models of the db to make queries.
This happens in the below context.
```
const server = new ApolloServer({
  typeDefs,
  resolvers,
  context: {
    models
  },
});
```  
# 5. Babel usage:

The application is written using es6. When the app is run locally using dev mode or debug mode, the application uses nodemon and runs the code using babel compiler.  
babel-node is the compiler used. Once when the local environment is done, the source code is built wherever required using the babel-cli and ran.  
The babel plugins used are specified in the .babelrc file.

```
{
    "presets": [
        "@babel/preset-env",
    ],
    "plugins": ["transform-object-rest-spread", "@babel/transform-runtime"]
}
```

All the commands to run are specified in the package.json files. Commands to run in all environment are specified such as i.e dev mode, prod mode, build source code etc.

# 6. App structure:  
the application's code can split into services.
a. Frontend
b. API server.

## 6.1 Frontend: 
The frontend app is built using a create-react-app template and ejected. It uses redux for state management. 
Once when a change is made into the application's source code, the build command is run to generate the static build.
And the build is being added into the nginx's default folder so as to make nginx take care of the build serving. The steps are defined in the dockerfile.

the source code structure is as follows:  

1. config:  
   The config contains the webpack configurations and environment configs.
2. nginx:  
   The nginx directory contains the configuration which is customly written to use this instead of the default one.
3. public:  
   the public folder of the react app.
4. scripts:  
   react scripts.
5. src:  
   The list of components is present here.
6. Dockerfile_Dev:  
   The dockerfile to be used.
7. Dockerrun.aws.json:  
   Deprecated.
8. package.json:  
   The package.json file of the react-app.

## 6.2 API server:  

The backend application is an express app which runs on top of apollo-graphql. The application is run locally using npm scripts. Once the changes / modifications are done, the docker image is built the, build files with the dependencies are pushed to the repository.  

The source code structure is as follows:  
1. api/bin:  
   The application's http server is present here.
2. api/caching:  
   All the data which is being cached is written in this directory.
3. api/connection:  
   All the db related queries, connections etc are present here.
4. api/controllers:  
   The controllers for every routes and rules are present.
5. api/cronjobs:  
   NOTE:DEPRECATED.
6. api/locales:  
   NOTE:DEPRECATED.
7. api/models:  
   All the models used by the sequelize ORM is present here.
8. api/modules:  
   All the modules used throughout the application which supports the app usage.
9. api/queries:  
   List of queries hit through master registry. arguments are passed dynamically to generate the query.
10. api/routes:
    All the http routing from the application to the controllers.
11. api/util:  
   misc/ utility methods.
12. api/app.js and api/op_conf.js:  
    app.js is the entry point from www and has all the routing, middlewares etc. op_conf.js has all the configuration and environment files.
13. migrations and seeders:  
   The db's migration files is present here. the initial set of changes in the db are preset in the seeders to ease up the work. The migration's are maintained through the sequelize-cli which pulls the environment details from config and .env files.
14. .babelrc:  
    the babel presets and plugins for transpilation.
15. Dockerfile:  
    The dockerfile to build the application.
16. package.json:  
    dependencies and other scripts.

> The application contains the necessary comments to understand the basic usage of every method etc.

# 2. Master registry:

## 2.1 Overview:  
Master registry (a) MR is where all the source data such as population data and address data is present. The connection between address data and the population data is as follows.


The image explains the hierarchy and the flow of data between tables.

## 2.2 DB structure and table definitions:  

For every head table, there is a member table associated which holds the population for that particular districts.

## 2.3 APP overview:  
The application is a very crude app built using express and uses postgraphile as a simple middleware to generate all the queries. check postgraphile for its usage.

All the logics are handled at the frontend wherever the application data is being pushed to.
```
  app.use(postgraphile({
      host:conf.MASTER_HOST,
      user:conf.MASTER_USER,
      password:conf.MASTER_PASSWORD,
      database:conf.MASTER_NAME,
      max:756,
      idleTimeoutMillis:30000,
      connectionTimeoutMillis:2000
  },'public',{
      graphiql:true,
      retryOnInitFail:true,
      bodySizeLimit:'100MB',
      enableQueryBatching:true,
      // queryCacheMaxSize:"100MB",
      watchPg:true,
      appendPlugins: [
          ConnectionFilterPlugin,
          PostGraphileFulltextFilterPlugin,
      ],
  }))
```

# 3. Report tool:
## 3.1 Overview:  
The application is an exact copy of the UHC API server but only contains a few extra queries which are used to generate the data required for reporting.

## 3.2 Workflow:  
The application's workflow happens as follows:
1. The application has a cron which runs exactly at 11pm. 
2. the cron pulls all the users for whom the report has to be genrated.
3. reports for individual users are being generated and then pushed into a s3 bucket.
4. The object URL is then mailed out to those users.

The bucket is pointed to a cloudfront server and is server from there with the base url of assets.uhcitp.in