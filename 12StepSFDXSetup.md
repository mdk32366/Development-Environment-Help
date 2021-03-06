#### Salesforce DX Setup Instructions

[Original Link](https://www.demandblue.com/salesforce-dx-setup/)

Your 12 Step Salesforce DX Setup Guide
1. Set up your project
Salesforce DX introduces a new project structure for your org’s metadata (code and configuration), your org templates, your sample data, and all your team’s tests. Store these items in a version control system (VCS) to bring consistency to your team’s development processes. Retrieve the contents of your team’s repository when you’re ready to develop a new feature.

2. Salesforce DX Setup – Authorize the Developer Hub org for the project
During Salesforce DX setup, the Dev Hub org enables you to create, delete, and manage your Salesforce scratch orgs. After you set up your project on your local machine, you authorize with the Dev Hub org before you create a scratch org.

For this, you need to login to Dev/Sandbox Org from CLI
Run the force:auth:web:login CLI command on a directory where code for deploy to sfdx will be available.

sfdx force:auth:web:login –d

(or)

sfdx force:auth:web:login –setdefaultdevhubusername –setalias {ALIAS HERE}

NOTE: Login must be a valid login to your Dev/Sandbox Org and with Admin permissions.

3. Configure your local project
The project configuration file sfdx-project.json indicates that the directory is a Salesforce DX setup project. The configuration file contains project information and facilitates the authentication of scratch orgs and the creation of second-generation packages. It also tells the Salesforce CLI where to put files when syncing between the project and scratch org.

4. Salesforce DX Setup – Create a scratch org
After you create the scratch org definition file, you can easily spin up a scratch org and open it directly from the command line.

a) Create the scratch org

Create a scratch org for development using a scratch org definition file

The scratch org definition defines the org edition, features, org preferences, and some other options.
Specify scratch org definition values on the command line using key=value pairs
Create a scratch org with an alias
Create a scratch org for user acceptance testing or to test installations of packages
Indicate that this scratch org is the default
Specify the scratch org’s duration, which indicates when the scratch org expires (in days)
b) Open the Org

To open the scratch org – sfdx force:org:open -u <username/alias>
To open the scratch org in Lightning Experience or open a Visualforce page, use the –path parameter – sfdx force:org:open –path lightning
c) Set default user

Copy the username and enter the following command to set the defaultusername:

sfdx force:config:set defaultusername={SET THIS TO NEW SCRATCH ORG’S USERNAME FROM THE ABOVE  COMMAND}

d) Display All Orgs

Run the following command to confirm the default Dev Hub [marked with (D)] and Active Scratch Org [marked with (U)]:

sfdx force:org:list –all

5.  Push the source from your project to the scratch org
a) To push changed source to your default scratch org:
sfdx force:source:push

b) To push changed source to a scratch org that’s not the default, you can indicate it by its username or alias
sfdx force:source:push –targetusername test-b4ag3oxmu@example.com

sfdx force:source:push -u test-b4ag3oxmu@example.com

sfdx force:source:push -u MyGroovyScratchOrg

c) Selecting Files to Ignore During Push

It’s likely that you have some files that you don’t want to sync between the project and scratch org. You can have the push command ignore the files you indicate in .forceignore

d) If Push Detects Warnings

If you run force:source:push, and warnings occur, the CLI doesn’t push the source. Warnings can occur, for example, if your project source is using an outdated version. If you want to ignore these warnings and push the source to the scratch org, run:

sfdx force:source:push –ignorewarnings

e) If Push Detects File Conflicts

If conflicts have been detected and you want to override them, here’s how you use the power of the force (overwrite) to push the source to a scratch org.

sfdx force:source:push –forceoverwrite

6. Salesforce DX Setup – Develop the app
a. Create Source Files from the CLI

To add source files from the Salesforce CLI, make sure that you are working in an appropriate directory.

Execute one of these commands.

apex:class:create
apex:trigger:create
lightning:app:create
lightning:component:create
lightning:event:create
lightning:interface:create
lightning:test:create
visualforce:component:create
visualforce:page:create
b. Edit Source Files

To edit a FlexiPage in your default browser—for example, to edit the Property_Record_Page source—execute this command.

sfdx force:source:open -f Property_Record_Page.flexipage-meta.xml

7. Pull the source to keep your project and scratch org in sync
After you do an initial push, Salesforce DX tracks the changes between your local file system and your scratch org. If you change your scratch org, you usually want to pull those changes to your local project to keep both in sync.

During development, you change files locally in your file system and change the scratch org using the builders and editors that Salesforce supplies. Usually, these changes don’t cause a conflict and involve unique files.

By default, only changed source is synced back to your project.

To pull changed source from the scratch org to the project:

sfdx force:source:pull

To pull source to the project if a conflict has been detected:

sfdx force:source:pull –forceoverwrite

https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_pull_md_from_scratch_org.htm

8. Salesforce DX Setup – Run tests
When you’re ready to test changes to your Salesforce app source code, you can run Apex tests from the Salesforce DX CLI. Apex tests are run in your scratch org.

You can also execute the CLI command for running Apex tests (force:apex:test:run) from within third-party continuous integration tools, such as Jenkins.

9. Export The Package.xml

Export package.xml file into the temporary directory. Type the commands below in the root folder of your Salesforce DX project:

sfdx force:mdapi:retrieve -r ./temp -u {TARGETUSERNAME} -k  {SFDC PROJECT SOURCE LOCATION}\src\package.xml

10. Convert Source code to Salesforce  DX
Convert the source code to the Salesforce Developer Experience project structure by running the following command:

                sfdx force:mdapi:convert –rootdir temp –outputdir force-app

11. Track Changes Between the Project and Scratch Org
To view the status of local or remote files:

sfdx force:source:status

12. Salesforce DX Setup – Sync up
Sync the local version with the version deployed to Scratch Org for every change and test the changes on the Scratch Org by repeating the above steps. Once the testing is completed, we need to convert the source from Salesforce DX format to the Metadata API format. This is done by running the following command:

sfdx force:source:convert –outputdir {OUTPUT DIRECTORY HERE}
