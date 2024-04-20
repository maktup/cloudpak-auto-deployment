## CLOUDPAK-AUTO-DEPLOYMENT

This document covers the proposal of a **STANDARD** tool for executing and installing **CLOUD PAKs** on **OPENSHIFT4** in a more automated way (Scripts):

![alt text](https://github.com/maktup/cloudpak-auto-deployment/blob/master/IMAGE/ResultadoEjecucion.jpg?raw=true)


## 'VALIDATION: PREREQUISITES':
It is necessary to have the client tool: OC (Openshift Client) already installed & running in the REMOTE execution environments. If you do NOT have it, you must install it from the LINK.


## 'INSTALLATION: BASE SOURCES':
Download from SOURCE REPOSITORY (GITHUB): https://github.com/maktup/cloudpak-auto-deployment.git
 
    $ git clone https://github.com/maktup/cloudpak-auto-deployment.git
    $ cd cloudpak-auto-deployment
    $ ls -ltr
    $ cd tool / cp4i
    $ ls -ltr
    

## 'CONFIGURATION: DIRECTORIES & SCRIPTs':
The proposed **TOOLS** standard will handle a series of **DIRECTORIES** & **FILES** to **CONFIGURE**, which are as follows:

**LEVEL 1:**
Here the directories of the different **CLOUD-PAKs** will be located, managed as **TOOLs**:

- cp4i
- cp4ba
- cp4s
- cp4mcm
- cp4waio


Currently, only **CP4I** is the one that is enabled & working, but the **STRUCTURE** of directories & execution mode must be the same.

**LEVEL 2:**
The standard DIRECTORIES managed by the TOOL, here will be 3:

    A. Log.
    B. properties.
    C. scripts.


**A. "LOG" DIRECTORY:**
In this directory, the **LOGS** of the **TOOL** execution will be **AUTO GENERATED**, including the trace of the **YAMLs** executed one after the other.
The file will handle a standard autogeneration **PATTERN** which is:

    CYCLE CLOUD PAK + _ + YYYYMMDD + .log


**B. "PROPERTIES" DIRECTORY:**
This will be the only CONFIGURATION FILE that the TOOL will handle, since the objective is NOT to impact the SCRIPT YAMLs that will work only as TEMPLATES.
The file will handle a standard autogeneration PATTERN which is:

    CLOUD PAK + CYCLE .properties

In this case, for the **CP4I** scenario it will be:

| NAME |  DETAIL  |
----------- | ------------ |
| namespace_name | ame of the Namespace that the Cloud Pak will hold. |
| channel_version | Version of the Channel used by the Cloud Pak. |
| platform_navigator_name | Name of the Platform-Navigator. |
| platform_navigator_version | Version of the Platform-Navigator. |
| platform_navigator_license | License assigned for the Platform-Navigator. |
| platform_navigator_storage | Storage allocated for Platform-Navigator. |
| platform_navigator_replicas | Number of Platform-Navigator Replicas. |
| operator_name | Name of the Operator Used. |
| entitlement_key_token | Token generated from Entitlement-Key.  |

**C. DIRECTORY "SCRIPTs":**
The purpose of this **DIRECTORY** is to store the **YAMLs** & **OC** Scripts, which will be used to create **RESOURCES** on **OPENSHIFT**.

It is important to know that these Scripts should NOT be manipulated, since they will only serve in **TEMPLATE** mode & their contents will be **REPLACED** in **RUNTIME**.

For example, in this case in **FILE TEMPLATE**: **6_platform-navigator.yml**, everything shown in **UPPER CASE****** will be automatically **REPLACED** in **RUNTIME**, by its equivalent, with exactly the same name in** LOWER CASE**, which can be found in the file: **Properties / CP4I.properties**.
Then, within the main **DIRECTORY** located at: **\ cloudpak-auto-deployment \ tool \ < CICLA_CLOUD-PAK >**, you will find the **MAIN FILE **for execution, this must be the only one that will be executed at the time.
In this case: **Main.sh**


Then, it can be seen that in the content of the **MAIN FILE**: **Main.sh**, the **VALIDATIONS** of the entire **TOOL** execution will be located, as well as the respective **REPLACES**.
For example, in this case the **STATEMENT**:    

    sed "s / NAMESPACE_NAME / $ {namespace_name} /"

It performs the **REPLACE** in RUNTIME, in the **YAMEL** file of everything it finds with the string: **NAMESPACE** for the content of the **ENVIRONMENT VARIABLE**: **$ {namespace_name}** that has been loaded at the beginning by the **CONFIGURATION FILE .properties**.


**EXECUTION: TOOL**
Then, from a **LINUX** instance, connect to **OPENSHIFT** remotely:
  
    $ oc $ login --token = sha256 ~ c0eD6-kKG7vIQuyVU11bdns86TPyjACKCYZ_lMR-D_8 --server = https: //c106-e.us-south.containers.cloud.ibm.com: 31222
    
    $ oc get nodes -o wide    
    Then, give the execution permissions to the FILES before executing:
    $ cd tool    
    $ ls    
    $ cd cp4i    
    $ ls    
    $ chmod 777 *. *

Then, we proceed to execute the main **SCRIPT** of the **CP4I** installation:

    $ sh ./Main.sh


**IMPORTANT:** in the **PICTURE** you can see that the predefined **STEPS** of the **CP4I** installation are being executed & then when the installation of the **PLATFORM-NAVIGATOR INSTANCE** is finished.

The interesting thing is that at the end of the **INSTALLATION** of the entire **SCRIPT**, the **URL** & the **USER / PASSWORD** to access the **PLATFORM-NAVIGATOR** will be displayed.
Then, when entering **OPENSHIFT**, the **CP4I** & all **OPERATORS** of their **CAPACITIES** are displayed, successfully installed.
Finally, you must validate & get access to the **URL** of the **PLATFORM-NAVIGATOR**.

