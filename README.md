Certificate Deployment Automation using Ansible
Project Overview
This project automates the deployment of Certificate Authority (CA) and intermediate certificates across a large Linux server estate using Ansible.
The playbook was successfully executed in two controlled phases:
•	Phase 1: 400 Development servers
•	Phase 2: 400 Production servers
Total servers covered: 800
The automation ensures consistent certificate placement, correct permissions, validation, and safe execution at scale, eliminating manual configuration and reducing operational risk.
________________________________________
Objectives
•	Standardize certificate deployment across all Linux servers
•	Ensure required PKI directories exist before deployment
•	Copy approved certificates with correct ownership and permissions
•	Validate successful deployment on each host
•	Fail safely on individual hosts without interrupting other servers
•	Support large-scale rollout across hundreds of systems
________________________________________
Certificates Deployed
The certificates are deployed to all target servers:
These certificates are required to establish internal trust chains and secure communications.
________________________________________
Target Directory Structure
Certificates are copied to the following location:
/etc/pki/new_pki/
This approach keeps project-specific certificates isolated while following standard Linux PKI conventions.
________________________________________
Playbook Execution Details
Privilege and Scope
•	The playbook runs on all target hosts defined in the inventory
•	Root privileges are used to write to system directories
•	Fact gathering is disabled to optimize execution time on large inventories
________________________________________
Variable Configuration
Variables are used to define certificate files and destination paths, making the playbook reusable and easy to extend without code changes.
________________________________________
Directory Validation
The playbook checks whether /etc/pki exists before attempting to create it. This ensures idempotent behavior and prevents unnecessary changes on systems that are already compliant.
________________________________________
Directory Creation
If required, the playbook:
•	Creates /etc/pki
•	Creates /etc/pki/new_pki
•	Applies secure ownership and permissions
________________________________________
Certificate Deployment
Certificates are copied from the Ansible control node to each target server:
•	Ownership is set to root
•	File permissions are set to 0644
•	Multiple certificates are handled using a loop for efficiency
________________________________________
Post-Deployment Verification
After copying, the playbook validates that each certificate:
•	Exists in the target directory
•	Is readable by the system
This prevents silent failures and ensures deployment integrity.
________________________________________
Error Handling and Resilience
The playbook uses structured error handling:
•	If a certificate is missing or invalid on a host, that host is marked as failed
•	Execution continues for remaining servers
This design is critical for large-scale enterprise deployments where partial failures must not stop the entire operation.
________________________________________
Deployment Strategy
Phase 1: Development Environment
•	Deployed to 400 development servers
•	Used to validate directory creation, permissions, and certificate integrity
•	No service impact observed
Phase 2: Production Environment
•	Same playbook reused without modification
•	Deployed to 400 production servers
•	Demonstrated scalability, consistency, and production readiness
________________________________________
Key Features
•	Idempotent execution
•	Fault-tolerant per-host error handling
•	Environment-agnostic design
•	Scalable to hundreds of servers
•	Security-compliant ownership and permissions
•	Production-ready automation
________________________________________
How to Run
ansible-playbook -i inventory copy_certificates.yml
________________________________________
Conclusion
This project delivers a secure, repeatable, and scalable solution for certificate deployment across enterprise Linux environments. It demonstrates practical experience with infrastructure automation, Linux PKI management, and controlled Dev-to-Production rollouts at scale.

