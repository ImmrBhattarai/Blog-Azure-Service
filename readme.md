# Creating a containerized WebApp in Azure | DIO Project
Creating a blog using Azure services, such as App Service or Container Apps, to demonstrate how web applications hosted in the cloud work.

![Screenshot From 2025-05-05 18-09-53](https://github.com/user-attachments/assets/10b369d7-72c0-4b75-ac34-15edb71142d3)
![Screenshot From 2025-05-05 18-07-55](https://github.com/user-attachments/assets/4fe1c760-a871-4704-9974-5eb2d2761caf)
![Screenshot From 2025-05-05 18-07-23](https://github.com/user-attachments/assets/f7ac6304-7e3a-4341-a22c-76a100db134a)
![Screenshot From 2025-05-05 18-03-36](https://github.com/user-attachments/assets/48515f7d-570c-4b09-8dd0-4f601a8c4eed)
![Screenshot From 2025-05-05 18-03-23](https://github.com/user-attachments/assets/d46ff406-f0ce-48a5-aab9-adb8dd91abca)
![Screenshot From 2025-05-05 18-03-22](https://github.com/user-attachments/assets/00a4efcd-5114-447c-91d0-25489e5ce5ca)
![Screenshot From 2025-05-05 18-01-31](https://github.com/user-attachments/assets/26b2523d-10db-450b-bce9-49aee801031e)

---

# Introduction:

**Project Goal:** building a simple web application (a blog) and deploying it as a container to Azure Container Apps.

**Technology Stack:** HTML, JavaScript (with localStorage), Bootstrap CSS, Docker, Nginx, Azure CLI, Azure Container Registry (ACR), and Azure Container Apps.

**Target Audience:** developers learning about containerization, Azure cloud deployment, or simple frontend development

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

![Screenshot From 2025-05-05 14-40-00](https://github.com/user-attachments/assets/8f7f5e55-f069-4e2c-a208-a80062e43650)

-> Created a PowerShell script (script.ps1) to automate the build and deployment process to Azure.

![Screenshot From 2025-05-05 19-42-34](https://github.com/user-attachments/assets/ba46ef55-be92-482f-acee-ecea70cfa5ff)

-> Build & Local Test: The script first builds the Docker image using the Dockerfile and tags it as blog-suraj-app:latest. It then runs the container locally, mapping port 80 of the host to port 80 of the container, allowing for local testing.

-> Azure Setup:
* Logs into Azure using az login.
* Creates an Azure Resource Group named diocontainer using az group create.
  
  ![Screenshot From 2025-05-05 16-53-07](https://github.com/user-attachments/assets/4ddde9d4-6825-4bde-ba31-529682ff31f6)
  ![Screenshot From 2025-05-05 18-11-06](https://github.com/user-attachments/assets/cd17f4f3-542b-402e-af14-8e81c2ef9bd7)
  
* Creates an Azure Container Registry (ACR) named blogsurajregistry within the resource group using az acr create.
  
  ![Screenshot From 2025-05-05 16-55-42](https://github.com/user-attachments/assets/54789d87-b2e6-467a-8642-fed76812f2b9)

-> Push to ACR:
* Logs into the created ACR using az acr login.
  
  ![Screenshot From 2025-05-05 17-07-55](https://github.com/user-attachments/assets/e4d9d1bb-9919-48e8-9b7d-2d9968a53a2e)
  
* Tags the local Docker image with the ACR's login server name (blogsurajregistry.azurecr.io/blog-suraj-app:latest) using docker tag.
  
  ![Screenshot From 2025-05-05 17-10-18](https://github.com/user-attachments/assets/f79de8e2-fa02-4b4d-a194-5f5f26d67db1)
  
* Pushes the tagged image to ACR using docker push.
  
  ![Screenshot From 2025-05-05 17-11-38](https://github.com/user-attachments/assets/6af4aa45-7d2d-4455-bca0-021c645b707a)


-> Deploy to Azure Container Apps:

  ![Screenshot From 2025-05-05 17-13-15](https://github.com/user-attachments/assets/5fb29fe6-c794-4cb9-bbd4-8e5d533f2c30)
  ![Screenshot From 2025-05-05 17-15-41](https://github.com/user-attachments/assets/9d7dfe0d-0061-4378-a249-5167976e20f3)
  ![Screenshot From 2025-05-05 17-16-23](https://github.com/user-attachments/assets/213edc6a-ab73-4dd3-afab-c7d91180d438)
  ![Screenshot From 2025-05-05 17-19-23](https://github.com/user-attachments/assets/619742f4-9c82-42b2-879a-543b4d4f428d)

* Creates an Azure Container Apps Environment named blog-suraj-env using az containerapp env create.
  
  ![Screenshot From 2025-05-05 17-40-57](https://github.com/user-attachments/assets/294b848c-5fb3-411b-a874-9eda32b2ac8c)
  ![Screenshot From 2025-05-05 17-55-06](https://github.com/user-attachments/assets/9176c05d-531f-41bb-aef4-54a8ee6ed3c6)

* Creates the Azure Container App named blog-suraj-app using az containerapp create. This command specifies:
  
  ![Screenshot From 2025-05-05 18-00-20](https://github.com/user-attachments/assets/374fdb0a-c9b0-4f9a-a738-f5cd9f1e83c0)

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
4. Create the script.ps1 deployment script.

**Important: Replace the hardcoded ACR password with a secure method in a real-world scenario. You might also need to adjust resource names if diocontainer or blogsurajregistry already exist in your Azure subscription.**

7. Run the PowerShell script (./script.ps1 in PowerShell or pwsh ./script.ps1 in bash if PowerShell Core is installed) to build the image, push it to Azure, and deploy the Container App. You will be prompted to log in to Azure during the script execution.
8. Access the deployed application via the URL provided by the az containerapp create command output.
