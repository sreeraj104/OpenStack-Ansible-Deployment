
---

### **Updated `troubleshooting.md`**
```markdown
# Troubleshooting Guide

This guide provides solutions to common issues encountered during or after the OpenStack deployment.

---

## 1. Horizon Not Accessible
- **Issue**: Cannot access Horizon at `http://<EC2_PUBLIC_IP>/`.
- **Solution**:
  1. Ensure port 80 is open in the EC2 security group.
  2. Restart Apache:
     ```bash
     sudo systemctl restart apache2
     ```

---

## 2. Swift Storage Issues
- **Issue**: Unable to upload or download objects in Swift.
- **Solution**:
  1. Verify that storage devices are mounted correctly:
     ```bash
     df -h
     ```
  2. Check Swift logs for errors:
     ```bash
     tail -f /var/log/swift/*.log
     ```

---

## 3. Playbook Errors
- **Issue**: Ansible playbook fails during execution.
- **Solution**:
  1. Run the playbook in verbose mode to identify the issue:
     ```bash
     ansible-playbook -i inventory/hosts.ini openstack-deployment.yml -vvv
     ```
  2. Check the Ansible logs for details:
     ```bash
     cat ansible.log
     ```

---

## 4. OpenStack Services Not Running
- **Issue**: Some OpenStack services are not running.
- **Solution**:
  1. Check the status of the service:
     ```bash
     systemctl status <service-name>
     ```
  2. Restart the service if necessary:
     ```bash
     sudo systemctl restart <service-name>
     ```

---

## 5. Network Configuration Issues
- **Issue**: Instances cannot access the internet or communicate with each other.
- **Solution**:
  1. Verify Neutron network configurations:
     ```bash
     openstack network list
     openstack subnet list
     ```
  2. Check the Neutron logs for errors:
     ```bash
     tail -f /var/log/neutron/*.log
     ```

---

## Next Steps
If the issue persists, refer to the [OpenStack Documentation](https://docs.openstack.org/) or open an issue in this repository.