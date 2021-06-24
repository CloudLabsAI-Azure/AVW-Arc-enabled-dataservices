## Deploy Azure Database for PostgreSQL server groups - Azure Arc using Azure portal
In this exercise you will be deployming Azure Database for PostgreSQL server groups - Azure Arc on top of a direct mode of Azure Arc data controller.


Task 1: Create Azure Database for PostgreSQL server groups - Azure Arc

1. Open you browser and navigate to portal.azure.com and login with you alb credentials if not already done.

1. Click on the search box and search for postgrs and select **Azure Database for PostgreSQL server groups - Azure Arc** from the list.

   ![](.././media/36.png "Lab Environment")
   
1. After selecting the **Azure Database for PostgreSQL server groups - Azure Arc* from the list click on **Create** button.

    ![](.././media/36.png "Lab Environment")
    
1. Now on **Basic** blade of **Azure Database for PostgreSQL server groups - Azure Arc* enter the below details in the required fields:

   * Subscription: Leave default
   * Resource Group: Select azure-arc from the drop down list

**Under Managed Instance details**
   
   * **Instance name**: Enter arcpostgres
  
   * **Custom location**: Select available custom location from dropdown.
   
   * **Service type**: Select **Load balancer** from drop down
    
   * **Compute+ Storage**: Click on **Configure compute + storage**
      
      ![](.././media/37.png "Lab Environment")
      
      
   Now on **Compute+ Storage** blade enter the following details:
   
   * Number of worker nodes : Enter 2
   * Data storage class: leave default
   * data volume size (in Gi): 1
   * Data-logs storage class: leave default
   * Data-logs volume size(in Gi): 1
   * Logs storage class: Leave deault
   * Logs storage class: Enter 1
   * Backup Storage class: leave default
   * Backups volume size (in Gi): 1

   ![](.././media/38.png "Lab Environment")
   * CPU Request: ```1```
   * CPU limit: ```2```
   * Memory request (in GiB): ```2```
   * Memory limit (in GiB): ```2```

  Now click on apply to save these values.
   ![](.././media/39.png "Lab Environment")
   
  * Extension: Leave default(citus)
  
  Under administrator account.
  * Password: ```Password.1!!```
  * Confirm Password : ```Password.1!!```

1. After adding all the details click on **Review + Create** button to validate all the added details.
 
   ![](.././media/40.png "Lab Environment")

1. Now Click on **Create** button to start the deployment.  
 
   ![](.././media/41.png "Lab Environment")
