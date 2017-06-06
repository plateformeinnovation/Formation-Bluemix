![](./images/cloudfoundry.png)
<!-- page_number: true -->
<!-- $size: 16:9 -->
<!-- prerender: true -->
<!-- footer: OPEN GROUPE - Formation Bluemix - JUIN 2017 -->

# Introduction

Dans cet exercice, vous allez découvrir les concepts de développement avec Cloud Foundry et les services Bluemix.
Dans cet exercice, vous allez créer une simple application web.
![Todo](./images/screenshot.png)

---

# Objectif

Dans l'exercice suivant, vous allez apprendre à :

+ Déployer une nouvelle application Cloud Foundry  basée sur le runtime Node.js
+ Utiliser la ligne de commande Cloud Foundry

---

# Prérequis

+ Avoir un [Bluemix IBM id](https://bluemix.net), ou  utiliser son compte existant.
+ Installer le [Bluemix CLI](http://clis.ng.bluemix.net)
+ Installer un [Git client](https://git-scm.com/downloads)
+ Installer [Node.js](https://nodejs.org)

---

# Etapes

1. [Créer une nouvelle application web](#etape-1---creer une-nouvelle-application-web)
1. [Contôler le code localement](#step-3---checkout-the-code-locally)
1. [Exécuter l'application localement](#step-4---run-the-app-locally)
1. [Changer un  fichier localement](#step-5---change-a-file-locally)
1. [Pousser  votre changement  local sur le cloud](#step-6---push-your-local-change-to-the-cloud)
1. [Soumettre votre changement et voir le déploiement automatically](#step-7---commit-your-changes-and-see-them-deployed-automatically)

---

# Etape 1 - Créer une nouvelle application web depuis l'interface web

1. Se connecter à la [Console Bluemix](https://console.bluemix.net).

1. Choisir  la  Région **United Kingdom** pour créer votre application.

1. Aller dans le  **Catalogue** Bluemix.

1. Dans la catégorie **Apps** , Choisir **Cloud Foundry Apps**

1. Créer  une nouvelle application avec le runtime ***SDK for Node.js***.
![Node.js](./images/nodejs.png)
1. Donner un nom unique à votre application (exemple:webapp-[vos-initials])
![Create app](./images/deploy-create-app.png)

On visualise facilement aux détails de cette application.
![Create app](./images/deploy-app-created.png)

1. Accéder à votre application.

Le runtime SDK for Node.js a créer une simple application web "Hello World!" qui nous servira comme point de départ.
![Create app](./images/deploy-app-helloworld.png)

---

# Etape 2 - Déployer une application web depuis la ligne de commande

1. Une fois l'application HelloWorld déployée, on peut récupérer le code source utilisé pour s'en inspirer à partir du menu Getting Started de l'application.


# Etape 3 - Vérifier le code  localement

1. Ouvrir un terminal ou une invite de commande afin de cloner le repository git

    ```
    git clone <URL-OF-YOUR-GIT-REPO>
    ```

1. Cette commande crée un répertoire de votre projet localement sur votr disque dur.

---

# Etape 4 - Exécuter l'application localement

1. Se déplacer sur le répertoire du projet

    ```
    cd webapp-[your-initials]
    ```

1. Installer les dépendances node.js pour ce projet

    ```
    npm install
    ```

1. Démarrer l'application

    ```
    npm start
    ```

    Une fois démarrée, la console doit affichée:

    ```
    > NodejsStarterApp@0.0.1 start /Users/jeromedruais/Documents/OPEN/codes/webapp-jd
    > node app.js

    server starting on http://localhost:[port-number]
    ```

1. Accéder à l'application avec votre navigateur web
![Create app](./images/deploy-app-local.png)

---

# Etape 5 - Changer un fichier localement

1. Ouvrir le fichier **public/index.html**, modifier le message d'accueil à la ligne 19

1. Recharger la page web pour confirmer le changement localement
![Create app](./images/deploy-app-local-modif.png)


---

# Etape 6 - Pousser votre changement local sur le cloud

Cloud Foundry repose sur le fichier *manifest.yml* pour savoir ce qu'il faut faire quand vous poussez une application sur Bluemix.
Un fichier manifest.yml a été généré par défaut pour votre application. Il ressemble à cela:

  ```yml
  applications:
  - path: .
    memory: 256M
    instances: 1
    domain: eu-gb.mybluemix.net
    name: webapp-[your-initials]
    host: webapp-[your-initials]
    disk_quota: 1024M
  ```
---

  Il définit l'infrastructure de l'application à partir du répertoire courant, qui déploiera avec **256MB**, avec **une** instance, sous le domaine  **eu-g.mybluemix.net** .
L'applicationsera nommée **webapp-[your-initials]** et il utilisera **webapp-[your-initials]** comme nom de hote.
Il utilisera **1024MB** de disque comme espace disponible.

1. Spécifier le buildpack a utiliser quand on pousse une application Cloud Foundry app est toujours plus rapide que de 'appuyer sur la détection du buildpack . Modifier le fichier Manifest généré en spécifiant le **buildpack** par la ligne suivante:

    ```yml
    applications:
    - path: .
      memory: 256M
      instances: 1
      domain: eu-gb.mybluemix.net
      buildpack: sdk-for-nodejs
      name: webapp-[your-initials]
      host: webapp-[your-initials]
      disk_quota: 1024M
    ```

1. Se connecter à Bluemix en indiquant le endpoint Bluemix de l'URL avec la région où l'application a été crée.

    ```
    bx api https://api.eu-gb.bluemix.net
    ```

1. S'authentifier à Bluemix

    ```
    bx login
    ```

1. Pousser l'application sur Bluemix

    ```
    bx cf push
    ```

1. Quand la commande est terminée, accéder à l'application s'éxécutant dans le cloud pour confirmer que le changement a été déployé

    ```
    requested state: started
    instances: 1/1
    usage: 256M x 1 instances
    urls: webapp-jd.eu-gb.mybluemix.net
    last uploaded: Tue Jun 6 15:11:45 UTC 2017
    stack: unknown
    buildpack: sdk-for-nodejs

     state     since                    cpu    memory          disk          details
     #0   running   2017-06-06 05:12:28 PM   0.0%   59.7M of 256M   67.7M of 1G
    ```

    ![Create app](./images/deploy-app-bluemix-modif.png)


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



1. Choisir  la  Région **United Kingdom** pour créer votre application.

1. Aller dans le  **Catalogue** Bluemix.

1. Dans la catégorie **Apps** , Choisir **Cloud Foundry Apps**

1. Créer  une nouvelle application avec le runtime ***SDK for Node.js***.
![Node.js](./images/nodejs.png)
1. Donner un nom unique à votre application (exemple:webapp-[vos-initials])
![Create app](./images/deploy-create-app.png)

On visualise facilement aux détails de cette application.
![Create app](./images/deploy-app-created.png)

1. Accéder à votre application.

Le runtime SDK for Node.js a créer une simple application web "Hello World!" qui nous servira comme point de départ.
![Create app](./images/deploy-app-helloworld.png)

---
