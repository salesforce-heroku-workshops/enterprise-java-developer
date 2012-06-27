Heroku Enterprise Developer Workshop
====================================

This workshop will give you an introduction to building enterprise Java applications on Force.com and Heroku.  Before you get started you will need to install these prerequisites:

* Java 1.6: [http://oracle.com](http://oracle.com) *** NEED URL ***
* SSH access to heroku.com
    1. Verify with: `telnet heroku.com 22`
    2. *** TODO ***
* Eclipse 3.7: [http://eclipse.org](http://eclipse.org) *** NEED URL ***
* EGit Eclipse Plugin
    1. In Eclipse select `Help` bar
    2. Select `Software Marketplace`
    3. *** TODO ***
* M2E Eclipse Plugin
    1. In Eclipse select `Help` bar
    2. Select `Software Marketplace`
    3. *** TODO ***
* Heroku Eclipse Plugin
    1. In Eclipse select `Help` bar
    2. Select `Software Marketplace`
    3. *** TODO ***
* Heroku Enterprise Trial Sign up *** TODO ***
* Salesforce.com Developer Edition Account *** TODO ***

Now that you have everything installed you will need to configure the Heroku settings in Eclipse.  Navigate to the Eclipse preferences:
* On Windows:

*** TODO ***

* On Mac:

*** TODO ***

* On Linux:

    1. Select `Window` from the menu bar
    2. Select `Preferences`

Now you will configure the API key for Heroku.

1. Select Heroku from the list on the left
2. Enter the email address you used for your Heroku account in the `Email` field
3. Enter your password in the `Password` field
4. Select the `Get API Key`
5. *** TODO: Master password stuff ***

Your API key for Heroku is now setup and since your email address and password are no longer needed, those fields will become blank.  *** VERIFY ***  You will now need to setup an SSH key for authenticating to Heroku for file uploads using Git.  You can use an existing SSH key or create a new one.  If you aren't sure if you have an existing SSH key, then create a new one.

To use an existing SSH key:
1. Select `Load SSH Key`
2. Locate the public key on your file system
3. Select `Ok`

To create a new SSH key:
1. Select `Generate` to be taken to the SSH2 preferences
2. Select the `Key Management` tab
3. Select `Generate RSA Key`
4. Optionally set a password for the SSH key
5. Select `Save Private Key...`
6. Save the key to the file system (usually SSH keys are stored in a `.ssh` directory inside the user's home directory)
7. Select the text for the SSH public key from the field beneath `You can paste this public key into the remote authorized_keys file:`
8. Copy the text for the public key into your copy buffer / clipboard
9. Select `Heroku` from the preferences list on the left
10. Paste the SSH public key text into the `SSH Key` field
11. Verify that your Heroku preferences look similar to this:
    *** TODO: SCREENSHOT ***
11. Select `Ok` to save all of the settings


Chapter 1: Getting Started with Spring MVC on Heroku
----------------------------------------------------

Now that everything is setup you will create your first application on Heroku using a Spring MVC template application.

1. From the Eclipse menu bar select File
2. Select New
3. Select Project
4. Expand the `Heroku` section
5. Select `Create Heroku App from Template`
6. *** TODO ***

You now have a simple Spring MVC application in Eclipse which has also been deployed on Heroku.  To view the application on Heroku:
1. *** TODO ***

The default page of the application is the instructions for pulling the application into your local development environment.  You've already done that using the Heroku Eclipse plugin, so you won't need to follow those.  To see the simple CRUD application in action select the `people page` link where you should see:
*** TODO: SCREENSHOT ***

Add a new `Person` to the database to verify the application is working correctly.

Back in Eclipse you can view the details about the application on Heroku:
1. *** TODO ***

This project uses Apache Maven for managing it's dependencies and to build the project.  You can see the dependency and build definition in the `pom.xml` file.  Among the dependencies you will see dependencies for Spring MVC.  You will also see a section for the `maven-dependency-plugin` which copies the `webapp-runner` dependency into a directory when the Maven `package` phase runs.  This makes it easy to run the application with `webapp-runner` which is a simple wrapper around Apache Tomcat.

Heroku is instructed to run the application using a file named `Procfile` in the project's root directory.  The `Procfile` for this application contains:
    
    web: java $JAVA_OPTS -jar target/dependency/webapp-runner.jar --port $PORT target/*.war

This tells Heroku that for the `web` process run the `webapp-runner` with Java, listening to the HTTP port specified by the `PORT` environment variable and running the WAR file that was created by the Maven build.

You can run this application locally within Eclipse to test changes before deploying them on Heroku.  Setup a new `Run Configuration` for `webapp-runner`:
1. In Eclipse select `Run` from the menu bar
2. Select `Run Configurations...`
3. Select `Java Application`
4. Select the `New launch configuration` icon (above the filter field)
5. In the `Name` field enter `webapp-runner for x` (replacing x with your project name)
6. In the `Main class` field enter `webapp.runner.launch.Main`
7. Select the `Browse...` button next to the `Project` field
8. Select your project from the list
9. Select the `Arguments` tab
10. In the `Program arguments` field enter `src/main/webapp`  *** TODO: VERIFY ON WINDOWS ***
11. Select `Run`

Test that the application is working locally by opening [http://localhost:8080](http://localhost:8080) in your browser.  You should see something similar to what you saw with your application on Heroku.  Test that the application is working with the database by visiting the `people page` and adding a `Person`.  When running locally this application uses an in-memory database.

Now lets make a simple change to the application, test that change locally, and then deploy the change on Heroku.

1. Open the `src/main/webapp/WEB-INF/jsp/people.jsp` file
2. Locate the line that displays each person's name:

         <td>${person.lastName}, ${person.firstName}</td>

3. Change the line to be:

         <td>${person.firstName} ${person.lastName}</td>

4. Reload the [http://localhost:8080/people/](http://localhost:8080/people/) page in your browser and you should now see the names displayed with the first name first.

Now you will deploy this change on Heroku.

1. Select the project's context menu (right-click on the project in the `Project Explorer` panel
2. Select `Team`
3. Select `Commit...`
4. Enter a `Commit message` like `Flipped first and last name`
5. Select `Commit`

This commits your changes to your local Git repository.  Those changes need to be pushed to Heroku.

1. Select the project's context menu
2. Select `Team`
3. Select `Push to Upstream`

This process will take about a minute to run.  The changes are uploaded to Heroku where Heroku runs the Maven build on the project and then deploys the new version.  Once that process is complete you should see a confirmation like the following:

*** TODO: SCREENSHOT ***

Now verify your changes are live on Heroku by visiting your application's `people page` (the `herokuapp.com` version, not the `localhost` version).

To view the logs for your application on Heroku:
1. *** TODO ***


* Managing your application
* Viewing your application processes (View -> My Heroku applications)
* Scale
* Collaborators
* Adding a collaborator
* Changing ownership
* Environment configuration
* Talk about addons (short blurb with a link to addons.heroku.com)
* Add/ Update / Delete
* Refresh
* Destroying your applications


Chapter 2: Building a RESTful API
---------------------------------
* Adding JSON nature to the "Person" entity
* Adding a JSP with JQuery to invoke the JSON


Chapter 3: Integating with Force.com
-----------------------------------
* Clone the "Spring MVC + Force.com template"
* Setting up Remote Access (Salesforce.com screenshots)
* Local
* Heroku
* Adding the environment variables on Heroku
* OAuth Key, Secret
* Walkthrough of the application
* dependencies
* Spring config - OAuth
* web.xml - Filters
* Code walkthrough (DAO)
* Running your application locally
* Run configuration
* Setting up Environment config (OAUTH Key, Secret)
* Test your your app locally
* Testing on Heroku
* Make a change: Adding a "Twitter handle" to Contact
* Adding a new field to contact
* Updating Person.java to include the new field
* Updating the DAO (SOQL Query)
* Modifying the JSP to include Twitter handle
* Deploying your changes to Heroku
* Testing your app on Heroku


Chapter 4: Distributed Sessions on Heroku
-----------------------------------------
* Distributed Sessions with Memcache
* Adding Memcache addon to your app
* Updating Procfile to use "memcache" option
* Deploying the changes to Heroku


Chapter 5: Real time Push notifications
---------------------------------------
* Adding "Pusher" (or PubNub) addon
* Updating dependencies to pom.xml
* Updating the "create" method to include a Push notification
* Updating the JSP to subscribe for Push notifications


Chapter 6: Monitoring and Managing your app
-------------------------------------------
* Adding the New Relic addon
* Java agent setup
* Procfile updates
* Deploying your changes to Heroku
* New Relic Dashboard walkthrough


Chapter 7: Searching Logs with Papertrail
-----------------------------------------
* Adding the "Papertrail Addon"
* Search your logs in real time
* Setting up an email alert based on logs
