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
   - environments
2. Master registry  
   - DB structure
   - Tables, definitions and relationships.
   - Graphql usage
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
- #### 3.5. /api/dailyreport/drug
- #### 3.6. /api/dailyreport/services
- #### 3.7. /api/druginventory
- #### 3.8. /:lang/:pagename
- #### 3.9. /api/phcchange
- #### 3.10. /api/users
- #### 3.11. /api/users/reset-password
- #### 3.12 /graphql