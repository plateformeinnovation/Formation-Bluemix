# Deploy the service in Bluemix with the toolchain

<!-- page_number: true -->
<!-- $size: 16:9 -->
<!-- prerender: true -->
<!-- footer: OPEN GROUPE - Formation Bluemix - JUIN 2017 -->


This project comes with a partially automated toolchain capable of deploying the service to Cloud Foundry, OpenWhisk and Kubernetes. There is some information you need to get before creating the toolchain.

> Although this section uses a toolchain, it assumes you have successfully configured the Bluemix CLI and its plugins. Some steps require you to use the command line.

> Only the US South region is currently supported.

## 1. Obtain a Bluemix API key

A Bluemix API key is used in place of your Bluemix credentials. It allows to access the Bluemix API. The toolchain uses the API key to interact with the Container Service API.

1. Generate a Bluemix API key

   ```
   bx iam api-key-create for-cli
   ```

   > You can also use an existing API key.

1. Make note of the generated API key. It won't be shown again.

## 2. Get the namespace where you will push the Docker image

1. Check the existing namespaces

   ```
   bx cr namespace-list
   ```

   > You can use any of the namespaces listed.

1. If you want to create a new namespace, use

   ```
   bx cr namespace-add fibonacci
   ```

1. Make note of the namespace. You will need it later.

## 3. Build the Docker image

### If you have used IBM Containers in the past, you can have the toolchain build the Docker image for you

The toolchain is able to build the Docker image but only if you have used IBM Containers in the past - the [*single and scalable containers*](https://console.ng.bluemix.net/docs/containers/cs_classic.html#cs_classic) option before Kubernetes was made available. If you did, you should be familiar with the notion of namespace and quota.

To build the Docker image in the toolchain, you will need to specify a Bluemix space that has quota assigned for IBM Containers. You can find which space has quotas for the IBM Containers by looking at your organization:

  ![](./quotas.png)

In the screenshot above, two spaces have Containers quota available. One will need to be selected when creating the toolchain.

### but if this is your first time using IBM Container with Kubernetes, you need to build the Docker image manually

Go through the steps detailed in the [manual instructions to build the Docker image](DEPLOY_MANUALLY.md#build-the-docker-image).

## 4. Create a Kubernetes cluster

Go through the steps detailed in the [manual instructions to create a Kubernetes cluster](DEPLOY_MANUALLY.md#create-a-kubernetes-cluster). Make sure to wait for the cluster to be in a Ready state before continuing to the next step.

## 5. Create the toolchain

1. Ensure your organization has enough quota for one web application using 256MB of memory, one Kubernetes cluster, and one OpenWhisk action.

1. Click ***Create toolchain*** to start the Bluemix DevOps wizard:

   [![Deploy To Bluemix](https://console.ng.bluemix.net/devops/graphics/create_toolchain_button.png)](https://console.ng.bluemix.net/devops/setup/deploy/?repository=https://github.com/IBM-Bluemix/multiple-deployment-options&branch=master)

1. Select the **GitHub** box.

1. Decide whether you want to clone or fork the repository.

1. If you decide to Clone, set a name for your GitHub repository.

1. Select the **Delivery Pipeline** box.

1. Select the region, organization and space where you want to deploy the web application. A random route will be used for the application.

   :warning: Make sure the organization and the space have no space in their names.

   :warning: Only the US South region is currently supported.

1. Select the region, organization and space where quota for IBM Containers have been specified. If you never use the IBM Containers before, you may need to build the Docker image outside of the toolchain as explained earlier in this document.

1. Optionally set the Bluemix API key. If you don't set the key, the Fibonacci service will NOT be deployed to a Kubernetes cluster and you will need to use the manual instructions.

1. Optionally set the name of an existing Kubernetes cluster if you have one, if you do NOT have an existing cluster then the toolChain will create one by default.

1. Replace the *<namespace>* value with the namespace where the Docker image has been or will be pushed.

1. Click **Create**.

1. Select the Delivery Pipeline.

1. Wait for the DEPLOY stage to complete.

1. The services are ready. Review the [Service API](README.md#Service_API) to call the services.


---

## Enjoy Bluemix ! :+1:


---



# Step 2 - Enable Continuous Delivery

Now let's add a source code repository and an automatic build pipeline to our project. The Git repository and issue tracking is hosted by IBM and built on GitLab Community Edition.

1. In your application **Overview** page, search **Continuous Delivery** and click the **Enable** button.

1. A new window opens to configure the Toolchain.

    ![Toolchain](./images/toolchain-gitlab.png)

1. The toolchain gets a default name you can change. In **Configurable Integrations** at the bottom, select **Git Repos and Issue Tracking**.

1. Keep the Default options to **Clone** the starter code for the "Hello World!" application into your GitLab account.

1. The toolchain has been configured successfully. A new Git Repository has been created, as well as a Build Pipeline so that your app gets automatically redeployed after every commit.

1. Open the Git repo and make note of the Git URL.

---

# Step 3 - Checkout the code locally

1. Open a terminal or a command prompt to clone the repository

    ```
    git clone <URL-OF-YOUR-GIT-REPO>
    ```

1. This command creates a directory of your project locally on your disk.

---

# Step 4 - Run the app locally

1. Change to the directory of the checkout

    ```
    cd todo-[your-initials]
    ```

1. Get the node.js dependencies for this project

    ```
    npm install
    ```

1. Start the app

    ```
    npm start
    ```

    Once started, the console output will look as follows:

    ```
    > NodejsStarterApp@0.0.1 start /Users/mace/todo-[your-initials]
    > node app.js

    server starting on http://localhost:[port-number]
    ```

1. Access the app with your web browser

---

# Step 5 - Change a file locally

1. Open **public/index.html**, modify the welcome message at line 19

1. Reload the page in your web browser to confirm the change locally

---

# Step 6 - Push your local change to the cloud

Cloud Foundry relies on the *manifest.yml* file to know what to do when you push the app on Bluemix.
A default manifest.yml file was generated for our app. It looks like:

  ```yml
  applications:
  - path: .
    memory: 256M
    instances: 1
    domain: mybluemix.net
    name: todo-[your-initials]
    host: todo-[your-initials]
    disk_quota: 1024M
  ```
---

It basically defines one application taking its content from the current directory,
being deployed with **256MB**, with **one** instance, under the **mybluemix.net** domain.
The app is named **todo-[your-initials]** and it is using **todo-[your-initials]** as host name.
It has **1024MB** of disk space available.

1. Specifying the buildpack to be used when pushing a Cloud Foundry app is always faster than relying on buildpack detection. Modify the generated Manifest to specify the **buildpack** by adding one line as follows:

    ```yml
    applications:
    - path: .
      memory: 256M
      instances: 1
      domain: mybluemix.net
      buildpack: sdk-for-nodejs
      name: todo-[your-initials]
      host: todo-[your-initials]
      disk_quota: 1024M
    ```

1. Connect to Bluemix by passing the Bluemix endpoint of the URL region where you created your app.

    ```
    bx api https://api.ng.bluemix.net
    ```

1. Login to Bluemix

    ```
    bx login
    ```

1. Push the app to Bluemix

    ```
    bx cf push
    ```

1. When the command completes, access the application running in the cloud to confirm your change was deployed

    ```
    requested state: started
    instances: 1/1
    usage: 256M x 1 instances
    urls: todo-[your-initials].mybluemix.net
    last uploaded: Mon Jan 16 21:20:54 UTC 2017
    stack: cflinuxfs2
    buildpack: SDK for Node.js(TM) (ibm-node.js-4.6.2, buildpack-v3.9-20161128-1327)

         state     since                    cpu    memory          disk          details
    #0   running   2017-01-16 10:22:17 PM   4.6%   22.1M of 256M   65.8M of 1G    
    ```

Changing files locally and pushing them worked but we can do better.
In a previous step we set up a Git repository and a build pipeline was automatically configured.

---

# Step 7 - Commit your changes and see them deployed automatically

1. Open **public/index.html**.

1. Change the page title at line 5.

1. Confirm the change works locally.

1. Commit your changes locally
    ```
    git commit -a -m "updated title"
    ```

    Note: you might be prompted to configure git for the first time:
    ```
    git config --global user.email "you@example.com"
    git config --global user.name "Your Name"
    ```

1. Push your changes
    ```
    git push
    ```

1. Back to the Bluemix console, go to your application **Overview**.

1. Click on the **View Toolchain** button in the Continuous Delivery section.

1. Click the **Delivery Pipeline** that was created automatically in a previous step.

1. Watch how the Delivery pipeline notice your commit and redeploy the application

1. When the command completes, access the application running in the cloud to confirm your change was deployed


---

# Step 8 - Get the Todo App code

In previous steps, we've seen the basic of modifying code and deploying the application.
Now let's focus on our task to build a Todo App. The application has already been developed and is available in this Git repository.

Your first task is to integrate this code in the app you created, replacing the existing app code.

1. Delete all files and folders from your app **except the manifest.yml and .git folder**.

1. Download the complete Todo application from [this archive](./solution/node-todo-master.zip) into a temp directory.

1. Unzip the files in a temp directory. It creates a *node-todo-master* folder.

1. Copy all files and directories from the extract to your app folder.

Note: Make sure the hidden files (.gitignore, .cfignore and .bowerrc) were also copied.

---

# Step 9 - Create and bind a Cloudant service

In order to store the todo, we will need a persistent storage. To do so, we will use a Cloudant NoSQL database, a JSON document oriented store, compatible with CouchDB.

1. Back to the Bluemix console, go to your application **Overview**.

1. Click **Connect New** to add a service to your application

1. Search for **Cloudant** in the catalog

1. Select the free **Lite** plan

1. Give the service a name such as **todo-cloudant-[your-initials]**

1. Click **Create**. Bluemix will provision a Cloudant service and connect it to your application.

1. Select **Restage** when prompted to do so.

    Your application will restart and the service connection information will be made available to your application.

    Note: All the steps above could have been scripted using the three commands below:
    ```
    cf create-service cloudantNoSQLDB Lite todo-cloudant-[your-initials]
    cf bind-service todo-[your-initials] todo-cloudant-[your-initials]
    cf restage todo-[your-initials]
    ```

---

# Step 10 - Connect the Cloudant DB to the application code

When your application runs in Cloud Foundry, all service information bound to the application are available in the **VCAP_SERVICES** variable.

Given a Cloud Foundry app relies on the VCAP_SERVICES environment variable, a straightforward approach is to set this variable in your environment by creating a local env file (JSON or key=value format), to test for this file in your app and to load the values if found.

1. In the Bluemix console, go to your application dashboard.

1. Select **Runtime**, then **Environment Variables**

1. Copy the full content of the **VCAP_SERVICES** into the file vcap-local.json of your project. Make sure to copy the content on line 3 below the services element. It should look as follows:

    ```json
    {
      "services":
      {
        "cloudantNoSQLDB": [
          {
            "credentials": {
                "username": "XXXX",
                "password": "XXXX",
                "host": "XXXXXX-bluemix.cloudant.com",
                "port": 443,
                "url": "https://....-bluemix.cloudant.com"
            },
            "name": "todo-cloudant",
            "label": "cloudantNoSQLDB",
            "plan": "Lite",
            ...
          }
        ]
      }
    }
    ```

---

# Step 11 - Run the Todo App locally

1. Get the dependencies for the Todo App. In your app directory, run:

    ```
    npm install
    ```

1. Run the application

    ```
    npm start
    ```

1. Access the local application

---

# Step 12 - Commit the changes

1. Add all new files to Git:

    ```
    git add .
    ```

1. Commit:

    ```
    git commit -a -m "full solution"
    ```

1. Push to remote Git

    ```
    git push
    ```

1. Watch the Delivery Pipeline processing your commit and deploying a new version of your app.

Congratulations! You completed this lab. You can get familiar with the application code content.


---

## Source code

### Back-end

| File | Description |
| ---- | ----------- |
|**package.json**|Lists the node.js dependencies|
|**.cfignore**|List of files and directories ignored when calling **cf push**. Typically we ignore everything that can be retrieved with bower or npm. This speeds up the push process.|
|**manifest.yml**|Used by Cloud Foundry when pushing the application to define the application environment, connected services, number of instances, etc.|
|**app.js**|Web app backend entry point. It initializes the environment and imports the Todo API endpoints|
|**todos.js**|Todo API implementation. It declares endpoints for PUT/GET/DELETE (create/retrieve/delete) and handles the *in-memory* storage.

---

### Front-end

| File | Description |
| ---- | ----------- |
|**.bowerrc**|Configuration file for the [bower](http://bower.io/) web package manager to put our web dependencies under public/vendor|
|**bower.json**|Web dependencies (bootstrap, angular)|
|**index.html**|Web front-end implementation. It displays the todo list and has a form to submit new todos.|
|**todo.js**|Declares the Angular app|
|**todo.service.js**|Implements the connection between the front-end and the back-end. It has methods to create/retrieve/delete Todos|
|**todo.controller.js**|Controls the main view, loading the current todos and adding/removing todos by delegating to the Todo service|

---

# Resources

For additional resources pay close attention to the following:

- [GitHub Guides](https://guides.github.com/)
- [Get started guides for your favorite runtimes](https://www.ibm.com/blogs/bluemix/2017/03/runtimes-get-started-guides/?social_post=829410659&fst=Learn&linkId=35308736)
