.. title:: Nutanix Files Bootcamp


.. toctree::
  :maxdepth: 2
  :caption: Deployment Labs
  :name: _deployment_labs
  :hidden:

  files_deploy/files_deploy
  file_analytics_deploy/file_analytics_deploy

.. toctree::
  :maxdepth: 2
  :caption:  Files Labs
  :name: _files_labs
  :hidden:

  files_smb_share/files_smb_share
  files_nfs_export/files_nfs_export
  files_file_blocking/files_file_blocking
  files_multiprotocol/files_multiprotocol

.. toctree::
  :maxdepth: 2
  :caption:  File Analytics Labs
  :name: _file_analytics_labs
  :hidden:

  file_analytics_scan/file_analytics_scan
  file_analytics_anomaly/file_analytics_anomaly

.. toctree::
   :maxdepth: 2
   :caption: Objects Labs
   :name: _objects_labs
   :hidden:

   objects_buckets_users_access_control/objects_buckets_users_access_control
   objects_versioning_access_control/objects_versioning_access_control
   objects_cli_scripts/objects_cli_scripts


.. toctree::
  :maxdepth: 2
  :caption: Appendix
  :name: _appendix
  :hidden:

  appendix/glossary
  tools_vms/windows_tools_vm
  tools_vms/linux_tools_vm

.. _getting_started:

---------------
Getting Started
---------------

Welcome to the Nutanix Consolidated Storage Bootcamp!

This workbook accompanies an instructor-led session that introduces Nutanix Files, File Analytics, Objects, and many common management tasks. Each section has a lesson and an exercise to give you hands-on practice. The instructor explains the exercises and answers any additional questions that you may have.

Traditionally, file storage has been yet another silo within IT, introducing unnecessary complexity and suffering from the same issues of scale and lack of continuous innovation seen in SAN storage. Nutanix believes there is no room for silos in the Enterprise Cloud. By approaching file storage as an app, running in software on top of a proven HCI core, Nutanix Files  delivers high performance, scalability, and rapid innovation through One Click management.

**In this lab you will step through managaging SMB shares and NFS exports, scale out the environment, and explore upcoming Files features. The lab will provide key considerations around deployment, configuration, and use cases.**

What's New
++++++++++

- Workshop updated for the following software versions:
    - AOS 5.15.x | 5.16.x | 5.17.x | 5.18.x
    - Prism 2020.9
    - Files 3.7.1
    - File Analytics 2.2

- Optional Lab Updates:

Agenda
++++++

- Deployment Labs
    - Files: Deploy
    - File Analytics: Enable

- Nutanix Files Labs
    - Files: Create SMB Share
    - Files: Create NFS Export
    - Files: Selective File Blocking

- Nutanix File Analytics Labs
    - File Analytics: Review Initial Scan
    - File Analytics: Anomaly Rules

- Nutanix Objects Labs
    - Objects: Creating Bukets & Users
    - Objects: Versioning & Access Controls
    - Objects: Using From CLI & Scripts



Initial Setup
+++++++++++++

- Take note of the *Passwords* being used.
- Log into your virtual desktops (connection info below)

Environment Details
+++++++++++++++++++

Nutanix Workshops are intended to be run in the Nutanix Hosted POC environment. Your cluster will be provisioned with all necessary images, networks, and VMs required to complete the exercises.

Networking
..........

Three/Four node HPOC clusters
-----------------------------

Three or four node Hosted POC clusters follow a standard naming convention:

- **Cluster Name** - POC\ *XYZ*
- **Subnet** - 10.\ **42**\ .\ *XYZ*\ .0
- **Cluster IP** - 10.\ **42**\ .\ *XYZ*\ .37

For example:

- **Cluster Name** - POC055
- **Subnet** - 10.42.55.0
- **Cluster IP** - 10.42.55.37 for the VIP of the Cluster


Throughout the Workshop there are multiple instances where you will need to substitute *XYZ* with the correct octet for your subnet, for example:

.. list-table::
  :widths: 25 75
  :header-rows: 1

  * - IP Address
    - Description
  * - 10.42.\ *XYZ*\ .37
    - Nutanix Cluster Virtual IP
  * - 10.42.\ *XYZ*\ .39
    - **PC** VM IP, Prism Central
  * - 10.42.\ *XYZ*\ .41
    - **DC** VM IP, NTNXLAB.local Domain Controller


Credentials
...........

.. note::

  The *<Cluster Password>* is unique to each cluster and will be provided by the leader of the Workshop.

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Credential
     - Username
     - Password
   * - Prism Element
     - admin
     - *<Cluster Password>*
   * - Prism Central
     - admin
     - *<Cluster Password>*
   * - Controller VM
     - nutanix
     - *<Cluster Password>*
   * - Prism Central VM
     - nutanix
     - *<Cluster Password>*

Each cluster has a dedicated domain controller VM, **DC**, responsible for providing AD services for the **NTNXLAB.local** domain. The domain is populated with the following Users and Groups:

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Group
     - Username(s)
     - Password
   * - Administrators
     - Administrator
     - nutanix/4u
   * - SSP Admins
     - adminuser01-adminuser25
     - nutanix/4u
   * - SSP Developers
     - devuser01-devuser25
     - nutanix/4u
   * - SSP Consumers
     - consumer01-consumer25
     - nutanix/4u
   * - SSP Operators
     - operator01-operator25
     - nutanix/4u
   * - SSP Custom
     - custom01-custom25
     - nutanix/4u
   * - Bootcamp Users
     - user01-user25
     - nutanix/4u

Access Instructions
+++++++++++++++++++

The Nutanix Hosted POC environment can be accessed a number of different ways:

Lab Access User Credentials
...........................

PHX Based Clusters:
**Username:** PHX-POCxxx-User01 (up to PHX-POCxxx-User20), **Password:** *<Provided by Instructor>*

RTP Based Clusters:
**Username:** RTP-POCxxx-User01 (up to RTP-POCxxx-User20), **Password:** *<Provided by Instructor>*

Frame VDI
.........

Login to: https://console.nutanix.com/x/labs

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials


Employee Pulse Secure VPN
..........................

Download the client:

PHX Based Clusters Login to: https://xld-uswest1.nutanix.com

RTP Based Clusters Login to: https://xld-useast1.nutanix.com

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

Install the client.

In Pulse Secure Client, **Add** a connection:

For PHX:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - PHX
- **Server URL** - xlv-uswest1.nutanix.com

For RTP:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - RTP
- **Server URL** - xlv-useast1.nutanix.com


Nutanix Version Info
++++++++++++++++++++

- **AHV Version** - AHV 20170830.337 (AOS 5.11+)
- **AOS Version** - 5.15.x | 5.16.x | 5.17.x | 5.18.x
- **PC Version** - Prism 2020.9
