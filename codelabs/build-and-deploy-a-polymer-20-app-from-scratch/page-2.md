# 2. Getting set up

## Install Git

Git is a version control tool.

### [**`Download the Git Installer`**](https://github.com/googlecodelabs/tensorflow-style-transfer-android/archive/codelab-start.zip)


1. Run the Git installer
1. Check whether Git is correctly installed: 
    ```
    git --version
    ```
If all is well, Git tells you its version info:

![](https://codelabs.developers.google.com/codelabs/whose-flag/img/93f6f173fa773d99.png)

If you don't see a Git version number at this point, you may need to refer to the [official Git installation instructions](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

## Install Node and npm

[Node](https://nodejs.org/en/about/) is a JavaScript runtime environment. [npm](https://www.npmjs.com/) is a package manager for Node. They will both be installed when you install Node.

### [**`Download the Node installer`**](https://nodejs.org/en/download/)

1. Download and run the Node installer (this will install npm as well).
1. Update npm to the latest version:            
    
        npm install npm@latest -g
1. Check that Node and npm are correctly installed. Run the following commands:

        node -v 
        
![](https://codelabs.developers.google.com/codelabs/whose-flag/img/6fef5cb42361ceb1.png)
 
    npm -v
![](https://codelabs.developers.google.com/codelabs/whose-flag/img/7c005028f5642e46.png)

If you don't see version numbers for Node and npm, you may need to refer to the [official installation instructions on the npm website](https://www.npmjs.com/get-npm).

## Install Bower

Bower is a package manager for the Web. Your Polymer project will rely on Bower to manage and track the Web Components it depends on, and their version information.

After installing npm, you will be able to install Bower using npm.

1. Run the following command: 
```npm
npm install -g bower
```
2. Check that Bower is installed correctly:
```
bower -v
```
![](https://codelabs.developers.google.com/codelabs/whose-flag/img/6c52150eb0bd53ba.png)

### Install the Polymer CLI

The Polymer CLI is what you'll use to generate your app from a template, view the working app locally, and package it up ready for deployment.

1. Use npm to install the Polymer CLI:
```
npm install -g polymer-cli
```
2. Check that the Polymer CLI installed correctly:
```
polymer --version
```
![](https://codelabs.developers.google.com/codelabs/whose-flag/img/cd82d720df51be27.png)

Congratulations! You've configured a development environment for Polymer apps.

