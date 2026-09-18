# Usecase 03-Implement a Public Transportation Data Application with Rayfin in Fabric Apps​

**Introduction**

This usecase demonstrates how to build and deploy a real-time public
transportation monitoring application by integrating **Microsoft
Fabric**, **Rayfin**, **Eventstream**, **Eventhouse (KQL Database)**,
**Semantic Models**, and **Fabric Apps**. The solution ingests live
transportation data, processes and stores it in Microsoft Fabric,
creates analytical models, and exposes the data through a modern web
application. This hands-on implementation showcases how organizations
can use Microsoft Fabric's end-to-end analytics capabilities to develop
scalable, real-time operational dashboards and applications for public
transportation systems.

**Objective**

- Set Up the Microsoft Fabric Environment

- Prepare the Development Environment

- Deploy the Real-Time Data Backend

- Build the Analytical Data Model

- Configure Security and Authentication

- Deploy the Rayfin Application

- Validate the End-to-End Solution

**Prerequisites**

Before starting, make sure you have:

1.  Node.js 20 or later installed. Check with "node -v" in a terminal.
    If it's missing or older, install it from nodejs.org.

2.  Git installed, to clone the sample repository.

3.  Access to a Microsoft Fabric workspace where you have permission to
    create an app (ask your Fabric admin if unsure).

4.  A terminal / command-line application (PowerShell, Terminal, etc.).

5.  GitHub account -- You are expected to have your own GitHub login
    credentials. If you do not have, please create one from here -
    +++https://github.com/signup?user_email=&source=form-home-signup+++

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
| Username | `[+++@lab.CloudPortalCredential](mailto:+++@lab.CloudPortalCredential)(User1).Username+++` |
| Password | `[+++@lab.CloudPortalCredential](mailto:+++@lab.CloudPortalCredential)(User1).Password+++` |

![](./media/image1.png)

> ![](./media/image2.png)

2.  In the portal, switch to **Fabric** Mode before proceeding to create
    workspace.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

![](./media/image4.png)

3.  In the Workspaces pane, click on **+New workspace** tile

![](./media/image5.png)

4.  In the **Create a workspace** pane that appears on the right side,
    enter the following details, and click on the **Apply** button.

| Setting | Value |
|---|---|
| Name | `Rayfin-Fabric-TodoappXXXX` (**XXXX can be a unique number**) |
| Advanced | Under **License mode**, select **Fabric** |
| Default storage format | **Small dataset storage format** |

![](./media/image6.png)

![](./media/image7.png)

5.  Once the workspace loads, copy the URL from the browser address bar.
    Remove anything after the workspace ID. The URL should look
    like https://app.fabric.microsoft.com/groups/\<workspace-id\>.

![](./media/image8.png)

# Task 2: Clone the lab repository

1.  Open your browser, navigate to the address bar, type or paste the
    following URL:

+++https://github.com/technofocus-pte/TF-Rayfin+++

2.  Click on **fork** to fork the repo. Give unique name to the repo and
    click on **Create repo** button.

![](./media/image9.png)

3.  In your GitHub repository, click **Code** and then select the
    **Copy** icon next to the repository URL to copy the clone link for
    use in the upcoming steps.

![](./media/image10.png)

# Task 3: Validate Required Software Setup

1.  In your Windows search box, type Visual Studio, then click
    on **Visual Studio Code**.

> ![A screenshot of a computer Description automatically
> generated](./media/image11.png)

2.  In Visual Studio Code, click the **More Actions (⋯)** menu, select
    **Terminal**, and then choose **New Terminal** to open a new
    integrated terminal window

![](./media/image12.png)

3.  In the terminal, navigate to the **Labfiles** directory

![](./media/image13.png)

4.  Run the following commands in your terminal and confirm each returns
    a version number:

> **+++node –version+++**
>
> **+++npm –version+++**
>
> **+++git –version+++**
>
> **+++copilot –version+++**

![](./media/image14.png)

# Task 4: Get the source code

1.  Clone the repository and move into the app's source folder:

     +++git clone https://github.com/<youraccount>/ TF-Rayfin.git+++ 

![](./media/image15.png)

2.  Change the directory

> **+++cd TF-Rayfin/templates/helsinki-public-transport+++**

![](./media/image16.png)

3.  In the Visual Studio Code terminal, run the az login command and
    complete the sign-in process using your Azure account credentials.

> **+++az login+++**

![](./media/image17.png)

4.  In the Sign in window, select Work or school account, and then
    select Continue to sign in with your organizational account.

![](./media/image18.png)

![](./media/image19.png)

![](./media/image20.png)

![](./media/image21.png)

4.  Select your subscription

![](./media/image22.png)

5.  In the Visual Studio Code terminal, run the az account show --query
    tenantId -o tsv command, and then note the displayed tenant ID for
    later use in the configuration steps.

> **+++az account show --query tenantId -o tsv+++**

![](./media/image23.png)

+++az login --tenant \<tenant-id\>+++

![](./media/image24.png)

6.  Select your subscription

![](./media/image25.png)

+++$env:FABRIC_TENANT_ID = "\<tenant-id\>"+++

+++$env:FABRIC_WORKSPACE_ID = "\<workspace-id\>"+++

![](./media/image26.png)

# Task 5: Deploy the Fabric Back End

1.  Creates the Eventhouse (which implicitly creates a KQL database of
    the same name), then reads back its query URI and database id for
    later steps.

+++cd fabric/deploy+++

+++python 01_eventhouse.py+++

> ![](./media/image27.png)

2.  Sends every command in fabric/eventhouse/DatabaseSchema.kql to the
    Kusto management endpoint, one at a time: creates raw_events,
    vehicle_positions, trip_updates and alerts, the three parse
    functions, the last_vehicle_position materialized view, and the
    update policies that wire raw_events to the typed tables. All
    commands are idempotent.

3.  In the Visual Studio Code terminal, navigate to the fabric\deploy
    folder, run the python 02_kql_schema.py command, and then verify
    that the *schema applied* message appears, indicating that the KQL
    schema has been successfully created.

**+++python 02_kql_schema.py+++**

![](./media/image28.png)

4.  Instantiates fabric/eventstream/eventstream.json with fresh node
    GUIDs for the source, stream and destination nodes, and points the
    destination at the Eventhouse from step 1. The source id is recorded
    — the producer notebook needs it to resolve its connection string at
    run time.

5.  In the Visual Studio Code terminal, run the python 03_eventstream.py
    command, and then verify that the event stream is created
    successfully by reviewing the displayed Eventstream ID and source ID
    details.

**+++python 03_eventstream.py+++**

![](./media/image29.png)

6.  Publishes fabric/notebook/notebook-content.py as a Fabric notebook,
    patching in the Eventstream item id and source id so the notebook
    can resolve its own connection string via the Fabric REST API — it
    never stores a secret.

> **+++python 04_notebook.py+++**

![](./media/image30.png)

7.  Publishes the TMDL definition under fabric/semantic-model/,
    rewriting every AzureDataExplorer.Contents(...) partition expression
    to point at this deployment's Kusto cluster URI and database. The
    .platform metadata file is deliberately not uploaded, since Fabric
    assigns that itself.

8.  In the Visual Studio Code terminal, run the python 04_notebook.py
    command, and then verify that the notebook is created successfully
    by confirming that the notebook name and ID are displayed in the
    terminal output.

**+++python 05_semantic_model.py+++**

![](./media/image31.png)

![](./media/image32.png)

9.  Takes ownership of the semantic model's dataset, PATCHes its gateway
    data source with a real Kusto access token, then switches the data
    source to end-user OAuth2 credentials. Skipping this step is the
    single most common cause of a broken lab: executeQueries returns
    HTTP 400 DatasetExecuteQueriesError and the app shows zeros with no
    further explanation.

10. In the Visual Studio Code terminal, run the python
    06_bind_credentials.py command, and then verify that the process
    completes successfully by confirming that the terminal displays
    *final credentialType: OAuth2*.

> +++python 06_bind_credentials.py+++

![](./media/image33.png)

11. Creates an hourly Cron trigger for the notebook. The notebook itself
    runs on a 58-minute budget so successive runs hand over without
    overlapping, and stands down on its own if an older run is still
    active.

> +++python 07_schedule.py+++

![](./media/image34.png)

12. In the Visual Studio Code terminal, run the az ad app create
    --display-name "helsinki-public-transport-spa" --sign-in-audience
    AzureADMyOrg command to create a Microsoft Entra application, and
    then note the generated appId from the command output for use in
    later configuration steps.

      +++az ad app create --display-name "helsinki-public-transport-spa" --sign-in-audience AzureADMyOrg+++


![](./media/image35.png)

13. From the command output, copy the value of the **appId** property
    and save it for use in subsequent deployment and configuration
    steps.

![](./media/image36.png)

![](./media/image37.png)

14. In the Visual Studio Code terminal, run the command to retrieve the
    application ID, store it in the appId variable, and then run echo
    $appId to display and verify the application ID value.

+++$appId = az ad app list --display-name "helsinki-public-transport-spa" --query "[0].appId" -o tsv+++

+++ echo $appId+++

![](./media/image38.png)

15. Open the Azure portal at +++https://portal.azure.com+++, enter
    +++Microsoft Entra ID+++ in the search box, and then select
    Microsoft Entra ID from the search results to open the Microsoft
    Entra administration center.

![](./media/image39.png)

16. In the Microsoft Entra administration center, select **App
    registrations** under **Manage**, and then select the
    **helsinki-public-transport-spa** application to open its
    registration details.

![](./media/image40.png)

17. In the **helsinki-public-transport-spa** application registration,
    under **Manage**, select **API permissions** to view and configure
    the permissions required by the application.

![](./media/image41.png)

18. On the API permissions page, select **Add a permission** to add the
    API permissions required by the helsinki-public-transport-spa
    application.

![](./media/image42.png)

19. In the Request API permissions pane, select the **APIs my
    organization** **uses** tab to browse and select APIs that are
    available within your organization.

![](./media/image43.png)

20. In the Request API permissions pane, on the APIs my organization
    uses tab, enter **Power BI Service** in the search box, and then
    select **Power BI Service** from the search results.

![](./media/image44.png)

21. In the Request API permissions pane for Power BI Service, select
    **Delegated permissions** to grant the application access to Power
    BI APIs on behalf of the signed-in user.

![](./media/image45.png)

22. In the Request API permissions pane, expand **Dataset,** select the
    **Dataset.Read.All** permission, and then select Add permissions to
    grant the application read access to all datasets available to the
    signed-in user.

> ![](./media/image46.png)

![](./media/image47.png)

23. In the Visual Studio Code terminal, create the .env.production.local
    file, add the required dataset ID, client ID, and tenant ID values,
    and then save the file to configure the application for production
    deployment.
```
@"
VITE_PBI_DATASET_ID=
VITE_PBI_CLIENT_ID=
VITE_PBI_TENANT_ID=
"@ | Set-Content -Path .env.production.local -Encoding utf8
```
![](./media/image48.png)

24. Checks, in order: (1) Eventstream topology — every node Running; (2)
    Kusto row counts and data freshness; (3) the exact DAX query the app
    itself issues, via executeQueries.

+++ python 09_verify.py+++

A useful diagnostic pattern from 09_verify.py: if the all-time position
counter is large but the live vehicle table is empty, that means
ingestion stopped more than roughly two hours ago — it is a
pipeline/capacity issue, not an authentication issue.

![](./media/image49.png)

25. Open the Microsoft Fabric portal at
    +++https://app.fabric.microsoft.com+++.

26. Verify that the required resources have been created successfully in
    the Fabric workspace, including the **Eventstream, Eventhouse, KQL
    database, semantic model, and notebook**.

![](./media/image50.png)

# Task 6: Install dependencies and run locally

1.  In the Visual Studio Code terminal, navigate to the project root
    directory, run the npm install command to install all required
    project dependencies, and wait for the installation process to
    complete successfully.

+++cd ..\\.+++

+++npm install+++

![](./media/image51.png)

2.  In the Visual Studio Code terminal, run the npm run dev command to
    start the application in development mode.

+++npm run dev+++

3.  When prompted, enter the Fabric workspace name (Rayfin-FabricXXXXXX)
    and press Enter to continue the deployment process.

![](./media/image52.png)

![](./media/image53.png)

4.  Copy the local frontend URL shown in the terminal, which should be
    similar to **http://localhost:5173**, and open it in a new browser
    tab.

![](./media/image54.png)

5.  Select the **Sign in with Microsoft** button, sign in with the same
    Microsoft account you used for Fabric:

    - **Email**: @lab.CloudPortalCredential(User1).Username

    - **TAP**: @lab.CloudPortalCredential(User1).AccessToken

![](./media/image55.png)

![](./media/image56.png)

6.  In the Visual Studio Code terminal, run the npx rayfin up
    --workspace-id \<workspace-id\> --tenant \<tenant-id\> command to
    deploy the application to your Fabric workspace, and then verify
    that the deployment completes successfully.

   +++**npx rayfin up --workspace-id <workspace-id> --tenant <tenant-id> -y**+++

![](./media/image57.png)

![](./media/image58.png)

7.  Verify that the application deployment completed successfully, and
    then copy the published application URL from the terminal output to
    access the deployed application.

![](./media/image58.png)

8.  Go to the **Azure portal**, navigate to **Microsoft Entra ID \> App
    registrations**, select the **helsinki-public-transport-spa app**

9.  Then, under Manage, select **Authentication (Preview)**. On the
    Authentication page, select **Add Redirect URI** to configure a
    redirect URI for the application.

![](./media/image59.png)

10. In the Select a platform to add redirect URI pane, under Web
    applications, select **Single-page application** to configure the
    redirect URI for the browser-based application.

> ![](./media/image60.png)

11. In the Add Redirect URI pane, enter the **published application
    URL** in the Redirect URI field, and then select **Configure** to
    save the redirect URI settings for the single-page application.

> ![](./media/image61.png)

![](./media/image62.png)

12. In the Visual Studio Code terminal, create the .env.production.local
    file, add the required dataset ID, client ID, and tenant ID values,
    and then save the file to configure the application for production
    deployment.

```
@"
VITE_PBI_DATASET_ID=
VITE_PBI_CLIENT_ID=
VITE_PBI_TENANT_ID=
"@ | Set-Content -Path .env.production.local -Encoding utf8
```

![](./media/image63.png)

13. Rebuild and redeploy

+++**npx rayfin up --workspace-id <workspace-id> --tenant <tenant-id> -y**+++

![](./media/image64.png)

14. In the Visual Studio Code terminal output, locate the published
    application URL, press Ctrl and select the URL
    (https://tiny-fawn-f224590f38-westus2.webapp.fabricapps.net) to open
    the deployed application in your default web browser.

![](./media/image65.png)

![](./media/image66.png)

15. Select the **Sign in with Microsoft** button, sign in with the same
    Microsoft account you used for Fabric:

    - **Email**: @lab.CloudPortalCredential(User1).Username

    - **TAP**: @lab.CloudPortalCredential(User1).AccessToken

![](./media/image67.png)

![](./media/image68.png)

16. Click "**Connect live data"**

![](./media/image69.png)

17. On the Microsoft sign-in consent screen, review the requested
    permissions for the helsinki-public-transport-spa application, and
    then select Accept to grant the application access to the required
    Power BI resources.

![](./media/image70.png)

![](./media/image71.png)

![](./media/image72.png)

18. Open the Microsoft Fabric portal
    at +++https://app.fabric.microsoft.com+++.

Open the **Rayfin\_<Fabric@lab.LabInstance.Id>** workspace you created
in Task 1

19. Select App

![](./media/image73.png)

20. Click "**Connect live data"**

![](./media/image74.png)

![](./media/image72.png)

21. In the Fabric portal, expand the Rayfin-FabricXXXXX workspace, and
    then select the helsinki-public-transport app from the workspace
    navigation pane to open the deployed application and verify that it
    loads successfully.

![](./media/image75.png)

![](./media/image76.png)

# Task 7: Clean up resources

1.  Select your workspace, the **Rayfin_FabricXXXX** from the left-hand
    navigation menu. It opens the workspace item view.

&nbsp;

2.  Select the ... option under the workspace name and
    select **Workspace settings**.

![](./media/image77.png)

3.  Navigate to the bottom of the General tab and select **Remove this
    workspace**.

![](./media/image78.png)

![](./media/image79.png)

![](./media/image80.png)

**Summary**

This usecase guides users through creating a complete real-time public
transportation solution by setting up a Microsoft Fabric workspace,
deploying ingestion and analytics components, configuring security, and
publishing a Rayfin-based Fabric App. The final outcome is a fully
functional application capable of visualizing and analyzing live public
transportation data using Microsoft Fabric services
