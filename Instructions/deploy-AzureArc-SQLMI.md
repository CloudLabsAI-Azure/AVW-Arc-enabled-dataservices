# Exercise 2: Deploy Azure Arc Enabled SQL Managed Instance
  Duration: 40 Minutes
  
 In this exercise, let's create an **Azure SQL Managed Instance - Azure Arc** using Azure Portal on top of a directly connect Azure Arc Data controller and restore and migrate the database using multiple methods. 

 Also, we will be exploring the Kibana and Grafana Dashboards and upload the logs and metrics to the Azure portal and view the logs.
 
 
 Task 1. Deploy Azure Arc SQL Managed instance.
 
 1. Open your browser and login to Azure portal if not already done.

1. Now Search for SQL Managed Instance -  Azure Arc and select it.

   ![](./media/27.png "Lab Environment")
   
1. Click on create ** + Create ** button to create the SQL Managed instance - Azure Arc.

   ![](./media/28.png "Lab Environment")
 
1. Now on **Basics** tab enter the below details:
 
 
   **Under project details**
    
    **Subscription**: Leave default.
    
    **Resource Group**: Select azure-arc from drop down
    
     
   
   **Under Managed Instance details**
   
    **Instance name**: Enter arcsql
  
    **Custom location**: Select available custom location from dropdown.
   
    **Service type**: Select **Load balancer** from drop down
    
    **Compute+ Storage**: Click on **Configure compute + storage**
      
      ![](./media/29.png "Lab Environment")
      
      
    Now on     **Compute+ Storage** blade enter the following details:
    
     * High availability : Select 1 replica
     * Memory Limit(in Gi): Enter 4
     * CPU Limit: Enter 2
     * Data storage class: leave default
     * data volume size (in Gi): 2
     * Data-logs storage class: leave default
     * Data-logs volume size(in Gi): 1
     * Logs storage class: Leave deault
     * Logs storage class: Enter 1
     * Backup Storage class: leave default
     * Backups volume size (in Gi): 1
    
      ![](./media/30.png "Lab Environment")
      ![](./media/31.png "Lab Environment")
    
    After adding all the above details click on **Apply** button.
    
    **Under Administrator account** Enter the below details
    
     **Managed Instance admin login**:  Enter arcsqluser
   
     **Password**: Enter Password.1!!
     
     **Confirm Password**: Enter Password.1!!
  
1. After adding all the required details click on **Review + Create button** to review the all details.
    
    ![](./media/32.png "Lab Environment")
    
1  Now Click on **Create** button to start the deployment.  
 
   ![](./media/33.png "Lab Environment")
 
1. After some time you see that the deployment of **SQL Managed Instance - Azure Arc** in completed. Now click on Go to resource button to navigate to the resource.

 ## validate the **Azure SQL Managed Instance - Azure Arc** is deployed.
 
  ```
  azdata arc sql mi list
  ```
  
  ![](./media/44.png "Lab Environment")
  
  > Note: If the state is showing creating then please run the above command after some time and check if the state is changed to ready or not.This can take upto few minutes to change the state to ready.

1. Now switch back to azure portak and on the overview blade of newly deployed **SQL Managed Instance - Azure Arc**, you can explore the details of namespace and other details of data controller and Azure Arc enabled SQLMI.

    ![](./media/34.png "Lab Environment")

    > Note: Please note that the External endpoint details can take few minutes to reflect on azure portal.
 
1. Copy the **External endpoint** and save it in a notepad, we will use it later while connecting to SQLMI using Azure data studio.

   ![](./media/34.png "Lab Environment")



## Task 2: Connect to Azure Arc enabled Azure SQL Managed Instance using Azure Data Studio.

In this task, let us learn how to connect to your newly created Azure Arc enabled Azure SQL Managed instance using Azure Data Studio.

1. Open Azure data studio from the VM desktop.

1. In the Azure Data Studio page, in connections blade under servers click on **New Connection**.

   ![](images/sql-instance4.png "Confirm")

1. Enter the following in the connection details page and click on **Connect**.

   - **Connection type** : Select **Microsoft SQL Server**.
   
   - **Sever**: Paste the external endpoint value which you copied earlier from Azure portal

   >**Note**: Make sure you have entered **IP Address** only and remove **port number**.
   
   - **Authentication type** : Select **SQL Login** from the drop down options
   
   - **User name** : Enter arcsqluser
     ```BASH
     arcsqluser
     ```
   
   - **Password** : Enter Password.1!!
     ```BASH
     Password.1!!
     ```
   
   ![](images/sqlconnect.png "Confirm")
   
1. Now you can see that you are successfully connected with your Azure Arc enabled SQL MI Server. Under servers you can see that you are successfully connected with your Azure Arc enabled SQL MI Server. You can explore the SQLMI dashboard to view the databases and run a query.

   ![](images/sqlmiserver.png "Confirm")

## Task 3: Configure Azure Arc enabled Azure SQL Managed Instance

In this task, let's learn to edit the configuration of Azure Arc enabled Azure SQL Managed instances with Azure Data CLI.

1. Open Command Prompt from the desktop shortcut and run the following command to see configuration options of Azure SQL Managed instance.

   ```BASH
   azdata arc sql mi edit --help
   ```

   ![](images/ex4t3-1.png "Confirm")

1. Now run the following command to set the custom CPU core and memory requests and limit. 

   >**Note**: Make sure to replace <NAME_OF_SQL_MI> with your Azure SQL Managed instance name which will be **arcsql** if you also provided the same for Instance name during creation of Azure SQL Managed Instance. Also, you shouldn't select the Core and memory limit more than the given limits.

   ```BASH
   azdata arc sql mi edit --cores-limit 3 --cores-request 2 --memory-limit 2Gi --memory-request 2Gi -n <NAME_OF_SQL_MI>
   ```      

   ![](images/ex4t3-2.png "Confirm")

1. Now, you can run the below command to view the changes that you made to the Azure SQL Managed instance.

   >**Note**: Make sure to replace <NAME_OF_SQL_MI> with your Azure SQL Managed instance name which will be **arcsql** if you also provided the same for Instance name during creation of Azure SQL Managed Instance.
   
   ```BASH
   azdata arc sql mi show -n <NAME_OF_SQL_MI>
   ```

   ![](images/ex4t3-3.png "Confirm")

## Task 4: Restore the AdventureWorks sample database into Azure SQL Managed instance - Azure Arc Using Kubectl.

Migrating an existing SQL database from a SQL Server to Azure Arc enabled SQL MI is very simple. All you have to do is to take a backup from your existing SQL Server, and then restore that backup to SQL MI.

Now let's restore the sample backup file i.e AdventureWorks backup (.bak) into your Azure SQL Managed instance container using Kubectl commands.

1. Launch a **Command Prompt** window from the desktop of your JumpVM if you have already closed the existing one.

1. In the Command Prompt, run the following command to get the list of pods that are running on your data controller. 

   > **Note**: Please make sure to replace the namespace name with your data controller namespace name which will be **arcdc**.

   ```BASH
   kubectl get pods -n <your namespace name>
   ```
   
1. From the output of the above command, copy the pod name of the SQL MI instance from the output which will be in following format sqlinstancename-0. If you followed the same naming convention as in the instructions, the pod name will be **arcsql-0**.

   > **Note**: Please copy the Pod Name for the next step.

   ![](images/ex4t4-2.png "Confirm")
   
1. In the Command Prompt, run the following command after replacing the required values. This will remotely execute a command in the Azure SQL Managed instance container to download the .bak file onto the container.

   >**Note**: Replace the value of the namespace name and pod name with the value you copied in the previous step.

   ```BASH
   kubectl exec <SQL pod name> -n <your namespace name> -c arc-sqlmi -- wget https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2019.bak -O /var/opt/mssql/data/AdventureWorks2019.bak
   ```

   ![](images/ex4t4-3.png "Confirm")

1. Now, to restore the AdventureWorks database, you can run the following command.

   > **Note**: Ensure to replace the value of the pod and the namespace name before you run it. Rest of the values we have already added in the command, you can go through the command and figure out what all are being passed as arguements.

   ```BASH
   kubectl exec <SQL pod name> -n <your namespace name> -c arc-sqlmi -- /opt/mssql-tools/bin/sqlcmd -S localhost -U arcsqluser -P Password.1!! -Q "RESTORE DATABASE AdventureWorks2019 FROM  DISK = N'/var/opt/mssql/data/AdventureWorks2019.bak' WITH MOVE 'AdventureWorks2017' TO '/var/opt/mssql/data/AdventureWorks2019.mdf', MOVE 'AdventureWorks2017_Log' TO '/var/opt/mssql/data/AdventureWorks2019_Log.ldf'"
   ```

   ![](images/ex4t4-4.png "Confirm")

1. Now you can switch back to Azure Data Studio, and then right-click on the SQL MI Server under CONNECTIONS on the top left of the Azure Data Studio and click on refresh

1. Then, expand your SQL MI server if not already by clicking on the arrow icon on the left of the IP Address, then expand Databases and verify that AdventureWorks2019 Database is listed there.

   ![](images/ex4t4-6.png "Confirm")

## Task 5: Migrate and Restore SQL Server DB to Azure Arc enabled Azure SQL Managed instance from Blob storage and Azure arc pod.

Now that we have the Azure SQL Managed instance ready, let's migrate and restore the SQL server database to the Azure arc enabled SQLMI. 
   
There are two methods to do the migration and restore - One is by using the Azure blob storage to back up and restore DB to SQLMI, and the Second one is by using the kubectl commands. 

 **Method 1: Using Azure blob storage** 
 
 **Step 1: Migrate: SQL Server to Azure Arc enabled Azure SQL Managed instance**

1. Navigate to Azure portal - https://portal.azure.com and login if you haven't already.

1. From the home page, select Resource groups under Navigate.

   ![](images/ex4t5-2.png "Confirm")

1. From the next page, open the resource group you have access to. You will be able to find a resource of type Azure blob storage with name as arcstorageUNIQUEID. Click on that resource. 

   ![](images/ex4t5-s1-2.gif "Confirm")

1. You will be directed to the overview blade of the resource. Then, click on the **Shared access signature** from the left side bar. Now click on the **Generate SAS and connection string** button to get the keys.

   > **Note**: Before generating SAS token make sure all the three check boxes(Service, Container, Object) under **Allow resource types** are checked.

   ![](images/Blob_SAS_generate.PNG "Confirm")
   
   > **Info**: **Shared Access Signature** makes sure that the database backups are only available for authorized users. Since we are talking about security, could be a good idea to also say that backups can also be encrypted if needed.

   ![](images/ex4t5-s1-3.gif "Confirm")

1. Now you will be able to see the storage account name at the top and Shared access signature token. Copy the Storage account name and SAS token from the portal and save it in a **notepad** for later usage. Then, click on Overview from the left tab and then select **Containers** and copy the container name onto the notepad for later use.

   ![](images/Blob_SAS.PNG "Confirm")

   > **Note**: After copying SAS token to notepad remove **?** at the beginning of the token before useing it later.

   ![](images/ex4t5-s1-4.gif "Confirm")

1. Launch Azure Data Studio and double click on the **localhost** to connect with the local server. After connecting, click on the databases folder and right-click on **AdventureWorks2019** Database, and select **New Query**.

   ![](images/sqlquery.png "Confirm")

1. Prepare your query in the following format replacing the placeholders indicated by the <...> using the information from the Storage account name and SAS token in earlier steps. 

   Once you have substituted the values, run the query. 

   ```BASH
   CREATE CREDENTIAL [https://<storage-account-name>.blob.core.windows.net/<storage-container-name>] WITH IDENTITY = 'Shared Access Signature' 
   ,SECRET = '< replace with sas token >'
   ```

   Once you see that the Command is executed successfully go to the next step.
          
   ![](images/SQL_q1.PNG "Confirm")

1. Similarly, prepare the BACKUP DATABASE command as follows to create a backup file to the blob container. 

   Once you have substituted the values, run the query. Make sure you replace the values of **Database name**, **Storage account name**, **Container name**, and **Filename**.

   ``` 
   BACKUP DATABASE <database name> TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<filename>.bak'; 
   ```

   Once you see that the command is executed successfully, you can go to the next step.

   ![](images/SQL_q2.PNG "Confirm")
   
1. Open the Azure portal and validate that the backup file created in the previous step is visible in the Blob container. For this, you have to go to the storage account and click on the container button and then open the arccontainer to view the backup file.
  
   ![](images/sqlquery2.png "Confirm")

**Step 2: Restore the database from Azure blob storage to Azure SQL Managed instance - Azure Arc**

1. Navigate back to Azure Data Studio, login and connect to the Azure SQL Managed instance - Azure Arc if not already .

1. Expand the System Databases, right-click on the master database, and select New Query.

1. In the query editor window, prepare and run the below query by replacing with the required values to create the credentials.

   ```
   CREATE CREDENTIAL [https://<storage-account-name>.blob.core.windows.net/<storage-container-name>] WITH IDENTITY = 'Shared Access Signature' 
   ,SECRET = '< SAS >'
   ```
 
1. Prepare and run the below command to verify the backup file is readable and intact. Make a note of logical names from the output windows.

   ```
   RESTORE FILELISTONLY FROM URL = 'https://<Storage account name>.blob.core.windows.net/<Container name>/<File Name>.bak'
   ```

1. Prepare and run the RESTORE DATABASE command as follows to restore the backup file to a database on Azure SQL Managed instance - Azure Arc. Make sure you update the test and test_log value to the logical values from the previous step.    

   ```
   RESTORE DATABASE <database name> FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<file name>'
   WITH MOVE '<file name>' to '/var/opt/mssql/data/<file name>.mdf'
   ,MOVE '<file name>_log' to '/var/opt/mssql/data/<file name>.ldf'
   ,RECOVERY  
   ,REPLACE  
   ,STATS = 5;  
   GO
   ```
   
   ![](images/SQL_q3.PNG "Confirm")
    
**Method 2: Copy the backup file into an Azure SQL Managed instance - Azure Arc pod using kubectl**
   
This method shows you how to take a backup file that you create via any method and then copy it into local storage in the Azure SQL Managed instance pod.
   
So you can restore from there much like you would on a typical file system on Windows or Linux.
   
In this scenario, you will be using the command kubectl cp to copy the file from one place into the pod's file system.
   
1. Connect to the SQL MI using Azure Data Studio and right-click on the database and click on the new query and paste the below query.
   
   ```
   BACKUP DATABASE AdventureWorks2019
   TO DISK = 'c:\tmp\<DB name>.bak'
   WITH FORMAT, MEDIANAME = 'AdventureWorks2019’ ;
   GO
   ```
    
2. Open CMD and run the below command to get the SQLMI pod name.
   
   ```
   kubectl get pods -n <namespace of data controller>
   ```

3. Copy the backup file from the local storage to the SQL pod in the cluster using the below SQL query. Make sure you replace the SQLMI pod name and DB name.
   
   ```
   kubectl cp C:\temp\<DB name>.bak <SQLMI pod name>:var/opt/mssql/data/<DB Name>.bak -n arc
   ```

4. Run the below query to restore the database to the Azure SQL Managed instance - Azure arc
     
   ```
   RESTORE DATABASE test FROM DISK = '/var/opt/mssql/data/<file name>.bak'
   WITH MOVE '<database name>' to '/var/opt/mssql/data/<file name>.mdf'  
   ,MOVE '<database name>' to '/var/opt/mssql/data/<file name>_log.ldf'  
   ,RECOVERY  
   ,REPLACE  
   ,STATS = 5;  
   GO
   ```

5. Now you can see that the database is created.
   
## Task 6: View SQL Mi logs in Azure portal.
   
Now that we have the database created, let us view some metrics, usages, and logs in the Azure portal.
   
1. Switch back to azure portal and navigate to your Azure arc enabled SQLMI.

1. Now select the logs from Left side menu.

    ![](media/45.png "Confirm")

1. if you see any queries window, click on the close button from right corner.

    ![](media/46.png "Confirm")
     
1. Now click on **Select scope** button to select the log analytics workspace that is connect with Azure Arc data controller to get the logs.

    ![](media/47.png "Confirm")
    
1. Select your log analytics workspace with the name of **logazure-arc** and click on **Apply** button to select the scope.
    
    ![](media/47.png "Confirm")
   
1. In the logs page, expand Custom Logs at the bottom of the list of tables and you will see a table called **sqlManagedInstances#####**.
   
   ![](images/workspace3.png "Confirm")

1. Double click on the table name to generate the query in query editor and click on Run button to execute the query to get the logs.
   
   ![](media/49.png "Confirm")


## Task 7: Monitor with Azure Data Studio

Now let us Monitor the SQL MI status using Grafana and Kibana.
  
1. Open Azure Data studio on the JumpVM provided and right-click on the SQL MI resource under the Azure Arc controller and click on manage.
  
1. Now copy the endpoint for Kibana dashboard and browser this endpoint in a browser.
  
1. Enter below user name and password for SQLMI.
  
   > **Note** You have to enter the credentials of Azure Arc data controller.
  
   - **User name** : arcuser
     ```BASH
     arcuser
     ```

   - **Password** : Password.1!!
     ```BASH
     Password.1!!
     ```

   ![](images/sql-mon-kibana-login.png "")
   
   ![](images/sql-mon-kibana.png "")

1. You can explore the page for kibana. 
  
   > ***Info***: You can learn more about kibana here: [View logs and metrics using Kibana and Grafana](https://docs.microsoft.com/en-us/azure/azure-arc/data/monitor-grafana-kibana)
    
## View the Visualization and metric using grafana graph
  
1. Now go back to the Azure Data Studio which you had opened earlier on the provided JumpVM on the left.
  
1. Now copy the endpoint for the Grafana dashboard and browser this endpoint in a browser.
  
1. Enter below user name and password for SQLMI.
  
   > **Note** You have to enter the credentials of the Azure Arc data controller.
      
   - **User name** : arcuser
     ```BASH
     arcuser
     ```

   - **Password** : Password.1!!
     ```BASH
     Password.1!!
     ```

   ![](images/sql-mon-grafana.png "")
   
1. You can explore the page for Grafana. 
  
   > ***Info***:  You can learn more about Grafana here: [View logs and metrics using Kibana and Grafana](https://docs.microsoft.com/en-us/azure/azure-arc/data/monitor-grafana-kibana)  
  

## After this exercise, you have performed the following

   - Created Azure SQL Managed instance.
   - Connected to Azure Arc enabled Azure SQL Managed instance.
   - Configured Azure Arc enabled SQL Managed Instance.
   - Restored the AdventureWorks sample database into Azure SQL Managed instance - Azure Arc.
   - Migrated and Restored SQL Server DB to Azure Arc enabled Azure SQL Managed instance from Blob storage and Azure arc pod.
   - Viewed SQL MI resources and logs in Azure portal.
   - Monitored with kibana and grafana.
