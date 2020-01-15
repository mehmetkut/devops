# Working with Git Locally

Git and Continuous Delivery is one of those delicious chocolate & peanut butter combinations we occasionally find in the software world, two great tastes that taste great together! Continuous Delivery of software demands a significant level of automation. It's hard to deliver continuously if you don't have a quality codebase. Git provides you with the building blocks to really take charge of quality in your
codebase; it gives you the ability to automate most of the checks in your codebase even before committing the code into you repository. To fully appreciate the effectiveness of Git, you must first understand how to carry out basic operations on Git, such as clone, commit, push, and pull.


The natural question is, how do we get started with Git? One option is to go native with the command line or look for a code editor that supports Git natively. Visual Studio Code is a cross-platform open source code editor that provides a powerful developer tooling for hundreds of languages. To work in the open source, you need to embrace open source tools. In this recipe, we'll start off by setting up the
development environment with Visual Studio Code, create a new Git repository, commit code changes locally, and then push changes to a remote repository on Azure DevOps Server.


# Getting ready

In this tutorial, we'll learn how to initialize a Git repository locally, then we'll use the ASP.NET Core MVC project template to create a new project and version it in the local Git repository. We'll then use Visual Studio Code to interact with the Git repository to perform basic operations of commit, pull, and push.

You'll need to set up your working environment with the following:

- .NET Core 2.0 SDK or later: https://www.microsoft.com/net/download/macos
- Visual Studio Code: https://code.visualstudio.com/download
- C# Visual Studio Code extension: https://marketplace.visualstudio.com/items?itemName=ms-vscode.
csharp
- Git: https://git-scm.com/downloads
- Git for Windows (if you are using Windows): https://gitforwindows.org/
The Visual Studio Marketplace features several extensions for Visual Studio Code that you can install to enhance your experience of using Git:
- Git Lens (https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens): This extension brings visualization for code history by leveraging Git blame annotations and code lens. The extension enables you to seamlessly navigate and explore the history of a file or branch. In addition to that the extension allows you to gain valuable insights via powerful comparison commands, and so much more.
- Git History (https://marketplace.visualstudio.com/items?itemName=donjayamanne.githistory): Brings
visualization and interaction capabilities to view the Git log, file history, and compare branches or
commits.

# How to do it…
1. Open the Command Prompt and create a new working folder:

    
    ```
    mkdir myWebApp
    cd myWebApp
    ```
2. In myWebApp, initialize a new Git repository:
    
    ```
    git init
    ```
3. Configure global settings for the name and email address to be used when committing in this Git repository:
    ```
    git config --global user.name "Tarun Arora"
    git config --global user.email "tarun.arora@contoso.com"
    ```

    If you are working behind an enterprise proxy, you can make your Git repository proxy-aware by adding the proxy details in the Git global configuration file. There are different variations of this command that will allow you to set up an HTTP/HTTPS proxy (with username/password) and optionally bypass SSLverification. Run the below command to configure a proxy in your global git config.
    ```
    git config --global http.proxy
    http://proxyUsername:proxyPassword@proxy.server.com:port
    ```
4. Create a new ASP.NET core application. The new command offers a collection of switches that can be used for language, authentication, and framework selection (more details can be found on Microsoft docs: https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-new?tabs=netcore2x)
   
    ```
    dotnet new mvc
    ```
    Launch Visual Studio Code in the context of the current working folder:
    ```
    code .
    ```
5. When the project opens up in Visual Studio Code, select Yes for the Required assets to build and debug are missing from ‘myWebApp’. Add them? warning message. Select Restore for the There are unresolved dependencies info message. Hit F5 to debug the application, then myWebApp will load in the browser.

    If you prefer to use the commandline you can run the following commands in the context of the git repository to run the web application.
    
    ```
    dotnet build
    dotnet run
    ```
    
    You'll notice the .vscode folder is added to your working folder. To avoid committing this folder into your Git repository, you can include this in the .gitignore file. With the .vscode folder selected, hit F1 to launch the command window in Visual Studio Code, type gitIgnore, and accept the option to include the selected folder in the .gitignore file.

6. To stage and commit the newly-created myWebApp project to your Git repository from Visual Studio Code, navigate to the Git icon from the left panel. Add a commit comment and commit the changes by clicking the checkmark icon. This will stage and commit the changes in one operation.

    Open Program.cs, you'll notice Git lens decorates the classes and functions with the commit history and also brings this information inline to every line of code.

7. Now launch cmd in the context of the git repository and run **git branch --list**. This will show you that currently only master branch exists in this repository. Now run the following command to create a new branch called **feature-devops-home-page**

    ```
    git branch feature-devops-home-page
    git checkout feature-devops-home-page
    git branch --list
    ```

    With these commands, you have created a new branch, checked it out. The --list keyword shows you a list of all branches in your repository. The green colour represents the branch that's currently checked out.

8. Now navigate to the file **~\Views\Home\Index.cshtml** and replace the contents with the text below...

    ```html
    @{
    ViewData["Title"] = "Home Page";
    }
    <div class="text-center">
        <h1 class="display-4">Welcome</h1>
        Types of Source Control Systems 
        <p>Learn about <a href="https://azure.microsoft.com/en-gb/services/devops/">Azure DevOps</a>.</p>
    </div>
    ```

9. Refresh the web app in the browser to see the changes…

10.  In the context of the git repository execute the following commands… These commands will stage the changes in the branch and then commit them.
        ```
        git status
        git add .
        git commit -m "updated welcome page"
        git status
        ```

11.   In order to merge the changes from the feature-devops-home-page into master, run the following commands in the context of the git repository.

        ```
        git checkout master
        git merge feature-devops-home-page
        ```

12. Run the below command to delete the feature branch

    ```
    git branch --delete feature-devops-home-page
    ```

# How it works…

The easiest way to understand the outcome of the steps done earlier is to check the history of the operation. Let's have a look at how to do this…

1. In git, committing changes to a repository is a two step process. Upon running add . the changesare staged but not committed. Finally running commit promotes the staged changes into into the repository.

2. To see the history of changes in the master branch run the command
    ```
    git log -v
    ```

3. To investigate the actual changes in the commit, you can run the command .
    ```
    git log -p
    ```

# There is more…

Git makes it really easy to backout changes, following our example, if you wanted to take out the changes made to the welcome page, this can be done by hard resetting the master branch to a previous version of the commit using the command below…

```
git reset --hard 5d2441f0be4f1e4ca1f8f83b56dee31251367adc
```

Running the above command would reset the branch to the project init change, if you run **git log -v** you'll see that the changes done to the welcome page are completely removed from the repository.
