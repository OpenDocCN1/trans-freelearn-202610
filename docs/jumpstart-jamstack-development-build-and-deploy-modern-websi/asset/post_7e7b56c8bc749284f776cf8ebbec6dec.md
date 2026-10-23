
Finally, as the output also states, by adding the --prod flag, deploy will be deployed and replace the current version in production if the deployment succeeds.

```
netlify deploy --prod
```

Additionally, in order to open the URL in the browser, add the --open flag as before:

```
netlify deploy --prod --open
```

The output from this command will be similar to the following:

```
Deploy path:          /home/chris/jumpstart-jamstack/hands-on-jamstack/web/public
Configuration path: /home/chris/jumpstart-jamstack/hands-on-jamstack/netlify.toml
Deploying to main site URL...
Finished hashing 1 files
CDN requesting 0 files
Finished uploading 0 assets
Deploy is live!
Logs:                 https://app.netlify.com/sites/jumpstart-jamstack-web/deploys/5f3cb84fbdd067963e250875
Unique Deploy URL: https://5f3cb84fbdd067963e250875--jumpstart-jamstack-web.netlify.app
Website URL:         https://jumpstart-jamstack-web.netlify.app
```

Unique Deploy URL provides a way to rollback to this particular deployment in the future, while Website URL, as expected, is the newly deployed version of the website.

In this section, we have learned how to build and deploy on Netlify using the command line. Next, we will learn another way to deploy Jamstack websites using Azure.

## Azure Static Web Apps deployment

A preview of the Azure Static Web Apps service was introduced, placing yet another competitor in the Jamstack hosting and deployment space. The URL to try out this service can be found at https://azure.microsoft.com/en-us/services/app-service/static/.

## Static web app creation

To create a static web app using Azure, follow these steps in order:

- 1. Start by creating an Azure account. The steps for creating a free Azure account may vary depending on the country. The following URL may take you to the signup page for your country: https://azure.microsoft.com/free/.
- 2. After signup, you should eventually arrive at the Azure portal location (https://portal.azure.com/). Here is a screenshot of the top navigation of this screen:

![](img/97ab031f38e2174e2f1b51da18dfa1d0_222_0.png)

Figure 14.6 – The Azure portal top navigation

The various Azure services are listed. As shown in Figure 14.6, Static Web Apps (Preview) is one of the options.

- 3. Placing the mouse on the **Static Web Apps (Preview)** button, the following menu will be displayed:

![](img/97ab031f38e2174e2f1b51da18dfa1d0_222_1.png)

Figure 14.7 – Static Web Apps creation

- 4. Click on **Create** to create the jamstack app.

5. Next, we will fill in the project details, Static Web App details, and Source Control Details fields:

![](img/97ab031f38e2174e2f1b51da18dfa1d0_223_0.png)

Figure 14.8 – Resource Group creation

6. Create a new resource group, enter a name for the static app, and then select an appropriate region.

7. Next, for Source Control Details, click the blue Sign in with GitHub button to sign in to your GitHub account. Authorize Azure to access your GitHub account.

8. In Source Control Details, choose your GitHub account name for Organization, and the appropriate Branch, which is usually the master:

> If you can't find an organization or repository, you might need to enable additional permissions on GitHub.

Organization *
chrispecoraro

Repository *
hands-on-JAMstack

Branch *
main

Figure 14.9 – Organization, Repository, and Branch selection on the configuration form

9. Finally, in the **Build Details** section, for the **Build Presets** option, select **Gatsby** in the *Static site generator* section:

### Create Static Web App (Preview)

Repository *  
`jumpstart-jamstack`

Branch *  
`production`

**Build Details**  
Enter values to create a GitHub Actions workflow file for build and release. You can modify the workflow file later in your GitHub repository.

Build Presets  
`Gatsby`  
ℹ These fields will reflect the app type's default project structure. Change the values to suit your app.

App location * ⓘ  
`/`

Api location ⓘ  
`api`

App artifact location ⓘ  
`public`

![](img/97ab031f38e2174e2f1b51da18dfa1d0_224_0.png)

10. Lastly, and most importantly, set the **App location** to `/web/` since that is where the Gatsby application is located in our project. **Api location** is to be left blank (delete `api`).

11. At the bottom, click the **Review + create** button, followed by the blue **Create** button. If everything is correct, the static website should be deployed. The deployment begins as queued and then begins building, so the entire process requires a few minutes:

### Create Static Web App (Preview)

Basics   Tags   Review + create

App Service Static Web Apps is a streamlined, highly efficient solution to take your static app from source code to global high availability. Pre-rendered files are served from a global footprint with no web servers required. Learn more

### Project Details

Select a subscription to manage deployed resources and costs. Use resource groups like folders to organize and manage all your resources.

Subscription *
Azure subscription No technical support

Resource Group *
(New) JumpstartJamstack
Create new

### Static Web App details

Name *
JSJ

Region *
East US 2

SKU
Free

### Source Control Details

GitHub account
chrispecoraro

> If you can't find an organization or repository, you might need to enable additional permissions on GitHub.

Organization *
chrispecoraro

Repository *
jumpstart-jamstack

Branch *
production

### Build Details

Enter values to create a GitHub Actions workflow file for build and release. You can modify the workflow file later in your GitHub repository.

Build Presets
Gatsby

These fields will reflect the app type's default project structure. Change the values to suit your app.

App location *
/web

Api location
e.g. "api", "functions", etc...

Output location
public

Build Presets
Gatsby

These fields will reflect the app type's default project structure. Change the values to suit your app.

App location *
/web

Api location
e.g. "api", "functions", etc...

Output location
public

Review + create   < Previous   Next : Tags >

## Static web app continuous deployment

Following deployment of the static web app, the screen shown in Figure 14.12 is displayed:

- 1. Clicking on the blue **Go to resource** button takes us to the **Essentials** screen:

![](img/97ab031f38e2174e2f1b51da18dfa1d0_226_0.png)

Figure 14.12 – The Azure Static Web Apps service deployment confirmation screen

- 2. The **Essentials** screen, which is accessed from the **Overview** menu item on the left-hand menu, displays the following information:

**Resource group**

**Location** (region)

**Subscription** and **Subscription ID**

**Tags**, if applicable

**URL**, which can be clicked to access the static web app

**Source**, which is the deployed GitHub branch, **schema** in this case

**Deployment history**, which is used to access the deployments from GitHub

Edit workflow, which is used to edit the text-based workflow file in .yml format:

![](img/97ab031f38e2174e2f1b51da18dfa1d0_227_0.png)

Figure 14.13 – The Azure Static Web Apps service Essentials screen

- 3. Clicking on the URL or, alternatively, the Browse button in the top menu will launch the web application in the browser.
- 4. Clicking on GitHub Action runs will lead us to the Workflows screen, as shown in Figure 14.14:

![](img/97ab031f38e2174e2f1b51da18dfa1d0_227_1.png)

Figure 14.14 – The Azure Static Web Apps CI/CD screen

All the deployments are displayed in the preceding screenshot.

5. Clicking on the Azure Static Web Apps CI/CD link (beginning with ci:) will bring us to the management screen to view the deployment details for that workflow:

![](img/97ab031f38e2174e2f1b51da18dfa1d0_228_0.png)

Figure 14.15 – A collapsed view of the Build and Deploy job screen

In the left menu, by clicking on the Build and Deploy job link, the details of the deployment may be viewed as shown in Figure 14.16:

The following screenshot shows what an expanded view looks like:

![](img/97ab031f38e2174e2f1b51da18dfa1d0_228_1.png)

Figure 14.16 – Excerpt of the expanded view of the Build and Deploy job screen

In the expanded view of the Build and Deploy tab, the actual output of the deployment is shown. The text is nicely colored red, yellow, and green to show warnings and success messages.

While, at time of writing, the Static Web Apps service on Azure is still a preview release, it definitely demonstrates a viable alternative to Netlify.
