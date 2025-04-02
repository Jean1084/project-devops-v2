Kubernetes Cluster with Vagrant
===============================

Project Overview
----------------

This project sets up a Kubernetes cluster using Vagrant and VirtualBox. The cluster consists of four nodes:

1.  **Master Node**: Controls the cluster

2.  **WordPress Node**: Hosts a WordPress application

3.  **MySQL Node**: Hosts a MySQL database for WordPress

4.  **Registry Node**: Stores Docker images used by the other nodes [GitHub Pages](https://jean1084.github.io) - [GitHub Repo](https://github.com/Jean1084/Jean1084.github.io.git)

- **Resources per Node**:
  - **1 CPU**
  - **1 GB RAM**
  - **30 GB Disk Space**

Prerequisites
-------------

Ensure you have the following installed on your system:

-   Vagrant

-   VirtualBox

-   kubectl

-   Docker

Setup Instructions
------------------

1.  **Clone the Repository**

    ```
    git clone git@github.com:Jean1084/project-devops-v2.git
    cd kubernetes-vagrant
    ```

2.  **Start the VMs**

    ```
    vagrant up
    ```

3.  **Verify the Cluster** Once the setup is complete, SSH into the master node and check the cluster status:

    ```
    vagrant ssh master
    kubectl get nodes
    ```

Project Architecture
--------------------

```
+-------------------+
|   Master Node    |
|   (Kubernetes)   |
+--------+---------+
         |
         |
+--------+---------+
| WordPress Node   |
| (WordPress App)  |
+------------------+
         |
         |
+--------+---------+
|  MySQL Node      |
| (Database)       |
+------------------+
         |
         |
+--------+---------+
| Registry Node   |
| (Docker Images) |
+-----------------+
```

Configuration Details
---------------------

-   **Kubernetes** is initialized on the master node.

-   **WordPress** is deployed in a container on the WordPress node.

-   **MySQL** is deployed as a database service for WordPress.

-   **Docker Registry** is hosted on the registry node to manage container images.

Managing the Cluster
--------------------

-   To list running pods:

    ```
    kubectl get pods -A
    ```

-   To describe a specific node:

    ```
    kubectl describe node <node-name>
    ```

-   To deploy additional applications, use Kubernetes manifests.

Cleanup
-------

To destroy the cluster and remove all VMs:

```
vagrant destroy -f
```