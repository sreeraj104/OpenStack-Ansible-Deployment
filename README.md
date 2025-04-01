# OpenStack Deployment with Ansible on EC2

![Build Status](https://img.shields.io/badge/build-passing-brightgreen)
![OpenStack](https://img.shields.io/badge/OpenStack-Ansible-orange)

## Overview
This project automates the deployment of an OpenStack environment using Ansible on an EC2 instance running Ubuntu Jammy (22.04). The deployment includes Compute (Nova), Storage (Swift), and Database (Trove) services with High Availability (HA), scalability, and security.

---

## Features
- **High Availability (HA)**: Ensures resilience for Nova, Swift, and Trove services.
- **Networking**: Configures private and public networks with routing and security groups.
- **Security**: Implements key-pair authentication, RBAC, and Swift encryption.
- **Monitoring and Logging**: Deploys OpenStack Telemetry (Ceilometer) and centralizes logs.
- **Scalability**: Supports auto-scaling for compute instances and dynamic scaling for Swift storage.

---

## Repository Structure
```
openstack-ansible-deployment/
├── ansible/
│   ├── inventory/                # Inventory files for Ansible
│   │   └── hosts.ini             # EC2 instance details
│   ├── roles/                    # Ansible roles
│   │   ├── cinder/               # Cinder tasks
│   │   ├── common/               # Common tasks (dependencies, updates)
│   │   ├── glance/               # Glance tasks
│   │   ├── horizon/              # Horizon tasks
│   │   ├── keystone/             # Glance tasks
│   │   ├── monitoring/           # Monitoring and logging tasks
│   │   ├── neutron/              # Networking tasks
│   │   ├── nova/                 # Nova tasks
│   │   └── scalability/          # Scalability tasks
│   │   ├── security/             # Security tasks
│   │   ├── swift/                # Swift tasks
│   │   ├── trove/                # Trove tasks
│   ├── openstack-deployment.yml  # Main playbook
│   └── group_vars/
│       └── all.yml               # Global variables
├── docs/
│   ├── architecture-diagram.png  # Architecture diagram
│   ├── validation-guide.md       # Validation steps
│   └── troubleshooting.md        # Troubleshooting tips
├── README.md                     # Main documentation
```

---

## Architecture Design

### High Availability (HA)
- **Compute (Nova)**: Multiple services with load balancing.
- **Storage (Swift)**: Replication zones for redundancy.
- **Database (Trove)**: Clustered database backend with load balancing.

### Networking
- **Private Network**: Internal communication between instances.
- **Public Network**: External access to instances.
- **Routing**: Configured between private and public networks.
- **Security Groups**: Firewall rules for access control.

### Security
- Key-pair authentication for compute instances.
- Role-Based Access Control (RBAC) in Keystone.
- Encryption for Swift object storage.

### Monitoring and Logging
- OpenStack Telemetry (Ceilometer) for resource monitoring.
- Centralized logging for Nova, Swift, and Trove.

### Scalability
- Auto-scaling for compute instances.
- Dynamic scaling for Swift storage nodes.

---

## Prerequisites
1. Install Ansible (v2.9 or later) and Python 3:
   ```bash
   sudo apt update
   sudo apt install -y ansible python3 python3-pip
   pip3 install openstacksdk
2. Ensure SSH access to the target EC2 instance.


Target Node (EC2 Instance)
---
1. Use Ubuntu Jammy (22.04) with at least 4 vCPUs and 16GB RAM.
2. Open the following ports in the EC2 security group:
    * 22 (SSH)
    * 80 (Horizon)
    * 5000 (Keystone)
    * 8080 (Swift)
    * 6000-6002 (Swift Rings)
    * 9696 (Neutron)

Environment Variables
---
Export the following environment variables on the control node:

```bash
export KEYSTONE_ADMIN_PASSWORD=<admin_password>
export KEYSTONE_DB_PASSWORD=<keystone_db_password>
export NOVA_SERVICE_PASSWORD=<nova_service_password>
export NOVA_DB_PASSWORD=<nova_db_password>
export GLANCE_SERVICE_PASSWORD=<glance_service_password>
export GLANCE_DB_PASSWORD=<glance_db_password>
export NEUTRON_SERVICE_PASSWORD=<neutron_service_password>
export NEUTRON_DB_PASSWORD=<neutron_db_password>
export CINDER_SERVICE_PASSWORD=<cinder_service_password>
export CINDER_DB_PASSWORD=<cinder_db_password>
export SWIFT_SERVICE_PASSWORD=<swift_service_password>
export TROVE_SERVICE_PASSWORD=<trove_service_password>
export TROVE_DB_PASSWORD=<trove_db_password>
export CEILOMETER_SERVICE_PASSWORD=<ceilometer_service_password> 
export HEAT_SERVICE_PASSWORD=<heat_password>
```

Cloud Configuration
---
Create a clouds.yaml file in ~/.config/openstack/:
```
clouds:
  openstack:
    auth:
      auth_url: http://<KEYSTONE_HOST>:5000/v3
      username: admin
      password: secure_admin_password
      project_name: admin
      user_domain_name: Default
      project_domain_name: Default
    region_name: RegionOne
    interface: public
    identity_api_version: 3

## How to Run the Playbook
1. Clone this repository:
   ```bash
   git clone https://github.com/sreeraj104/OpenStack-Ansible-Deployment.git
   cd openstack-ansible-deployment
   ```
2. Update the `inventory/hosts.ini` file with your EC2 instance details.
    ```bash
    [openstack]
    ec2-instance ansible_host=<EC2_PUBLIC_IP> ansible_user=ubuntu ansible_ssh_private_key_file=<PATH_TO_PRIVATE_KEY>

    [openstack:vars]
    ansible_python_interpreter=/usr/bin/python3
    ```

3. Run the playbook:
   ```bash
   ansible-playbook -i ansible/inventory/hosts.ini ansible/openstack-deployment.yml
   ```

---

## Validation
Refer to the Validation Guide for detailed steps to validate your OpenStack deployment.

---

## Troubleshooting
Refer to the Troubleshooting Guide for solutions to common issues.

---

## Documentation
- [Architecture Diagram](docs/architecture-diagram.png)
- [Validation Guide](docs/validation-guide.md)
- [Troubleshooting Guide](docs/troubleshooting.md)
- [OpenStack Documentation](https://docs.openstack.org/)
- [Ansible Documentation](https://docs.ansible.com/)
