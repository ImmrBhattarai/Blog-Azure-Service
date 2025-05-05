# Creating a containerized WebApp in Azure | DIO Project
Creating a blog using Azure services, such as App Service or Container Apps, to demonstrate how web applications hosted in the cloud work.
---

# Introduction:

**Project Goal:** Explain that the project demonstrates building a simple web application (a blog) and deploying it as a container to Azure Container Apps.

**Technology Stack:** List the key technologies used: HTML, JavaScript (with localStorage), Bootstrap CSS, Docker, Nginx, Azure CLI, Azure Container Registry (ACR), and Azure Container Apps.

**Target Audience:** Mention who would find this article useful (e.g., developers learning about containerization, Azure cloud deployment, or simple frontend development).
---

## Here's what goes with this project.

1. Develop the Blog Frontend:

-> Created the main page (index.html) to list blog posts. It uses Bootstrap for styling and JavaScript to fetch and display posts stored in the browser's localStorage.

-> Created the post creation page (create-post.html). This page includes a form to input the title and content of a new post. JavaScript handles saving the new post to localStorage and redirecting back to the index page.

-> Created the post detail page (post-detail.html). This page displays the full content of a selected post and allows users to add comments. It uses JavaScript to retrieve the specific post and its associated comments from localStorage, display them, and save new comments.

-> Implemented basic data persistence using localStorage to store posts and comments directly in the user's browser.

-> Included input sanitization using a basic escapeHTML function in create-post.html and post-detail.html to prevent simple HTML injection.


2. Containerize the Application:

-> Created a Dockerfile to package the web application.

-> Used the nginx:alpine base image, which is a lightweight web server.

-> Copied all HTML files (*.html) from the project directory into the Nginx default web root (/usr/share/nginx/html/) within the container.

-> Exposed port 80, the default HTTP port used by Nginx.


3. Automate Build and Deployment:

-> Created a PowerShell script (script.ps1) to automate the build and deployment process to Azure.

-> Build & Local Test: The script first builds the Docker image using the Dockerfile and tags it as blog-suraj-app:latest. It then runs the container locally, mapping port 80 of the host to port 80 of the container, allowing for local testing.

-> Azure Setup:
* Logs into Azure using az login.
* Creates an Azure Resource Group named diocontainer using az group create.
* Creates an Azure Container Registry (ACR) named blogsurajregistry within the resource group using az acr create.

-> Push to ACR:
* Logs into the created ACR using az acr login.
* Tags the local Docker image with the ACR's login server name (blogsurajregistry.azurecr.io/blog-suraj-app:latest) using docker tag.
* Pushes the tagged image to ACR using docker push.

-> Deploy to Azure Container Apps:
* Creates an Azure Container Apps Environment named blog-suraj-env using az containerapp env create.
* Creates the Azure Container App named blog-suraj-app using az containerapp create. This command specifies:
    + The resource group (diocontainer).
    + The image to use from ACR (blogsurajregistry.azurecr.io/blog-suraj-app:latest).
    + The Container App Environment (blog-suraj-env).
    + The target port inside the container (80).
    + External ingress to make the app accessible from the internet.
    + The ACR registry server, username, and password (Note: Hardcoding credentials like this is not recommended for production; consider using service principals or managed identities).


4. Version Control Setup (Optional but Recommended):

-> Initialized a Git repository (presumably).
-> Created a .gitignore file. The current configuration (!index.html) seems intended to force inclusion of index.html while potentially ignoring other HTML files if a broader pattern like *.html were added later and then negated. It also explicitly includes the Dockerfile and script.ps1.


## To Replicate This Project:

1. Ensure you have Docker, Azure CLI, and Git installed.
2. Create the HTML files (index.html, create-post.html, post-detail.html) with their respective JavaScript logic.
3. Create the Dockerfile as shown.
4. Create the script.ps1 deployment script. Important: Replace the hardcoded ACR password with a secure method in a real-world scenario. You might also need to adjust resource names if diocontainer or blogsurajregistry already exist in your Azure subscription.
5. Run the PowerShell script (./script.ps1 in PowerShell or pwsh ./script.ps1 in bash if PowerShell Core is installed) to build the image, push it to Azure, and deploy the Container App. You will be prompted to log in to Azure during the script execution.
6. Access the deployed application via the URL provided by the az containerapp create command output.