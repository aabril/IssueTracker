CSC 438 Issue Tracker Project Documentation
 
Project Contributors:
Adrian Petras
Derek Simmons
Bryan Barnard

Application Hosting: http://nardbard.pythonanywhere.com/newtracker (provided by pythonanywhere)
Source Hosting: http://code.google.com/p/csc438-issue-tracker-web2py/ (provided by Google Code)
 
Controllers
- controllers/services.py
-- Functions
--- ping: simple response to send hostname and text in response to ping request, indicating the connection was made and it is responding
--- call: endpoint that routes xmlrpc calls to service functions (ping and new issue)
--- newissue: receives input for issue specific variables, including project id, summary, description, and owner and inserts a new record into the issue tracker db.

- controllers/default.py
-- Functions
--- addtition of 'assign' function: controller action that allows user to assign issues to other members of project team
--- addition of 'dependencies' function: controller action that allows user to view an issues dependencies and add new dependencies

Views
- views/default/assign.html - view to handle assign controller action
- views/default/dependencies.html - view to handle dependencies controller action

Models
- models/db_tracker.py - defines tables for issue tracker app


Plugin_IssueTracker Installation and use instructions:

"The Web2py Issue Tracker Plugin allows you to add functionality to any web2py app to be able to submit issues to a linked issue tracker, using xmlrpc services and ajax"

- Basic Usage

1. To use the issue_tracker plugin you must have an account on an installation of the web2py issue tracker app. Can be downloaded from google code at: http://code.google.com/p/csc438-issue-tracker-web2py/, this is a clone of http://code.google.com/p/web2py-issuetracker/.
2. Create a project in the issue tracker and note the project id.
3. Download and install the issue_tracker plugin from the google code downloads site (http://code.google.com/p/csc438-issue-tracker-web2py/downloads/detail?name=web2py.plugin.issuetracker.w2p&can=2&q=#makechanges)
4. View and update the plugin config file under models/plugin_issuetracker.py
5. Update the 'models/plugin_issuetradker.py' editing the:
     - 'plugin_issuetracker_host' variable to point to your instance of issue tracker
     - 'plugin_issue_tracker_projectid' variable to reference the project id of your issue tracking project
     - 'plugin_issuetracker_pwd' if you are using basic authentication this will be the password of your user on the tracker project.
     - 'plugin_issuetracker_user' if you are using basic authentication this will be your username on the remote issue tracker project.

6. Navigate to the plugin_issuetracker controller in your site, the index function should display the variables you have set in your config file
7. Navigate to the plugin_issuetracker controller/ping function, this will send a request to the remote host and return a success message if connectivity was established
8. If step 7 was successful let's try to submit a new issue. Navigate to the plugin_issuetracker controller/postnewissue function and populate the form with summary,description and owner values and submit. You should be notified via a flash that you have successfully submitted a new issue.
9. A sample function using ajax is included to show you how this functionality could be implemented on any page in your site using an ajax call. Navigate to plugin_issuetracker controller/postajax. Here you can populate the form fields submit. The difference is here you can dynamically set the project and the submit makes an ajax call to the postajax controller method. View the html to see how this is handled with query and web2py helpers. This could be embedded in any view/control in your app to make the call via ajax.


- Advanced Usage (experimental)
     
1. The web2py admin console provides a simple and easy way to view tickets that have been created by yours applications. With a few slight modifications and utilizing the plugin issue tracker you can setup the admin error controller to be able to create issues in a issue tracker.
2. This will require minor modifications to the default controller in the admin application, the errors.html view, and adding an additional view to your application. Reference the files included in the plugin_issuetracker project source under admin_modifications (http://code.google.com/p/csc438-issue-tracker-web2py/source/browse/#hg%2Fadmin_modifications) for samples, or simply copy and paste them over your corresponding admin files.
3. You all also need to modify the models/issuetracker.py file to set the variables for your issue tracker instance host.
4. Once these modifications have been made browsing to you errors section of your app next to the 'details' link listed for each ticket you should see a 'submit issue' link. Clicking this link will take you to a partially populated form that, summary containing the ticket name and description containing the simple description (output) of your ticket. Simple add the additional fields and submit to create a new issue in your issue tracker.


Plugin_IssueTracker Manifest

Views
- views/issue_tracker/postnewissue.html - view to format the postnewissue function of plugin_issuetracker controller.
- views/issue_tracker/index.html - view to handle the display from index function of plugin_issuetracker controller.     
- views/issue_tracker/ping.html - view to handle the display from ping function of plugin_issuetracker controller. 

Controllers
- controllers/plugin_issuetracker.py
-- Functions
--- ping :  connects to the issue tracker service via the variables specified in models/plugin_issuetracker.py, calling the service and returning a simple response if the connection is successful.
--- index:  displays the variables as defined in models/plugin_issuetracker.py for debugging purposes.
--- postnewissue: provides a form via which a user can input variables to be stored for a new issue in the issue tracker, utilizes the variables set in models/plugin_issuetracker.py, to make an xmlrpc call to the issue tracker and submit a new issue.

Models
models/plugin_issuetracker.py - used as a config file to define variables necessary to connect to the remote issue tracker.
- Variables
-- 'plugin_issuetracker_host' - the uri of the the controller hosting the service method for the issue tracker (ie. 'http://127.0.0.1:8000/issue_tracker/services/call/xmlrpc).
-- 'plugin_issuetracker_user' - username of the user account that should be used to connect to the remote host using basic authentication
-- 'plugin_issuetracker_pwd' - password for the user account that will be used to connect to the remote host using basic authentication
-- 'plugin_issuetracker_projectid' - project id of the remote project for which issues should be submitted, this is necessary to associate the issue with a project (i.e.. 1)

Static
- static/plugin_issuetracker/license.html - licensing info for the plugin
- static/plugin_issuetracker/about.html - general info about the use of the plugin
 
Admin Modifications

Files modified specifically to allow the submission of issues from the admin/errors controller

Controllers
- admin_modifications/controllers/default.py  - addition of new function 'newissue' - this allows for the submission of issues from tickets via xmlrpc calls.       

Views
- admin_modifications/views/default/newissue.html - view to handle new function, newissue
- admin_modification/views/default/errors.html - modify to add submit issue to the list of errors listed for any application

Models
- admin_modifications/models/issuetracker.py - settings for issuetracker
