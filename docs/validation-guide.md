# Validation Guide

This guide provides steps to validate the OpenStack deployment after running the Ansible playbook.

---

## 1. Access Horizon
- Open a browser and navigate to: `http://<EC2_PUBLIC_IP>/`

- Log in with:
- **Username**: `admin`
- **Password**: The value of `KEYSTONE_ADMIN_PASSWORD`.

---

## 2. Verify OpenStack Services
1. Source the `clouds.yaml` file:
     ```bash
    export OS_CLOUD=openstack
    ```

2. List all OpenStack services:
    ```bash
    openstack service list
    ```

3. Create a Test Instance

    1. Upload an image
        ```bash
        openstack image create --disk-format qcow2 --container-format bare --file <IMAGE_FILE> test-image
        ```
    2. Create a network
        ```bash
        openstack network create test-network
        ```
    3. Launch an instance:
        ```bash
        openstack server create --flavor m1.small --image test-image --network test-network --key-name default-keypair test-instance
        ```
4. Test Swift Storage

    1. Create a container:
        ```bash
        openstack container create test-container
        ```

    2. Upload an object:
        ```bash
        openstack object create test-container <LOCAL_FILE>
        ```

    3. Download the object:
        ```bash
        openstack object save test-container <OBJECT_NAME>
        ```

5. Test Trove Database

    1. Create a database instance:
        ```bash
        openstack database instance create --flavor m1.small --size 5 --datastore mysql --datastore-version 5.7 test-db
        ```
    
    2. Verify the database instance
        ```bash
        openstack database instance list
        ```

6. Verify Logs

* eck logs for each service in /var/log/<service-name>/.
* Example:
    ```bash
    tail -f /var/log/nova/nova-api.log
    ```