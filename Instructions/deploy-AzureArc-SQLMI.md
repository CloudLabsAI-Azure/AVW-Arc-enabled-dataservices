# Deploy Azure Arc Enabled SQL Managed Instance

  In thia Exercise you will be deploy Azure Arc SQL Managed instance on top of Azure Arc Data COntroller with Direct mode with the help of Azure Porta.
 
 
 Task 1. Deploy Azure Arc SQL Managed instance.
 
 1. Open your browser and login to Azure portal if not already done.

1. Now Search for SQL Managed Instance -  Azure Arc and select it.

   ![](.././media/27.png "Lab Environment")
   
1. Click on create ** + Create ** button to create the SQL Managed instance - Azure Arc.

   ![](.././media/28.png "Lab Environment")
 
1. Now on **Basics** tab enter the below details:
 
 
   **Under project details**
    
    **Subscription: Leave default.
    
    **Resource Group**: Select azure-arc from drop down
    
     
   
   **Under Managed Instance details**
   
    **Instance name**: Enter arcsql
  
    **Custom location**: Select available custom location from dropdown.
   
    **Service type**: Select **Load balancer** from drop down
    
    **Compute+ Storage**: Click on **Configure compute + storage**
      
      ![](.././media/29.png "Lab Environment")
      
      
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
    
      ![](.././media/30.png "Lab Environment")
      ![](.././media/31.png "Lab Environment")
    
    After adding all the above details click on **Apply** button.
    
    **Under Administrator account** Enter the below details
    
     **Managed Instance admin login**:  Enter arcsqluser
   
     **Password**: Enter Password.1!!
     
     ** Confirm Password**: Enter Password.1!!
  
1. After adding all the required details click on **Review + Create button** to review the all details.
    
    ![](.././media/32.png "Lab Environment")
    
1  Now Click on **Create** button to start the deployment.  
 
   ![](.././media/33.png "Lab Environment")
 
1. After some time you see that the deployment of **SQL Managed Instance - Azure Arc** in completed. Now click on Go to resource button to navigate to the resource.

1. Now on the overview blade of newly deployed **SQL Managed Instance - Azure Arc**, you can explore the details of namespace and other details of data controller and Azure Arc enabled SQLMI.

    ![](.././media/34.png "Lab Environment")

  -Note: Please note that the External endpoint details can take few minutes to reflect on azure portal.
 
1. Copy the **External endpoint** and save it in a notepad, we will use it later while connecting to SQLMI using Azure data studio.

1.
