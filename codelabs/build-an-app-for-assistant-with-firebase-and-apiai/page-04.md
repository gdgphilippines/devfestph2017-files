# 4. Create Firebase console Project

### Create project

From [Firebase home](https://developers.google.com/firebase) page click Console then click on **Add Project**.

In the "Create a Project" dialog, call the project `animal-guesser`, then click on Create Project. The console may add some additional hex digits to the name. Make note of this name.

#### Import bootstrap database contents from json

1.  Select **Database** from left-nav menu.
2.  Select **Import JSON** from the overflow menu at the top right
3.  Choose the `database.json` file from the `project` directory in your clone from GitHub

![Import JSON Image](https://codelabs.developers.google.com/codelabs/assistant-codelab/img/184553d7cea0cfea.png)

#### Examine the database structure

Walk through the database structure in the console (effectively a binary tree) and how it will be used to power the app's knowledge.

![Database Structure Image](https://codelabs.developers.google.com/codelabs/assistant-codelab/img/63452861b57d0e81.png)