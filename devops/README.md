# DevOps avec Bluemix

<!-- page_number: true -->
<!-- $size: 16:9 -->
<!-- prerender: true -->
<!-- footer: OPEN GROUPE - Formation Bluemix - JUIN 2017 -->

Adoptez une approche DevOps en utilisant le Continuous Delivery, qui inclut une chaîne d'outils ouverte automatisant la génération et le déploiement d'applications. Vous pouvez commencer en créant une chaîne d'outils de déploiement simple qui prend en charge les tâches de développement, de déploiement et les opérations.
Vous utiliserez l'application Todo avec les outils DevOps de Bluemix.


![Todo](./images/screenshot.png)


# Objectif

Dans l'exercice suivant, vous allez apprendre à :

+ Mettre en place un dépot pour votre code source afin de collaborer
+ Gérer l'intégration continue et le déploiement continu

# Prérequis

+ Avoir un [Bluemix IBM id](https://bluemix.net), ou  utiliser son compte existant.
+ Installer le [Bluemix CLI](http://clis.ng.bluemix.net)
+ Installer un [Git client](https://git-scm.com/downloads)
+ Installer [Node.js](https://nodejs.org)


----

# Etapes


1. [Activer le déploiement continu](#etape-1------activer-le-déploiement-continu)

1. [Soumettre votre changement et le voir se déployer automatiquement](#etape-2------soumettre-votre-changement-et-le-voir-se-déployer-automatiquement)

---


# Etape 1 - Activer le déploiement continu

Maintenant ajoutez un dépot pour votre code source et un pipeline de déploiement automatique à votre projet. Le dépot Git et la gestion des issue tracking est géré:
  + soit par IBM et fonctionne avec GitLab Community Edition en mode privé.
  ![Toolchain](./images/toolchain-gitlab.png)


  + soit par GitHub.com en mode public et/ou privé avec un abonnement.
  ![Toolchain](./images/toolchain-github.png)



1. Depuis la page **Overview** de votre application, recherchez **Continuous Delivery** et cliquez sur le bouton **Enable**.

1. Une nouvelle fenêtre s'ouvre pour configurer la chaine d'outil, Toolchain.
La Toolchain contient un dépot Git, un Pipeline de déploiement et un IDE web.
![Toolchain](./images/devops-enable.png)

1. La toolchain propose un nom par défaut qui est modifiable. Dans **Configurable Integrations** en bas, selectionner **Git Repos et Issue Tracking**.

1. Garder les options par défaut **Clone** pour cloner le code source de l'application
 "Hello World!" dans votre compte GitLab ou GitHub.
 ![Toolchain](./images/devops-enabled.png)


1. La toolchain a été configuré avec succès. Un nouveau dépot Git a été créé, ainsi qu'un Pipeline qui pourra déployer votre application automatiquement à chaque commit.
![Toolchain](./images/devops-created.png)


1. Ouvrez le dépot Git et notez son URL.


1. Ouvrir un terminal ou une invite de commande afin de cloner le repository git

    ```
    git clone <URL-OF-YOUR-GIT-REPO>
    ```

1. Cette commande crée un répertoire de votre projet localement sur votr disque dur.


Dans le chapitre précédent, vous poussiez le changement manuellement. Ici, vous bénéficierez des fonctionnalités du dépot Git et du pipeline de déploiement automatique.

# Etape 2 - Soumettre votre changement et le voir se déployer automatiquement

1. Ouvrir le fichier **public/index.html**.

1. Changer le titre de la page à la ligne 5.

1. Valider que le changement fonctionne en local.

1. Soumettre votre changement localement
    ```
    git commit -a -m "updated title"
    ```

    Note: Vous devrez peut-etre configurer pour la premierè fois:
    ```
    git config --global user.email "you@example.com"
    git config --global user.name "Your Name"
    ```

1. Pousser votre changement
    ```
    git push
    ```

1. Revenir à la console Bluemix, aller dans l'onglet **Overview** de votre application.

1. Cliquer sur le bouton **View Toolchain** dans la section Continuous Delivery.

1. Cliquer sur **Delivery Pipeline** qui a été créé précédemment.

1. Regarder la prise en compte de votre changement par le Delivery pipeline  qui rédéploie l'application.

1. Quand la commande est terminée, accéder à l'application s'éxécutant dans le cloud pour confirmer que le changement a été déployé

---
## Enjoy Bluemix ! :+1:
