# Deploy Azure Arc Data Controller 
 In this Exercise you will Connect the AKS to Azure Arc Kuberenetes cluster and after you will be deploying Azure Arc Data Controller and cutom location with the help of Azure Portal and Az CLI on top of Azure Arc kubernetes cluster.
 
 
## Exercise 1: Connect Existing Azure Kuberenets Cluset to Azure Arc Kuberenetes cluster.
 
 # Task 1: Login to Azure and install Azure CLI extensions.
 
1. Open **Windows Powershell** from the desktop of your ARCHOST VM and run the below command to login to Azure.
    ```
    az login
    ```
1. After running the above command a browser tab will open to login to azure portal.
 
1. On the **Sign into Microsoft Azure** tab you will see the login screen, in that enter following **Email/Username** and then click on **Next**. 
   * Email/Username: <inject key="AzureAdUserEmail"></inject>

1. Now enter the following **Password** and click on **Sign in**.
   * Password: <inject key="AzureAdUserPassword"></inject>

1. After adding the creds you will see that you have logged into the Microsoft Azure.

    ![](.././media/4.png "Lab Environment")

1. Now switch back to Windows Powershell and you will be able to see that you have logged in to Azure.

1. Now run the below commands to install required Azure CLI extensions.
   
   ```
   az extension add --name connectedk8s
   az extension add --name k8s-extension
   az extension add --name customlocation  
   
   ```
   
    ![](.././media/5.png "Lab Environment")
1. If you've previously installed the connectedk8s, k8s-extension, and customlocation extensions, update to the latest version using the following command:

   ```
   az extension update --name connectedk8s
   az extension update --name k8s-extension
   az extension update --name customlocation
   ```
   
1. You can validate you have all the required extensions with the latest versions by running the below command: 
   
   ```
   az version
   ```
   
1. After making sure the required tools are installed, next step is to register your subscription with Arc for kubernetes.


1. Run the below command to register the required Resource providers if not already registered. 
   ```
   az provider register --namespace Microsoft.Kubernetes
   az provider register --namespace Microsoft.KubernetesConfiguration
   az provider register --namespace Microsoft.ExtendedLocation
   ```

Task 2: Connect Azure Kubernetes cluster to Azure Arc kuberenets cluster and enable features on top op Azure Arc CLuster.

In this Task you will be Connecting the existing AKS to Azure Arc kubernetes cluster and will be enabling custom features to create the extension and custom location on Azure Arc kubernetes cluster.

1. Run the below command to Connect the existing AKS to Azure Arc kubernetes cluster. Once you run the below command, it will take few minutes to Connect the AKS to Azure Arc.

   ```
   az connectedk8s connect --name Arc-Data-Demo --resource-group azure-arc
   
   ```
  
   -Note: We hav already defined  your Cluster name and Azure resouce group name in the above commands, If you are trying this in your subscription please make sure that you have entered correct details.
   
 1. Once the previous command is executed successfully, the provisioning state in output will show as succeeded.

     ![](.././media/6.png "Lab Environment")
   
 1. Now let us verify if the Kubernetes cluster is connected to Azure Arc and is in a healthy state.

 1. Verify whether the Azure Arc Kubernets cluster is connected by running the following command:

    ```
    az connectedk8s list -g azure-arc -o table
    
    ```
    ![](.././media/7.png "Lab Environment")
    
 1. Azure Arc enabled Kubernetes deploys a few operators into the azure-arc namespace. You can view these deployments and pods by running the command in the command prompt:  

     ```
     kubectl -n azure-arc get deployments,pods
     
     ```
    The output should be similar as shown below:
    
    ![](.././media/8.png "Lab Environment")
    
    
 1. Navigate to the Resource Group from the Azure portal navigation pane and click on the Resource Group named azure-arc. Look for the resource named **Arc-Data-Demo** of resource type Azure Arc enabled Kubernetes resource.

     ![](.././media/9.png "Lab Environment")
        
 # Create custom location of Arc Cluster. 
 
 1. Now run the below command to enable features to create the custom location:

    ```
    az connectedk8s enable-features -n Arc-Data-Demo -g azure-arc --features cluster-connect custom-locations
    
    ```
    The output should be similar as shown below:
    
    ![](.././media/10.png "Lab Environment")
    
    
    -Note: Custom Locations feature is dependent on the Cluster Connect feature. So both features have to be enabled for custom locations to work.Also az connectedk8s enable-features needs to be run on a machine where the kubeconfig file is pointing to the cluster on which the features are to be enabled.
    
 1. Now run the below command to deploy the extenstion of Azure Arc enabled Data Services on Azure Arc kubernetes cluster.
  
     ```
     az k8s-extension create --name azdata --extension-type microsoft.arcdataservices --cluster-type connectedClusters -c Arc-Data-Demo -g azure-arc --scope cluster --release-namespace arc --config Microsoft.CustomLocation.ServiceAccount=sa-bootstrapper
     
     ```
 1. After running the above command you will notive that the **Installstate** in still pending, this is because the extension will take a few minutes to complete the installation.

    ![](.././media/11.png "Lab Environment")

 1. To verify the extension installation, Switch back to browser and search for **Kuberenetes - Azure Arc** and select your cluster.

 1. Now select **Extension (preview)** from left side menu and check if the Install status in **Installed** or not, if not please refresh after sometime and then check.

    ![](.././media/12.png "Lab Environment")

 
    
