#  JSSP Static Broker

This document will describe how to 
- install
- execute
- build the JSSP Static broker

# 1. Register and configure a service instance and smartobjects in K2

If you alread have the "JAVASCRIPT CONNECTOR - CREATE OR UPDATE" form on your K2 environment you can skip step 1.1.

# 1.1 Install JSSP pacakge

Follow the steps in the "Deploying and Registering a JSSP Service Type" document:  
https://help.k2.com/onlinehelp/Platform/UserGuide/June/default.htm#../Subsystems/Default/Content/Extend/JS-Broker/JSSPRegister.htm

# 1.2 Install the JSSP broker

1. Save the ShareDoStatic.jssp file to your desktop.
2. Open the JSSP form: https://xxx.onk2stable.com/Runtime/Runtime/Form/JavaScript+Service+Type+Management+Form/ where xxx.onk2stable.com is the host of your K2 environment
3. Type in a SYSTEM NAME, DISPLAY NAME (DESCRIPTION is optional).
4. Click on the JSSP CONNECTOR FILE control and choose the downloaded jssp file from point 1.
5. Wait for the file to finish uploading. Can be seen on the file upload control with the file size showing a 12 KB/ 25 KB progeress. When it is done, it will just show the total size (ex. 25 KB)
6. Click on the "Create or Update" button

# 1.3 Setup OAuth authentication to ShareDo

If you have done this and already have a OAuth resource setup, you can skip this step. If not, then please follow the steps in the OAuth Setup document

# 1.4 Register a Service Instance and generate SmartObjects

1. Open your K2 Management Site
2. Go to the Service Instances page under Integration.
3. Click the +Add button
4. Type in a DisplayName and Description
5. In the Service Type dropdown, choose the one you installed in step 1.2
6. Choose OAuth from the Authentication Mode dropdown.
7. Select the OAuth Resource you created in step 1.3 from the OAuth Resource Name dropdown
8. Setup the Service Keys
   1. Optionally you can specify an Azure Storeage Connection String to a queue for logging/debugging purposes.
   2. Provide the url to your ShareDo instance api in the ServiceURL setting (ex https://xxx.sharedo.co.uk/api/).
9. Tick the "Generate SmartObjects for this Service Instance" tickbox to also create the Smartobjects.

# 2. Ussage of SmartObjects

# 2.1 WorkTypes

This SmartObject is used to get a list of the WorkTypes. The systemname is specifically needed when createing a new worktype.

The List Method provides the following outputs:
1. SystemName - System name of the worktype in ShareDo
2. Name - Name of the worktype in ShareDo
3. Description - Description of the worktype in ShareDo
4. Icon - Icon of the worktype in ShareDo
5. Active - If the worktype is active or disabled.
6. Abstract - If the worktype is marked as abstract.
7. CoreType - If the worktype is a CoreType
8. SystemName Path - Concatednated path of the worktype
9. Parent SystemName - The direct parent of the worktype
10. Has DerivedTypes - If the worktype has derivedtypes (children)

Filters can be applied to the smartobject method to filter the list.  
The List method does not show the Task worktypes. See the Tasks smartobject that will release later.

# 3. Build

To build you need to setup a development environment.  
Here are the steps:
1. Install the followng on your environment:
   1. NodeJS - https://nodejs.org/en/download/
   2. VisualStudio Code - https://code.visualstudio.com/
2. Download/Clone the JSSP Static Broker source to your local drive.
3. Open the folder in VisualStudio Code
4. The Src folder contains the ts source files
5. In the terminal, run "npm install". This will install all the needed libraries.
6. In the terminal, run "npm run build". This will build the source fles to js files.
7. The dist folder will contain the js files that rollup has built

You can also follow this document:
https://help.k2.com/onlinehelp/Platform/UserGuide/June/default.htm#../Subsystems/Default/Content/Extend/JS-Broker/JSSPEnvironment.htm


