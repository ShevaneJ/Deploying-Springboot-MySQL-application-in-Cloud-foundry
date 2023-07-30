# Deploying-Springboot-MySQL-application-in-Cloud-foundry


Steps to deploy the springboot application in cloud foundry:

For Backend springboot application:
1. Add dependancy for the war file.
2. Perform mvn clean and install (either in command prompt or in IDE)-> A war file will be created in the backend source target folder. 
3. Make sure jdk is added in the Java->installed jre in the spring tool/eclipse and build the war file.
4. Create a new folder for deployment and copy the war file.
5. Write a manifest.yml file( manifest file below) and add that into the deployment folder.
#
manifest.yml
applications:
- name: 'Application name'
  stack: cflinuxfs3
  memory: 2G
  instances: 1
  # timeout: 1000
  random-route: false
  health-check-type: process
  path: ./app.war
  buildpacks:
    - java_buildpack_offline_latest
  env:
    JAVA_OPTS: '-Djava.security.egd=file:///dev/urandom -Dhttp.sslVerify=false -Duser.timezone=UTC'

  
6. Open command prompt in the deployment folder.
7. run:  cf push (app-name) -p (war/jar)name.
8. Open the deployed app and view whether it hit the target page or not.
 

Database:
1. Create a new external service for database in cloud foundry in cmd prompt. It will be in cloud foundry.
Commands:
cf create-service p.mysql db-small db-name -c "{\"enable_external_access\":true}"

2. Create a service key for the database.
3. Bind this database service to the backend app deployed in the cloud foundry.
4. Open the service key and copy the hostname, username, url and password.
5. Add those in the springboot application.properties

#Application properties 
## Spring DATASOURCE (DataSourceAutoConfiguration & DataSourceProperties)
spring.datasource.url = jdbc:mysql://url for cloud/schema-name?allowPublicKeyRetrieval=true&useSSL=false
spring.datasource.password =
spring.datasource.user =

## Hibernate Properties

# The SQL dialect makes Hibernate generate better SQL for the chosen database
spring.jpa.database-platform=org.hibernate.dialect.MySQL8Dialect

# Hibernate ddl auto (create, create-drop, validate, update)
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.hibernate.ddl-auto = update
spring.jpa.properties.hibernate.globally_quoted_identifiers=true
spring.datasource.connection = DriverManager.getConnection(jdbc:mysql://url/schema name?allowPublicKeyRetrieval=true&useSSL=false,user name,password);
 
6. Again build the war file and push.
