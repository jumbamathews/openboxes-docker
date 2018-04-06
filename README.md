# openboxes-docker

This project contains the relevant files to run OpenBoxes using Docker.  It utilizes docker-compose, which enables connecting several independent
dockerized services together (in this case MySQL and Grails), and maintaining configuration for these within a docker-compose.yml file.

If you haven't already done do, you will need to [Install Docker](https://docs.docker.com/) and [Install Docker Compose](https://docs.docker.com/compose/)

### Step 1:  Adjust the include configuration files as needed

Although this docker-compose project should work without modification, there may be occasions when you would like more control over the
way it operates, given your particular machine configuration.  Most of the included files should be considered defaults that can be changed.  Some common examples:

**The location of openboxes code** - This is assumed to be locally cloned as "openboxes" in a directory at the same level as this docker project.
You can see this in the docker-compose file, which attempts to mount a volume from "../openboxes" on your host into the grails-java-7 container at "/app", which is
where the container assumes that the Grails project is located.

**The Tomcat port** - Default is 8080.  Changing this requires editing the "grails" service settings in docker-compose.yml 
and the grails.serverUrl in openboxes-config.properties

**The Tomcat memory and environment** - Edit the included openboxes-setenv.sh file

**The MySQL database name, port, username, or password** - Changing these requires editing the "db" service settings in docker-compose.yml 
and the dataSource.url, dataSource.username, and/or dataSource.password in openboxes-config.properties

**The MySQL root password** - Edit the "db" service settings in docker-compose.yml 

**The MySQL configuration** - Edit the included openboxes-mysql.conf file

### Step 2:  Build the necessary Docker images and attempt initial run

From the base directory (the one that contains your docker-compose.yml file) run:

`docker-compose up --build`

This will build the grails-java-7 image that is included and referenced as a relative image within the docker-compose.yml file.  It will then try to
fire up the application the first time.

*NOTE*: This will likely fail the first time when Grails attempts to run.  We generally see the following error on first run:

```
groovy.lang.MissingPropertyException: No such property: mergedConfig for class: org.codehaus.groovy.grails.commons.DefaultGrailsApplication
```

### Step 3:  Attempt to start up the service

After step 1 above, Ctrl-C out of it, and attempt to run again, this time omitting the build flag:

`docker-compose up`

This should successfully start up the application.  If any errors occur, these will generally be configuration related.  Fix and repeat this step

Ctrl-C from the window in which you brought the containers up is sufficient to later stop those services.  You can also stop them explicitly from another
terminal window, by running:

`docker-compose down`
