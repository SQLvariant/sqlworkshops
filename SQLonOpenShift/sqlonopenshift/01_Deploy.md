![](../graphics/microsoftlogo.png)

# Workshop: SQL Server on OpenShift

#### <i>A Microsoft workshop from the SQL Server team</i>

<p style="border-bottom: 1px solid lightgrey;"></p>

<img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/textbubble.png"> <h2>Deploy SQL Server on OpenShift</h2>

You'll cover the following topics in this Module:

<dl>

  <dt><a href="#3-0">1.0 SQL Server Deployment on OpenShift</a></dt>
  
</dl>

<p style="border-bottom: 1px solid lightgrey;"></p>

<h2><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png"><a name="3-0">1.0 SQL Server Deployment on OpenShift</a></h2>

In this module you will learn the fundamentals of how to deploy SQL Server OpenShift. The SQL Server engine can be fun in a container and in a Kubernetes cluster. In this module, you will learn the basic steps for how to deploy a SQL Server container in an OpenShift cluster. This module is required to complete the Modules 2, 3, and 4 of the workshop.

In the deployment of SQL Server on OpenShift, you will:

- Create a secret for the password to login to SQL Server
- Create a PersistentVolumeClaim where SQL Server databases and files will be stored
- Deploy a SQL Server container using a declarative .yaml file which will include a LoadBalancer to connect to SQL Server.

Proceed to the Activity to learn these deployment steps.

<p style="border-bottom: 1px solid lightgrey;"></p>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b><a name="aks">Activity: Deploy SQL Server on OpenShift</a></b></p>

Follow these steps to deploy SQL Server on OpenShift:

1. Open a shell prompt and change directories to the **SQLonOpenShift/sqlonopenshift/01_Deploy** folder.

2. Login to the OpenShift cluster with the oc.exe program using the username and password you created or was provided by your instructor.

    `oc -u <user> -p <Password>`

    **Note**: If you were provided a login for this workshop your instructor should have created a project for you to work in. If you are running this workshop yourself as a cluster administrator, be sure to create a new project for this module and use that project for Modules 2,3, and 4.

3. Create a secret to store the System Administrator (sa) password. For this workshop, you will be connecting as the sa login. In production SQL Server environment, you would not normally use the sa login. Use the following command or execute the **step1_create_secret.sh** script found in the **01_Deploy** folder:

    `oc create secret generic mssql --from-literal=SA_PASSWORD="Sql2019isfast"`

    When this completes you should see the following message and be placed back at the shell prompt

    **secret/mssql created**

4. Create a PersistentVolumeClaim to store SQL Server databases and files. Use the following command or execute the **step2_storage.sh** script found in the **01_Deploy** folder:

    `oc apply -f storage.yaml`

      When this completes you should see the following message and be placed back at the shell prompt

    **persistentvolumeclaim/mssql-data created**

5. Deploy SQL Server using the following command or the **step3_deploy_sql.sh** script found in the **01_Deploy** folder:

    `oc apply -f sqldeployment.yaml`

    When this compeletes, you should see the following messages and be placed back at the shell prompt

      **deployment.apps/mssql-deployment created
      service/mssql-service created**

    At this time, you have submitted a deployment. OpenShift will schedule a SQL Server container in a pod on a node on the cluster. Proceed to the next step to check on whether the deployment was successful.

6. Verify the SQL Server deployment has succeeded by running the following command:

    `oc get deployment mssql-deployment`

    When the value of **AVAILABLE** becomes 1, the deployment is successful including a Running Container. Depending on the load of your cluster and whether the container image of SQL Server is already present, the deployment could take several minutes.

    A successful deployment will render output like the following:

    NAME               DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    
   mssql-deployment   1         1         1            1           3m

    You can run the following command to check on the status of the pod and load balancer service

    `oc get all`

     When the deployment is successful, the STATUS of the pod is Running and the service will have a valid EXTERNAL-IP address.

A pod with a SQL Server container is now deployed and a load balancer service is attached to the pod. Proceed to the next Modeule to connect and run queries against your SQL Server deployment

<p style="border-bottom: 1px solid lightgrey;"></p>



<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/owl.png"><b>For Further Study</b></p>

- [Quickstart: Run SQL Server container images with Docker](https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker?view=sql-server-linux-ver15&pivots=cs1-bash)
- [Deploy a SQL Server container in Kubernetes with Azure Kubernetes Services (AKS)](https://docs.microsoft.com/en-us/sql/linux/tutorial-sql-server-containers-kubernetes?view=sql-server-2017)

<p style="border-bottom: 1px solid lightgrey;"></p>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/geopin.png"><b >Next Steps</b></p>

Next, Continue to <a href="02-Query.md" target="_blank"><i>Connect and Query</i></a>.