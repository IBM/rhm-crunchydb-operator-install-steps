---
#Front matter (metadata).
abstract:               # REQUIRED

authors:
 - name: "Rahul Reddy Ravipally"
   email: "raravi86@in.ibm.com"
 - name: "Manoj Jahgirdar"
   email: "manoj.jahgirdar@in.ibm.com"
 - name: "Srikanth Manne"
   email: "srikanth.manne@in.ibm.com"
 - name: "Manjula G. Hosurmath"
   email: "mhosurma@in.ibm.com"

completed_date: 2020-07-03

components:
- slug: "Crunchy PostgreSQL for Kubernetes"
  name: "Crunchy PostgreSQL for Kubernetes"
  url: "https://marketplace.redhat.com/en-us/products/crunchy-postgresql-for-kubernetes"
  type: "component"
- slug: "redhat-marketplace"
  name: "Red Hat Marketplace"
  url: "https://marketplace.redhat.com/"
  type: "component"

draft: true|false       # REQUIRED

excerpt:                # REQUIRED

keywords:               # REQUIRED - comma separated list

last_updated:           # REQUIRED - Note: date format is YYYY-MM-DD

primary_tag:          # REQUIRED - Note: Choose only only one primary tag. Multiple primary tags will result in automation failure. Additional non-primary tags can be added below.

pta:                    # REQUIRED - Note: can be only one
# For a full list of options see https://github.ibm.com/IBMCode/Definitions/blob/master/primary-technology-area.yml
# Use the "slug" value found at the link above to include it in this content.
# Example (remove the # to uncomment):
 # - "cloud, container, and infrastructure"

pwg:                    # REQUIRED - Note: can be one or many
# For a full list of options see https://github.ibm.com/IBMCode/Definitions/blob/master/portfolio-working-group.yml
# Use the "slug" value found at the link above to include it in this content.
# Example (remove the # to uncomment):
# - "containers"

related_content:        # OPTIONAL - Note: zero or more related content
  - type: announcements|articles|blogs|patterns|series|tutorials|videos
    slug:

related_links:           # OPTIONAL - Note: zero or more related links
  - title:
    url:
    description:

runtimes:               # OPTIONAL - Note: Select runtimes from the complete set of runtimes below. Do not create new runtimes. Only use runtimes specifically in use by your content.
# For a full list of options see https://github.ibm.com/IBMCode/Definitions/blob/master/runtimes.yml
# Use the "slug" value found at the link above to include it in this content.
# Example (remove the # to uncomment):
 # - "asp.net 5"

series:                 # OPTIONAL
 - type:
   slug:

services:               # OPTIONAL - Note: please select services from the complete set of services below. Do not create new services. Only use services specifically in use by your content.
# For a full list of options see https://github.ibm.com/IBMCode/Definitions/blob/master/services.yml
# Use the "slug" value found at the link above to include it in this content.
# Example (remove the # to uncomment):
# - "blockchain"

subtitle:               # REQUIRED

tags:
# Please select tags from the complete set of tags below. Do not create new tags. Only use tags specifically targeted for your content. If your content could match all tags (for example cloud, hybrid, and on-prem) then do not tag it with those tags. Less is more.
# For a full list of options see https://github.ibm.com/IBMCode/Definitions/blob/master/tags.yml
# Use the "slug" value found at the link above to include it in this content.
# Example (remove the # to uncomment):
 # - "blockchain"

title:                  # REQUIRED

translators:             # OPTIONAL - Note: can be one or more
  - name:
    email:

type: tutorial

---

# Steps to Deploy CrunchyDB Operator from Red Hat Marketplace on OpenShift Cluster

### Step 1: Configure Openshift Cluster(ROKS) with Redhat Market Place

#### Step 1.1: Download OpenShift Command Line Interface (CLI) binary

- Follow the steps below to launch the cluster console which is also called RedHat OpenShift Container Platform.

- Login to [IBM Cloud Account](https://cloud.ibm.com/) and navigate to Dashboard as shown.

![rhm](doc/source/images/dashboard.png)

- Click on **Clusters** and select the cluster which you have created under prerequisites. In our case, cluster name is **cp-rhm-poc**.

![](doc/source/images/cluster.png)

- After you launch the cluster, click on **OpenShift web console** on the top right hand side.

![](doc/source/images/web-console.png)

- We can see the RedHat OpenShift Container Platform (Web Console). Click on **question mark icon** on the top right hand side and select **Command Line Tools**. 

![](doc/source/images/cmd-line-tools.png)

- Navigate to the section `oc - OpenShift Command Line Interface (CLI)` and download the respective oc binary onto your local system. 

**NOTE: This is needed to manage OpenShift projects from a terminal and is further extended to natively support OpenShift Container Platform features.**

![](doc/source/images/oc-binary.png)

- We are all set to proceed to next step which is to register the OpenShift cluster on RedHat Marketplace platform. 

**NOTE: This is mandatory to install any operators from RedHat Marketplace platform using the OpenShift cluster**.

#### Step 1.2: Register the cluster on RedHat Marketplace

- Sign up and login to RHM portal at [Link](https://marketplace.redhat.com/en-us) and click on **workspace** and then click on cluster. We need to add our new OpenShift cluster and register it on RHM platform.

![](doc/source/images/add-cluster.png)

- Update the **cluster name**, generate the pull secret as per the instructions and save it as shown.

![](doc/source/images/cluster-details.png)

- Copy the curl command which starts with `curl -sL https` and append the pull secret towards the end. 

**NOTE: The entire script should be handy to be used in next step.**

- We need to start the cluster first to register it. Open a terminal and type `oc login`, update the `username` and `password` which are used for accessing the cluster and hit enter. 

![](doc/source/images/start-cluster.png)

- The cluster is up and running at this point. We need to run the entire script which is from previous step and hit enter. It will take a couple of mins and we can see that we have successfully registered the cluster on RHM portal.

![](doc/source/images/register-cluster.png)

#### Step 1.3: Create a project in web console

- We need to create a project to be used and managed from command line. Click on **Create Project** and give a name as `Cockroachdb-test-project`.

![rhm](doc/source/images/create-project.png)

### Step 2: Install the CrunchyDB Operator

- Navigate to **OpenShift web console** which was launched during previous step. Select **operatorhub** under Operators and type 'Crunchy' in the search bar.

![](doc/source/images/install-operator1.png)

- Click on **Crunchy Postgres Operator** (non custom) and click **install**.

![](doc/source/images/install.png)

- Create Operator Subscription by choosing All namespaces or specific namespace (select default project crunchy-project) and click **subscribe**.

![](doc/source/images/subscribe1.png)

- After a couple of minutes, the operator gets installed on the cluster. We can verify by clicking on Installed Operators under `Operators` and see that the operator is successfully installed with status showing as Succeeded.

![](doc/source/images/installed-operator.png)

### Step 3: Connect to the Openshift Cluster in CLI (Command Line Interface)

- Login to the ROKS(IBM Managed) Openshift cluster through CLI(command line Iterface). 
To login you would require token which can be genrated after you login to Openshift Cluster web console. See below screenshot to `copy the path`.
![](doc/source/images/Login-CopyCommand.png)

- A new window will open with the login token details. See below screenshot for details. Copy the login token as per the below screenshot.
![](doc/source/images/Login-Token.png)

- In terminal, paste the login command, Once you login you would see a similar screen as shown below.
![](doc/source/images/CLI-Login.png)

### Step 4: Create and Deploy CrunchyDB Operator on OpenShift Cluster and Create a database.

- (i). Use the new namespace where we have isntalled the Crunchy Postgres operator.
- (ii). Run the below command in CLI(command line Iterface). Once it runs successfully, check for the logs and be sure there are no errors in the ansible script. Wait for the pod state to change to complete state.

``oc create -f postgres-operator.yml`` 

``oc get po
NAME               READY   STATUS      RESTARTS   AGE
pgo-deploy-zl6sz   0/1     Completed   0          24h
``
- (iii). switch to pgo namespace. 

- (iv). Edit `pgo-config configmap` and update `DisableFSGroup` to `false`.

- (v). Restart postgress operator pod. postgres-operator-f7d8c5667-4hhrk

`Note:`Reason for above step (iv, v):
``Crunchy PostgreSQL for Kubernetes is set up to work with the "restricted" SCC by default, but we may need to make modifications. In this mode, we will want to ensure that "DisableFSGroup" is set to false mentioned "pgo-config" ConfigMap.
Making changes to the "pgo-config" ConfigMap, we will have to restart the "postgres-operator" Pod. ``

- (vi). Download the pgo binary mentioned in the [document URL](https://access.crunchydata.com/documentation/postgres-operator/latest/quickstart/)

- (vii). Make sure the pvc are in bound state. Run the below command: 
`oc get pvc`
![](doc/source/images/pvc.png)

- (viii). Create database using below command.
``pgo create cluster -n pgo hippo``
This will create database (pods) in pgo namespace.

- (ix). To validate run below commands.
    a .`` pgo show cluster -n pgo hippo``
    b.  ``pgo test -n pgo hippo``

Attached is the postgres-operator.yml updated file. (edited) 
[Download postgres-operator.yml](postgres-operator.yml)

### Step 5: Access the cluster on your localhost

- Let us view the results of the commands we ran in the earlier steps via the pgAdmin 4 console. The console can be accessed at localhost with port forwarding.

- Run the following command in Terminal:
```bash
$ pgo create pgadmin hippo
```
- This creates a pgAdmin 4 deployment unique to this PostgreSQL cluster and synchronizes the PostgreSQL user information into it.

- To access pgAdmin 4, you can set up a `port-forward` to the Service, which follows the pattern `<clusterName>-pgadmin`, to port `5050`:
```bash
$ kubectl port-forward -n pgo svc/hippo-pgadmin 5050:5050 
```

```
Forwarding from 127.0.0.1:5050 -> 5050
Forwarding from [::1]:5050 -> 5050
```

- Open <http://localhost:5050> on your browser and use your database username (e.g. `hippo`) and password (e.g. `datalake`) to log in.

![](doc/source/images/login-pgo.png) 

(Note: if your password does not appear to work, you can retry setting up the user with the [pgo update](https://access.crunchydata.com/documentation/postgres-operator/4.3.2/pgo-client/reference/pgo_update_user/) user command: `pgo update user -n pgo --username=hippo --password=datalake hippo`)

- Once logged in, you can see the pgAdmin 4 console as shown.

![](doc/source/images/pgadmin4.png)
