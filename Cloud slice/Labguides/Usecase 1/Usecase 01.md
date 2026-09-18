## Usecase 01-Develop a CRUD-enabled Todo application with Rayfin in Fabric Apps

**Introduction**

This use case demonstrates how to develop and deploy a CRUD (Create,
Read, Update, Delete) enabled To-Do application using **Rayfin** within
**Microsoft Fabric Apps**. The exercise provides hands-on experience in
creating a Microsoft Fabric workspace, deploying a prebuilt To-Do App
template, configuring the development environment, running the
application locally, and publishing it to Fabric. Participants learn how
Fabric Apps simplifies full-stack application development by providing
integrated backend services, authentication, data storage, and
deployment capabilities.

**Objective**

- Create and configure a Microsoft Fabric workspace for application
  development.

- Deploy a To-Do App using the Rayfin-powered App template available in
  Fabric Apps.

- Set up a local development environment using Visual Studio Code,
  Node.js, and Rayfin tooling.

- Perform CRUD operations on to-do items through the application
  interface.

- Test the application locally and validate functionality.

- Publish the application to Microsoft Fabric using Rayfin deployment
  commands.

- Verify data persistence by confirming that to-do records are stored in
  the underlying Fabric SQL database.

- Understand the end-to-end application lifecycle within the Microsoft
  Fabric ecosystem.

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
    +++<https://app.fabric.microsoft.com/+++> then press
    the **Enter** button and sign in with your credentials

| Credential | Value |
|---|---|
| Username | `[+++@lab.CloudPortalCredential](mailto:+++@lab.CloudPortalCredential)(User1).Username+++` |
| Password | `[+++@lab.CloudPortalCredential](mailto:+++@lab.CloudPortalCredential)(User1).Password+++` |

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
| Name | `Rayfin-Fabric-TodoappXXXX` (**XXXX can be a unique number**) |
| Advanced | Under **License mode**, select **Fabric** |
| Default storage format | **Small dataset storage format** |

![](./media/image5.png)

![](./media/image6.png)

![](./media/image7.png)

5.  Once the workspace loads, copy the URL from the browser address bar.
    Remove anything after the workspace ID. The URL should look
    like https://app.fabric.microsoft.com/groups/\<workspace-id\>.

> ![](./media/image8.png)

# Task 2: Create a Fabric App

1.  Create a new lakehouse by clicking on the **+New item** button in
    the navigation bar.

![](./media/image9.png)

2.  In the New item dialog, enter +++**app+++** in the search box, and
    then select **App (preview)** from the search results

![](./media/image10.png)

3.  On the Pick a template to get started page, select the **To-Do App**
    template.

![](./media/image11.png)

4.  After you select the *To-Do App* template, the app deployment starts
    automatically. Wait approximately 2 to 3 minutes for the deployment
    to complete

![](./media/image12.png)

![](./media/image13.png)

![](./media/image14.png)

5.  In the Getting started section, under Set up your project, select
    the **copy icon** to copy the scaffold command to your clipboard.

![](./media/image15.png)

# Task 3: Deploy the backend and the app

1.  In File Explorer, navigate to **C:\LabFiles**, create a new folder
    named **Todo-app.**

![](./media/image16.png)

2.  In your Windows search box, type Visual Studio, then click
    on **Visual Studio Code**.

> ![A screenshot of a computer Description automatically
> generated](./media/image17.png)

3.  In Visual Studio Code, select **File \> Open Folder**, and then
    browse to and open the **C:\LabFiles\Todo-app** folder.

> ![](./media/image18.png)
>
> ![](./media/image19.png)

4.  When the Workspace Trust dialog appears, select **Yes, I trust the
    authors** to open the folder and enable all features in Visual
    Studio Code.

> ![](./media/image20.png)

5.  In Visual Studio Code, click the **More Actions (⋯)** menu, select
    **Terminal**, and then choose **New Terminal** to open a new
    integrated terminal window

> ![](./media/image21.png)

6.  In the Visual Studio Code terminal, paste the copied scaffold
    command, and then press enter to create the To-Do app project in the
    Todo-app folder.

> ![](./media/image22.png)

7.  Enter Y

> ![](./media/image23.png)
>
> ![](./media/image24.png)

8.  Wait for the project scaffolding process to complete. When the
    Project created successfully! message appears in the terminal, the
    To-Do app project is ready for development.

> ![](./media/image25.png)

9.  In the Visual Studio Code terminal, type +++**cd to-do-app**+++ and
    press Enter to navigate to the newly created project directory.

> ![](./media/image26.png)

10. Edit your app code directly. Run it locally against your Fabric
    backend.

> **+++npm run dev+++**
>
> ![](./media/image27.png)

11. Copy the local frontend URL shown in the terminal, which should be
    similar to +++http://localhost:5173+++, and open it in a new browser
    tab.

> ![](./media/image28.png)

12. Select the **Sign in with Microsoft** button, sign in with the same
    Microsoft account you used for Fabric:

    - **Email**: @lab.CloudPortalCredential(User1).Username

    - **TAP**: @lab.CloudPortalCredential(User1).AccessToken

> ![](./media/image29.png)
>
> ![](./media/image30.png)

13. In the Todo App, enter +++**Create Fabric Workspace**+++ in the
    input box, and then select **Add** to create a new to-do item.

> ![](./media/image31.png)
>
> **+++Create Fabric App+++**
>
> ![](./media/image32.png)

14. In the To-Do list, select the circle next to Create Fabric Workspace
    to mark the task as completed.

> ![](./media/image33.png)
>
> ![](./media/image34.png)
>
> ![](./media/image35.png)

15. Back in the Visual Studio Code terminal, stop the Vite dev server by
    pressing **Ctrl+C**.

16. When you're ready, deploy your updates to Fabric.

**+++npx rayfin up+++**

> ![](./media/image36.png)

17. In the Visual Studio Code terminal output, locate the published
    application URL, press Ctrl and select the URL
    (https://happy-pearl-cd18684b37-westus2.webapp.fabricapps.net) to
    open the deployed application in your default web browser.

> ![](./media/image37.png)
>
> ![](./media/image38.png)

18. Select the **Sign in with Microsoft** button, sign in with the same
    Microsoft account you used for Fabric:

    - **Email**: @lab.CloudPortalCredential(User1).Username

    - **TAP**: @lab.CloudPortalCredential(User1).AccessToken

> ![](./media/image39.png)
>
> ![](./media/image40.png)

19. Open the Microsoft Fabric portal
    at +++https://app.fabric.microsoft.com+++.

Open the **Rayfin\_<Fabric@lab.LabInstance.Id>** workspace you created
in Task 1

20. Select **To do_app**

> ![](./media/image41.png)
>
> ![](./media/image42.png)

21. In the SQL database explorer, expand **To do app \> dbo \> Tables**,
    and then select the Todos table to verify that the to-do items are
    stored successfully and that the Create Fabric Workspace task is
    marked as completed.

> ![](./media/image43.png)

22. In the Fabric portal, select the *Rayfin-Fabric-TodoappXXXXX*
    workspace from the navigation pane.

> ![](./media/image44.png)

23. Select the ... option under the workspace name and
    select **Workspace settings**.

> ![](./media/image45.png)

24. Navigate to the bottom of the General tab and select **Remove this
    workspace**.

![](./media/image46.png)

![](./media/image47.png)

![](./media/image48.png)
