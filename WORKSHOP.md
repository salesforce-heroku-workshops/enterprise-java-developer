Heroku Enterprise Developer Workshop
====================================

This workshop will give you an introduction to building enterprise Java applications on Force.com and Heroku.  Before you get started you will need to install these prerequisites:

* SSH access to heroku.com
    1. If you have telnet installed you can verify this by running the following in a command prompt / terminal:

            telnet heroku.com 22
    2. If the connection is refused then you will need to ask your network administrators to open up SSH access to `heroku.com`
* Java SE 6 - JDK: [http://www.oracle.com/technetwork/java/javase/downloads/index.html](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Eclipse 3.7 or newer (Eclipse IDE for Java EE Developers): [http://www.eclipse.org/downloads/](http://www.eclipse.org/downloads/)
* Maven Eclipse Plugin
    1. In Eclipse select `Help` bar
    2. Select `Eclipse Marketplace...`
    3. In the `Find` field enter `Maven`
    4. Select `Go`
    5. Select `Install` on the `Maven Integration for Eclipse` item
    6. Follow the dialogs to complete the installation
* Heroku Eclipse Plugin
    1. In Eclipse select `Help` bar
    2. Select `Install New Software...`
    3. Select `Add...`
    4. In the `Location` field enter: http://eclipse-plugin.herokuapp.com/install
    5. Select `Ok`
    6. Once `Heroku Eclipse Integration` appears in the list, check the box to it's left
    7. Select `Next`
    8. Follow the dialogs to complete the installation
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
6. Select `Next`
7. If prompted for your `secure storage password` enter it and select `Ok`
8. Pick a name for your application.  The name needs to be unique across all of the apps on Heroku.  It can only use alpha-numeric characters and dashes.  Enter that name in the `Application Name` field.
9. Select `Next`
10. Select `Web app with Spring and Tomcat`
11. Select `Finish`

You now have a simple Spring MVC application in Eclipse which has also been deployed on Heroku.  View your application on Heroku in your web browser by navigating to `http://yourappname.herokuapp.com` (replacing `yourappname` with the name you selected for your application).

The default page of the application is the instructions for pulling the application into your local development environment.  You've already done that using the Heroku Eclipse plugin, so you won't need to follow those.  To see the simple CRUD application in action select the `people page` link where you should see:
*** TODO: SCREENSHOT ***

Add a new `Person` to the database to verify the application is working correctly.

Back in Eclipse you can view the details about the application on Heroku:

1. Select `Window` from the menu bar
2. Select `Show View`
3. Expand `Heroku` in the list
4. Select `My Heroku Applications`
5. Select `Ok`
6. Double-click on your application in the list to view it's details

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

To view the logs for your application on Heroku, start by opening the `My Heroku Applications` view in Eclipse:

1. Select `Window` from the menu bar
2. Select `Show View`
3. Expand `Heroku` in the list
4. Select `My Heroku Applications`
5. Select `Ok`
6. Locate your application in the list
7. Select the context menu for the application (right-click on the application)
8. Select `View Logs`

You will now see the logs for your application on Heroku in the `Heroku log console` view.  If you make a request to the the application in your browser then you will now see the request logged in the `Heroku log console` view.

You can see the status for the web process from the `Procfile` by expanding the application in the `My Heroku Applications` view.

Double-click on the application in the `My Heroku Applications` view to see the details about the application.

By default the application runs on one Dyno.  You can allocate (i.e. scale) as many Dynos as you want to the application's processes.  To scale the web process to five dynos:

1. *** TODO: Waiting on Eclipse Plugin Feature ***

Collaborators can be added to an application in order to allow other developers access to push changes and manage the application.  To add a collaborator to the application:

1. Navigate to the Heroku application details
2. Select the `Collaborators` tab
3. Select `+`
4. In the `Email` field enter the email address for the collaborator
5. Select `OK`

You can transfer ownership of the application to someone else by adding them as a collaborator and then selecting `Make Owner` from the `Collaborators` tab.

Heroku uses environment variables to handle configuration values that vary between environments.  To add a new environment variable to the application:

1. Navigate to the Heroku application details
2. Select the `Environment Variables` tab
3. *** TODO: Waiting on Eclipse Plugin Feature ***

Heroku provides a large ecosystem of cloud services that can be instantly provisioned for your application.  To see the list of Heroku Add-ons navigate to [http://addons.heroku.com](http://addons.heroku.com) in your browser.  If you are logged into heroku.com you can add add-ons to your application from your web browser.  You will do this shortly.

If needed you can destroy your applications on Heroku by selecting `Destroy` from the context menu for an application in the `My Heroku Applications` view.  This will destroy the application, the Git repository, all add-ons, and all data for an application.


Chapter 2: Building a RESTful API
---------------------------------

The application you started with is a pretty typical Java web application that uses JSPs for generating web pages on the server.  It is becoming much more common for web applications to expose serialized data in a RESTful architecture for web and mobile consumption.  Now you will add a service to the application that will return the list of people as serialized data using the JavaScript Object Notation (JSON).

Start by adding the jQuery and Jackson JSON libraries to your project dependencies:

1. Add the `webjars` repository within the `project` section of your `pom.xml` file:

        <repositories>
            <repository>
                <id>webjars</id>
                <url>http://webjars.github.com/m2</url>
            </repository>
        </repositories>

2. Add the dependencies to the `dependencies` section of your `pom.xml` file:

        <dependency>
            <groupId>org.codehaus.jackson</groupId>
            <artifactId>jackson-mapper-asl</artifactId>
            <version>1.9.2</version>
        </dependency>
        <dependency>
            <groupId>com.jquery</groupId>
            <artifactId>jquery</artifactId>
            <version>1.7.2-1</version>
        </dependency>

Now add a new method to the `src/main/java/com/example/controller/PersonController.java` file:

1. Add the import statements below the `package` line:

        import java.util.List;
        import org.springframework.http.MediaType;
        import org.springframework.web.bind.annotation.ResponseBody;

2. Add a new method within the `public class` block:

        @RequestMapping(value = "/people.json", method = RequestMethod.GET, produces = MediaType.APPLICATION_JSON_VALUE)
        public @ResponseBody List<Person> listPeople() {
            return personService.listPeople();
        }

3. Terminate the running process by opening the `Console` view and selecting the stop / terminate button
4. Restart the `webapp-runner` process by selecting `Run` from the menu bar then select `Run`

Verify that the JSON service works by opening [http://localhost:8080/people/](http://localhost:8080/people/) in your browser and adding a couple new people to the database.  Then open [http://localhost:8080/people/people.json](http://localhost:8080/people/people.json) in your browser and you should see a JSON encoded list of the people in your database.

Now you will add a HTML file that will load and render the people through JavaScript / jQuery:

1. Edit the `src/main/resources/applicationContext.xml` file and add the following to the `beans` section:

        <mvc:resources location="classpath:public/" mapping="/assets/**"/>

    This sets up a mapping that will allow you to load jQuery from the webjar.
2. Create a new file named `src/main/webapp/people.html` that contains:

        <!DOCTYPE html>
        <html>
        <head>
        <script src="/people/assets/jquery.min.js"></script>
        <script>
            $(function() {
                $.getJSON("people/people.json", function(people) {
                    $.each(people, function(index, person) {
                        $("#people").append($("<li>").text(person.firstName + " " + person.lastName))
                    })
                })
            })
        </script>
        </head>
        <body>
            <ul id="people"></ul>
        </body>
        </html>

3. Restart the server again (terminate and run like before)
4. Test the new web page by loading [http://localhost:8080/people.html](http://localhost:8080/people.html) in your browser

Now deploy your changes on Heroku (like before) by committing the changes to your local Git repository and then pushing the changes to Heroku.  Then verify that the new `people.html` page works on Heroku.


Chapter 3: Integrating with Force.com
-------------------------------------

Now that you've learned the basics of deploying Java apps on Heroku you will deploy a Java application that integrates with Force.com through RESTful APIs.

Start by creating a new project from the `Spring and Force.com` *** TODO: Need final name *** template:

1. From the Eclipse menu bar select File
2. Select New
3. Select Project
4. Expand the `Heroku` section
5. Select `Create Heroku App from Template`
6. Select `Next`
7. If prompted for your `secure storage password` enter it and select `Ok`
8. Pick a name for your application.  The name needs to be unique across all of the apps on Heroku.  It can only use alpha-numeric characters and dashes.  Enter that name in the `Application Name` field.
9. Select `Next`
10. Select `Spring and Force.com` *** TODO: Need final name *** *** TODO: Need this template app in list ***
11. Select `Finish`

This template application uses OAUTH to authenticate a user with Force.com.  To setup OAUTH you will need to configure a remote access application on Force.com.

1. Login to [Salesforce.com](http://salesforce.com)
2. Select your name in the top right and select `Setup`
3. On the left, expand `Develop` and select `Remote Access`
4. Select `New` to create a new Remote Access Application
5. In the `Application` field enter `local-spring`
6. In the `Contact Email` field enter your email address
7. Enter `http://localhost:8080/_auth` in the `Callback URL` field
8. Select `Save`
9. Leave the `Remote Access Detail` page open because shortly you will need some information from it

Now that OAuth is configured on Salesforce.com we can run this application locally to test it.

1. In Eclipse select the `Run` menu
2. Select `Run Configurations...`
3. Select `Java Application`
4. Select the `New launch configuration` icon (above the filter field)
5. In the `Name` field enter `webapp-runner for x` (replacing x with your project name)
6. In the `Main class` field enter `webapp.runner.launch.Main`
7. Select the `Browse...` button next to the `Project` field
8. Select your project from the list
9. Select the `Arguments` tab
10. In the `Program arguments` field enter `src/main/webapp`  *** TODO: VERIFY ON WINDOWS ***
11. Select the `Environment` tab
12. Select `New`
13. In the `Name` field enter `SFDC_OAUTH_CLIENT_ID`
14. Retrieve the `Consumer Key` value from the `Remote Access Detail` page on Salesforce.com then copy and paste it into the `Value` field
15. Select `Ok`
16. Select `New`
17. In the `Name` field enter `SFDC_OAUTH_CLIENT_SECRET`
18. Retrieve the `Consumer Secret` value from the `Remote Access Detail` page then copy and paste it into the `Value` field
19. Select `Ok`
20. Select `Run` to start the application

Now that the application is up and running you can test it in your browser by visiting:  
[http://localhost:8080/](http://localhost:8080/)

The index page is unprotected and should load without having to authenticate.  Now load the "Contacts" page by visiting:  
[http://localhost:8080/sfdc/contacts](http://localhost:8080/sfdc/contacts)

You should now be redirected to Salesforce.com's OAuth handshake page.  Select `Allow` to do the OAuth handshake.  You will then be redirected back to the "Contacts" page which should now display a list of your Salesforce.com contacts.  (Note: Developer Edition accounts have a few contacts out-of-the-box.)  Test that creating and deleting contacts also works.

The application is already running on Heroku but in order for it to work properly you need to configure the `SFDC_OAUTH_CLIENT_ID` and `SFDC_OAUTH_CLIENT_SECRET` environment variables.  You will need to setup a new `Remote Access Application` on Salesforce.com that has the `Callback URL` for the application on Heroku.

1. Login to [Salesforce.com](http://salesforce.com)
2. Select your name in the top right and select `Setup`
3. On the left, expand `Develop` and select `Remote Access`
4. Select `New` to create a new Remote Access Application
5. In the `Application` field enter `local-spring`
6. In the `Contact Email` field enter your email address
7. Enter `https://yourappname.herokuapp.com/_auth` in the `Callback URL` field (make sure you specify `https` as the protocol)
8. Select `Save`
9. Leave the `Remote Access Detail` page open because shortly you will need some information from it

You now have a new `Consumer Key` and `Consumer Secret` that will be used for your app on Heroku.  Those values will need to be set as environment variables on your Heroku application.

1. Locate the application in the `My Heroku Applications` view and double-click on the application
2. Select the `Environment Variables` tab
3. *** TODO: Waiting on Eclipse plugin support ***

To test the application on Heroku navigate to `https://yourappname.herokuapp.com` in your browser (replace `yourappname` with your app name and insure you use `https` for the protocol).  Opening the "Contacts" page should trigger the OAUTH handshake and then display your contacts.

*** TODO: Screenshot ***

Now that the application is working lets make a simple modification to it.  Lets add a Twitter handle field to the `Contact` object on Salesforce.com:

1. Login to [Salesforce.com](http://salesforce.com)
2. Select your name in the top right and select `Setup`
3. On the left, expand `Customize` then expand `Contacts`
4. Select `Fields`
5. Next to `Contact Custom Fields & Relationships` select `New`
6. Select `Text` as the data type
7. Select `Next`
8. In the `Field Label` field enter `TwitterHandle`
9. In the `Length` field enter `64`
10. Select `Next`
11. Select `Next`
12. Select `Save`

Now that the field has been added, test it by adding a Twitter handle to a Contact:

1. Select the `Contacts` tab
2. Next to the `All Contacts` drop-down select `Go`
3. Pick a contact and select `Edit`
4. Locate the `TwitterHandle` field and enter a fictitious Twitter handle
5. Select `Save`

You will now need to modify your Java application to display the new Twitter handle for the contacts.

1. Open the `src/main/java/com/example/controller/ContactsController.java` file and locate the line:

        map.put("contactList", service.query("select Id,FirstName,LastName,Email FROM Contact"));

2. Update the SOQL query to include the new `TwitterHandle` field:

        map.put("contactList", service.query("select Id,FirstName,LastName,Email,TwitterHandle__c FROM Contact"));

    The `__c` indicates that the field is a custom field.
3. Open the `src/main/webapp/WEB-INF/jsp/contacts.jsp` file
4. Add a new line beneath:

        <th>Email</th>

    Containing:

        <th>Twitter</th>

5. Add a new line beneath:

        <td>${contact.getField("email").value}</td>

    Containing:

        <td><a href="http://twitter.com/${contact.getField("twitterhandle__c").value}">${contact.getField("twitterhandle__c").value}</a></td>

Test your change locally by restarting the local server and opening [http://localhost:8080/sfdc/contacts](http://localhost:8080/sfdc/contacts) in your browser.  You should see a new column containing the Twitter handle.

You can now deploy your changes on Heroku.  First commit the changes to your local Git repository:

1. Select the project's context menu (right-click on the project in the `Project Explorer` panel
2. Select `Team`
3. Select `Commit...`
4. Enter a `Commit message` like `Added Twitter Handle`
5. Select `Commit`

Now push your changes to Heroku

1. Select the project's context menu
2. Select `Team`
3. Select `Push to Upstream`

Test the new version of the application on Heroku by navigating to `https://yourappname.herokuapp.com` in your browser (replace `yourappname` with your app name and insure you use `https` for the protocol).  Open the "Contacts" page and you should now see the new `Twitter` column.


Chapter 4: Distributed Sessions on Heroku
-----------------------------------------

Heroku's Dynos are meant to be used in a stateless fashion for instant scalability and updates.  Some Java applications use session state to manage context across requests.  Spring Security uses session state to identify a user across requests.  To handle the use of session state you need to externalize the state.  Since this application uses `webapp-runner` this is very easy to by adding the Memcache Heroku Add-on and configuring `webapp-runner` to use it.

1. In your browser navigate to [https://api.heroku.com/login](https://api.heroku.com/login) and login
2. Navigate to [https://addons.heroku.com/memcache](https://addons.heroku.com/memcache)
3. Select `Add` for the free 5MB plan
4. Select the application from the drop-down
5. Select `Select`
6. Open the `Procfile` and change line 1 to be:

        web:    java $JAVA_OPTS -jar target/dependency/webapp-runner.jar --port $PORT --session_manager memcache target/*.war

    This enables `webapp-runner` to use the provisioned Memcache service for externalized session storage

Now commit the changes to the local Git repository and push them to Heroku:

1. Select the project's context menu (right-click on the project in the `Project Explorer` panel
2. Select `Team`
3. Select `Commit...`
4. Enter a `Commit message` like `Switched on Memcache session management`
5. Select `Commit`
6. Select the project's context menu
7. Select `Team`
8. Select `Push to Upstream`

Verify that your application still works on Heroku.


Chapter 5: Real time Push notifications
---------------------------------------

With Heroku it's easy to at real-time push notifications to an application.  You will now add a notification that triggers every time a Contact is updated through your application on Heroku.  You will use the PubNub Heroku add-on to do the real-time push.

First add a Maven repository to the `project` section of your `pom.xml` file for the PubNub library:

    <repositories>
        <repository>
            <id>pubnub-repo</id>
            <url>http://pubnub-repo.herokuapp.com/</url>
        </repository>
    </repositories>

Also in the `pom.xml` add the pubnub library as a dependency in the `dependencies` section:

    <dependency>
        <groupId>pubnub</groupId>
        <artifactId>pubnub</artifactId>
        <version>3.1</version>
    </dependency>

Create a new file named `src/main/java/com/example/services/PubnubService.java` containing:

    package com.example.services;
    
    import org.json.JSONException;
    import org.json.JSONObject;
    
    import pubnub.Pubnub;
    
    public class PubnubService {
        public static void pushUpdate(String id) {
            try {
                String publishKey = System.getenv("PUBNUB_PUBLISH_KEY");
                String subscribeKey = System.getenv("PUBNUB_SUBSCRIBE_KEY");
                String secretKey = System.getenv("PUBNUB_SECRET_KEY");
        
                JSONObject jsonObject = new JSONObject();
                jsonObject.put("id", id);
                jsonObject.put("action", "updated");
        
                Pubnub pubnub = new Pubnub(publishKey, subscribeKey, secretKey);
                pubnub.publish(id, jsonObject);
            }
            catch (JSONException e) {
            
            }
        }
    }

In the `src/main/java/com/example/controller/ContactsController.java` file add these import statements:

    import com.example.services.PubnubService;
    import org.springframework.http.MediaType;
    import org.springframework.web.bind.annotation.ResponseBody;

Also in the `ContactsController` class add a new method to retrieve a `Contact` as JSON:

    @RequestMapping(value = "/{id}/json", produces = MediaType.APPLICATION_JSON_VALUE)
    public @ResponseBody Map<String, Object> getContactDetail(@PathVariable("id") String id) {
        return service.fetch("Contact", id).getRaw();
    }

Then in the `editContact` method, below the line containing:

    service.update(record);

Add the following:

    PubnubService.pushUpdate(id);

In the `src/main/webapp/WEB-INF/jsp/footer.jsp` file, below the line containing:

    <script src="/resources/js/jquery-1.7.1.min.js"></script>

Add the following:

    <script src="/resources/js/bootstrap.min.js"></script>

In the `src/main/webapp/WEB-INF/jsp/contacts.jsp` file, below the line containing:

    <jsp:include page="header.jsp"/>

Add the following:

    <div pub-key="<%=System.getenv("PUBNUB_PUBLISH_KEY")%>" sub-key="<%=System.getenv("PUBNUB_SUBSCRIBE_KEY")%>" ssl="off" origin="pubsub.pubnub.com" id="pubnub"></div>
    <script src="https://pubnub.s3.amazonaws.com/pubnub-3.1.min.js"></script>

    <script>
    function updateContact(message) {
        $.get("contacts/" + message.id + "/json", function(contact) {
            var alert = $("<div>").addClass("alert alert-info");
            alert.append($("<a>").addClass("close").attr("data-dismiss", "alert").html("&times;"));
            alert.append($("<h4>").addClass("alert-heading").text("Contact " + contact.FIRSTNAME + " " + contact.LASTNAME + " Updated!"));
            $(".span8").prepend(alert);
        })
    }
    </script>

And below the line containing:

    <c:forEach items="${contactList}" var="contact">

Add the following:

    <script>
    PUBNUB.subscribe({
        channel: "${contact.getField("id").value}",
        callback: updateContact
    });
    </script>

Now your application will display a message on the "Contacts" page each time someone updates a `Contact` through the application.  That message will be pushed out in real-time to all of the users of the application.

To configure PubNub you will need to add the PubNub Heroku Add-on then obtain and set the required environment variables:

1. Navigate in your browser to [https://addons.heroku.com/pubnub](https://addons.heroku.com/pubnub)
2. Select `Add` for the free "Minimal" level of service
3. Select your application from the drop-down
4. Select the `Select` button
5. When the PubNub service has been installed, select PubNub from the list of resources in your application

The `PUBLISH KEY`, `SUBSCRIBE KEY`, and `SECRET KEY` values will need to be added to your local runtime configuration and to your environment variables on Heroku.

First add these to your local run configuration so you can test locally:

1. If you have a webapp-runner actively running then terminate it in Eclipse
2. In Eclipse select the `Run` menu
3. Select `Run Configurations...`
4. Select the configuration on the left named `webapp-runner for x` (replacing x with your project name)
5. Select the `Environment` tab
6. Select `New`
7. In the `Name` field enter `PUBNUB_PUBLISH_KEY`
8. Retrieve the `PUBLISH KEY` value from the PubNub Heroku Add-on page then copy and paste it into the `Value` field
9. Select `Ok`
10. Select `New`
11. In the `Name` field enter `PUBNUB_SUBSCRIBE_KEY`
12. Retrieve the `SUBSCRIBE KEY` value from the PubNub Heroku Add-on page then copy and paste it into the `Value` field
13. Select `Ok`
14. Select `New`
15. In the `Name` field enter `PUBNUB_SECRET_KEY`
16. Retrieve the `SECRET KEY` value from the PubNub Heroku Add-on page then copy and paste it into the `Value` field
17. Select `Ok`
18. Select `Run` to start the application

To test the real-time push you will need two browser windows.  Open both to the [http://localhost:8080/sfdc/contacts](http://localhost:8080/sfdc/contacts) page.  Then in one browser select a contact to edit (by selecting the contact name).  Then select edit, make a change, and select `Save`.  In your other browser you should see a notification that the Contact was updated.

Before you deploy these changes on Heroku set the environment variables for PubNub:

1. Locate the application in the `My Heroku Applications` view and double-click on the application
2. Select the `Environment Variables` tab
3. *** TODO: Waiting on Eclipse Plugin env var support ***

Now deploy your changes:

1. Select the project's context menu (right-click on the project in the `Project Explorer` panel
2. Select `Team`
3. Select `Commit...`
4. Enter a `Commit message` like `Added real-time push with PubNub`
5. Select `Commit`
6. Select the project's context menu
7. Select `Team`
8. Select `Push to Upstream`

Verify that the real-time push notifications work in your application on Heroku.


Chapter 6: Performance Monitoring with New Relic
------------------------------------------------

New Relic provides an in-depth view into the performance and memory usage of applications on Heroku.

Start by adding the New Relic Add-on:

1. Navigate in your browser to [https://addons.heroku.com/newrelic](https://addons.heroku.com/newrelic)
2. Select `Add` for the free "Standard" level of service
3. Select your application from the drop-down
4. Select the `Select` button

Now your application needs to be configured to use the New Relic Java agent:

1. Download the newest New Relic Java Agent zip file from [http://download.newrelic.com/newrelic/java-agent/newrelic-api/](http://download.newrelic.com/newrelic/java-agent/newrelic-api/)
2. Unzip the archive
3. Copy the `newrelic.jar` and `newrelic.yml` files into the root directory of your project

Commit these new files to your Git repository and push the new version to Heroku:

1. Select the project's context menu (right-click on the project in the `Project Explorer` panel
2. Select `Team`
3. Select `Commit...`
4. Enter a `Commit message` like `Added New Relic files`
5. In the `Files` list check the boxes next to the `newrelic.jar` and `newrelic.yml` files
5. Select `Commit`
6. Select the project's context menu
7. Select `Team`
8. Select `Push to Upstream`

You need to change the default `JAVA_OPTS` environment variable for your application to the Java process to use the New Relic Java agent.

1. Navigate to the Heroku application details
2. Select the `Environment Variables` tab
3. *** TODO: Waiting on Eclipse Plugin Feature ***
4. `JAVA_OPTS`
5. `-Xmx384m -Xss512k -XX:+UseCompressedOops -javaagent:newrelic.jar`

Test the application on Heroku in your browser and verify that it works as expected.

You can now browse the New Relic Dashboard:

1. In your browser navigate to [https://api.heroku.com/myapps](https://api.heroku.com/myapps)
2. Select the application you added New Relic to
3. Select the `Add-ons` drop-down
4. Select `New Relic`

Explore the New Relic dashboard.


Chapter 7: Searching Logs with Papertrail
-----------------------------------------

The Papertrail Heroku Add-on provides a powerful way to search your application logs in your browser and setup alerts for specific search criteria.

1. Navigate in your browser to [https://addons.heroku.com/papertrail](https://addons.heroku.com/papertrail)
2. Select `Add` for the free "Test" level of service
3. Select your application from the drop-down
4. Select the `Select` button
5. Open the `Add-ons` drop-down for your application
6. Select `Papertrail`

After a minute or so Papertrail will have your latest logs.

Enter a search term like `WARN` in the search box at the bottom of the screen.  You will then see all of the log entries for your application which match that term.

You can setup Papertrail to notify you via email when new log entries matching your search are received:

1. Select `Save Search`
2. Enter a name for the saved search
3. Select `Save & Setup Alert`
4. Select `Emails`
5. In the `Recipients` field enter your email address
6. Choose a `Frequency`
7. Select `Update`

The next time your application has a log event that matches your search, you will receive an email notification.
