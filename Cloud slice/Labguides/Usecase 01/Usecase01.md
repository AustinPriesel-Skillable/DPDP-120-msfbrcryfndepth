## Usecase 1- Build and Deploy the Contoso Chef Application with Rayfin

**Scenario**

Contoso Chef is a modern recipe-sharing platform that enables food
enthusiasts to discover recipes, create and manage their own culinary
content, upload recipe images, and collaborate with other users through
likes and comments. To accelerate application development while
minimizing infrastructure complexity, Contoso Chef leverages **Rayfin on
Microsoft Fabric** to provide a fully managed backend, authentication,
database services, and application hosting.

As a developer, your goal is to deploy the Contoso Chef application to
Microsoft Fabric, connect it to a managed Rayfin backend, and explore
how Rayfin simplifies the process of building, deploying, and
maintaining scalable full-stack applications. Throughout this lab, you
will provision Fabric resources, deploy the application, validate
Microsoft SSO authentication, create recipes, and learn how to update
and redeploy application components efficiently.

**Introduction**

Building modern applications often requires developers to manage
multiple services, including authentication, databases, APIs, storage,
and application hosting. Rayfin simplifies this process by providing a
managed backend platform that integrates directly with Microsoft Fabric,
enabling developers to focus on application functionality rather than
infrastructure management.

In this lab, you will deploy the **Contoso Chef** sample application
using Rayfin on Microsoft Fabric. You will create a Fabric workspace,
configure the development environment, deploy a managed backend, launch
the application, and explore its core recipe-sharing features. You will
also learn how local development and production deployments can
seamlessly share the same backend services, enabling rapid application
development and deployment workflows. The Contoso Chef application
demonstrates how Microsoft Fabric and Rayfin can be used together to
build fast, secure, and resilient cloud-native applications with
Microsoft Entra authentication and managed data services.

**Objectives**

- Create and configure a Microsoft Fabric workspace.

- Validate the required development tools and environment.

- Clone and configure the Contoso Chef application source code.

- Install project dependencies and authenticate with Rayfin.

- Deploy a managed backend and application using Rayfin on Microsoft
  Fabric.

- Verify the deployment and access the application through Microsoft
  Entra SSO.

- Run the application locally while connecting to the deployed Fabric
  backend.

- Create, edit, and manage recipes within the Contoso Chef application.

- Upload media assets and interact with recipe content through comments
  and likes.

- Redeploy frontend and database updates using Rayfin deployment
  commands.

- Understand how Rayfin accelerates full-stack application development
  on Microsoft Fabric.

**Prerequisites**

Before starting, make sure you have:

1.  Node.js 20 or later installed. Check with "node -v" in a terminal.
    If it's missing or older, install it from nodejs.org.

2.  Git installed, to clone the sample repository.

3.  Access to a Microsoft Fabric workspace where you have permission to
    create an app (ask your Fabric admin if unsure).

4.  A terminal / command-line application (PowerShell, Terminal, etc.).

# Task 1: Create a Fabric workspace

In this task, you create a Fabric workspace. The workspace contains all
the items needed for this lakehouse tutorial, which includes lakehouse,
dataflows, Data Factory pipelines, the notebooks, Power BI datasets, and
reports.

1.  Open your browser, navigate to the address bar, and type or paste
    the following URL:
    +++https://app.fabric.microsoft.com/+++ then press
    the **Enter** button and sign in with your credentials

| Credential | Value |
|---|---|
| Username | +++@lab.CloudPortalCredential(User1).Username+++ |
| Password | +++@lab.CloudPortalCredential(User1).Password+++ |

![](./media/image1.png)

> ![](./media/image2.png)

2.  In the portal, switch to **Fabric** Mode before proceeding to create
    workspace.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

3.  In the Workspaces pane, click on **+New workspace** tile

![](./media/image4.png)

4.  In the **Create a workspace** pane that appears on the right side,
    enter the following details, and click on the **Apply** button.

| Setting | Value |
|---|---|
| Name | +++Rayfin-FabricXXXX+++ (**XXXX can be a unique number**) |
| Advanced | Under **License mode**, select **Fabric** |
| Default storage format | **Small dataset storage format** |

![](./media/image5.png)

![](./media/image6.png)

![](./media/image7.png)

5.  Once the workspace loads, copy the URL from the browser address bar.
    Remove anything after the workspace ID. The URL should look
    like https://app.fabric.microsoft.com/groups/\<workspace-id\>.

![](./media/image8.png)

# Task 2: Clone the lab repository

1.  Open your browser, navigate to the address bar, type or paste the
    following URL:
    +++https://github.com/technofocus-pte/rayfin-on-microsoft-fabric+++

> ![](./media/image9.png)

2.  Click on **fork** to fork the repo. Give unique name to the repo and
    click on **Create repo** button.

> ![](./media/image10.png)
>
> ![](./media/image11.png)

3.  In your GitHub repository, click **Code** and then select the
    **Copy** icon next to the repository URL to copy the clone link for
    use in the upcoming steps.

> ![](./media/image12.png)

# Task 3: Validate Required Software Setup

1.  In your Windows search box, type Visual Studio, then click
    on **Visual Studio Code**.

> ![A screenshot of a computer Description automatically
> generated](./media/image13.png)

2.  Launch Visual Studio Code and sign in using the **Sign In** button
    located in the upper-right corner of the application window.

![](./media/image14.png)

3.  Click on **Continue with GitHub**

![](./media/image15.png)

4.  On the GitHub sign-in page, enter the provided username or email
    address and password, then click **Sign in** to authenticate and
    connect GitHub with Visual Studio Code.

![](./media/image16.png)

> ![](./media/image17.png)

5.  Click **Open** to open the selected repository and begin working in
    Visual Studio Code.

![](./media/image18.png)

6.  In Visual Studio Code, click the **More Actions (⋯)** menu, select
    **Terminal**, and then choose **New Terminal** to open a new
    integrated terminal window

> ![](./media/image19.png)

7.  In the terminal, navigate to the **Labfiles** directory

> ![](./media/image20.png)

8.  Run the following commands in your terminal and confirm each returns
    a version number:

> **+++node --version+++**
>
> **+++npm --version+++**
>
> **+++git --version+++**
>
> **+++copilot --version+++**

![](./media/image21.png)

# Task 4: Get the source code

1.  Clone the repository and move into the app's source folder:

    +++git clone https://github.com/<youraccount>/rayfin-on-microsoft-fabric.git+++

![](./media/image22.png)

2.  Change the directory

> **+++cd rayfin-on-microsoft-fabric/src+++**

![](./media/image23.png)

All remaining commands are run from this "src" folder.

![](./media/image24.png)

# Task 5: Install dependencies

1.  In the integrated terminal, run the following command to install all
    required project dependencies:

> **+++npm install+++**

This downloads all the packages the app needs (React, Vite, Rayfin CLI,
etc.).

![](./media/image25.png)

2.  In the integrated terminal, run the following command to sign in to
    Rayfin and authenticate your deployment environment

> **+++npx rayfin login+++**

![](./media/image26.png)

![](./media/image27.png)

3.  If your account has access to more than one Fabric tenant, pin to
    the right one:

> +++npx rayfin login --tenant <tenant-id>+++

This opens a browser sign-in prompt — complete it with your Fabric/Entra
ID credentials.

# Task 6: Deploy the backend and the app

1.  After signing in, run the following command to start the deployment
    process:

2.  In the integrated terminal, run the following command, replacing
    **\<workspace-id\>** with the **Microsoft Fabric workspace ID** that
    you saved in Task 1

      +++npx rayfin up --workspace-id <workspace-id>+++

![](./media/image28.png)

![](./media/image29.png)

This single command:

- Provisions a Rayfin item in your Fabric workspace

- Applies the database schema

- Builds the Vite frontend (npm run build:fabric)

- Deploys the static site bundle

- Writes the live URLs and publishable key into a new .env.fabric file

This can take a few minutes the first time.

![](./media/image30.png)

3.  Copy the application **URL** and keep it available for use in the
    next tasks.

![](./media/image31.png)

# Task 7: Open and verify the deployed app

1.  Check the deployment status and get the hosting URL:

> **+++npx rayfin up status+++**

![](./media/image32.png)

Open the printed URL in a browser and sign in with your Microsoft Fabric
account. On first sign-in, the app automatically imports a 100-recipe
catalogue into the database (takes about 30 seconds, with a progress
banner). This "self-seeding" is idempotent, so revisiting later won't
create duplicates.

> **Note on anonymous access:** At release, unauthenticated (anonymous)
> access to Fabric data sources is not supported. Every visitor —
> including the discover page and "unlisted" recipe links — must sign in
> via Microsoft Fabric SSO.

1.  Open the Microsoft Fabric portal
    at +++https://app.fabric.microsoft.com+++.

2.  Open the Rayfin_Fabric@lab.LabInstance.Id workspace you created in Exercise 1.

![](./media/image33.png)

3.  Select **contoso-chef** app

![](./media/image34.png)

![](./media/image35.png)

![](./media/image36.png)

# Task 8: Run the app locally against the deployed backend

1.  For local development with hot-reload:

> +++npm run dev+++

This regenerates a .env file from .env.fabric and starts the Vite dev
server at http://localhost:5173, connected to the same Fabric backend
you deployed in Step 4. Sign in via the popup that appears.

![](./media/image37.png)

2.  Copy the local frontend URL shown in the terminal, which should be
    similar to http://localhost:5173, and open it in a new browser tab.

![](./media/image38.png)

3.  Select the **Sign in with Microsoft** button, sign in with the same
    Microsoft account you used for Fabric:

    - **Email**: @lab.CloudPortalCredential(User1).Username

    - **TAP**: @lab.CloudPortalCredential(User1).AccessToken

![](./media/image39.png)

![](./media/image40.png)

![](./media/image41.png)

![](./media/image42.png)

![](./media/image43.png)

4.  In the terminal output from the **npx rayfin up** command you ran in
    Task 6\>Step 3, find the **static hosting URL** printed by the CLI.
    The URL should look similar
    to https://\<random-prefix\>.webapp.rayfin….com.

![](./media/image44.png)

5.  Open the hosting URL in a new browser tab. The app displays the same
    auth page as your local frontend, including the **Sign in with
    Microsoft** button, because both use the same Fabric backend.

![](./media/image44.png)

![](./media/image45.png)

![](./media/image46.png)

# Task 9: Explore core features

1.  Browsing the discover page (public recipes) and select My recipes

2.  Then, click **My recipes** in the navigation menu and create your
    own recipe

![](./media/image47.png)

3.  Enter the details for a new recipe of your choice. For this lab, a
    sample recipe is used to demonstrate the recipe creation process.

> **Description**: Fluffy, fool-proof steamed white rice — a simple side
> that pairs with almost anything.
>
> **Type**: main (or "side", if that option exists)
>
> **Cuisine**: Global / Asian
>
> **Origin country**: (optional — leave blank or pick where you learned
> it)
>
> For the ingredients and steps sections further down the form:
>
> **Ingredients:**
>
> 1 cup white rice (basmati or jasmine)
>
> 2 cups water
>
> 1/2 tsp salt
>
> 1 tsp butter or oil (optional)
>
> **Steps:**

- Rinse the rice under cold water until it runs clear, to remove excess
  starch.

- Add rice, water, salt, and butter/oil to a pot; bring to a boil
  uncovered.

- Once boiling, reduce heat to low, cover tightly, and simmer for 15
  minutes without lifting the lid.

- Remove from heat and let it rest, covered, for 5 minutes.

- Fluff with a fork before serving.

![](./media/image48.png)

![](./media/image49.png)

![](./media/image50.png)

4.  Liking and commenting on recipes

5.  Uploading a cover image when creating/editing a recipe. Browse
    to **C:\LabFiles\\** on your VM, then select ***SteamredRice*** file
    and click on **Open** button.

![](./media/image51.png)

![](./media/image52.png)

6.  Click **Save recipe**

![](./media/image53.png)

![](./media/image54.png)

# Task 10: Redeploying after changes

1.  Deploy the updated backend API metadata and frontend:

> **+++npx rayfin up+++**
>
> ![](./media/image55.png)

2.  To redeploy only the frontend (static web application) after making
    UI changes, run the following command in the integrated terminal

> +++**npx rayfin up staticapp deploy**+++ \# Redeploy frontend only
>
> ![](./media/image56.png)

3.  To apply only the database schema changes without redeploying other
    project components, run the following command in the integrated
    terminal

> +++**npx rayfin up db apply**+++ \# Apply schema changes only
>
> ![](./media/image57.png)

4.  Open the application using the Hosting URL generated during
    deployment. After the application loads successfully, click **My
    recipes** in the navigation menu to access and manage your personal
    recipes

> ![](./media/image58.png)

![](./media/image59.png)

![](./media/image60.png)

5.  Open the Microsoft Fabric portal
    at +++https://app.fabric.microsoft.com+++.

6.  Open
    the Rayfin\_[Fabric**@lab.LabInstance.Id**](mailto:Fabric@lab.LabInstance.Id) workspace
    you created in Exercise 1.

![](./media/image33.png)

4.  Select **contoso-chef** app

![](./media/image34.png)

7.  Click **My recipes** in the navigation menu to access and manage
    your personal recipes.

![](./media/image61.png)

# Task 11: Create Data agent

1.  Now, click on **RayfinFabricXXX** on the left-sided navigation pane.

![](./media/image62.png)

2.  In the **Fabric** home page, select **+New item.** In the Filter by
    item type search box, enter +++**data agent**+++ and select the Data
    agent

![](./media/image63.png)

3.  Enter **+++Rayfin_agent+++** as the Data agent name and
    select **Create**.

![](./media/image64.png)

4.  In **Rayfin_agent** page, select **Add a data source**

![](./media/image65.png)

5.  In the OneLake catalog tab, select the **contoso-chef** SQL database
    and select **Add.**

![](./media/image66.png)

![](./media/image67.png)

6.  Select all tables

![](./media/image68.png)

7.  Enter the following text and click on the **Submit icon** as shown
    in the below image.

> +++How many recipes are available in the Contoso Chef application?+++

![](./media/image69.png)

![](./media/image70.png)

8.  Enter the following text and click on the **Submit** icon as shown
    in the below image.

+++What are the newest recipes added to the platform?+++

![](./media/image71.png)

![](./media/image72.png)

> +++Which cuisine is most popular among users?+++

![](./media/image73.png)

![](./media/image74.png)

9.  Select **Publish**.

![](./media/image75.png)

![](./media/image76.png)

10. After publishing, verify the success message and select **View
    publishing details** to review the agent deployment.

![](./media/image77.png)

# Task 12: Clean up resources

1.  Select your workspace, the **Rayfin_FabricXXXX** from the left-hand
    navigation menu. It opens the workspace item view.

> ![](./media/image78.png)

2.  Select the ... option under the workspace name and
    select **Workspace settings**.

![](./media/image79.png)

3.  Navigate to the bottom of the General tab and select **Remove this
    workspace**.

![](./media/image80.png)

![](./media/image81.png)

![](./media/image82.png)
