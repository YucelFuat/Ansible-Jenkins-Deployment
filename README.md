# C4N Jenkins Deployment Role

This Ansible role installs and configures Jenkins CI for various deployment scenarios. Originally based on geerlingguy.jenkins, modified and maintained by C4N.

## Deployment Methods

This role supports multiple deployment methods:

1. **Bare Metal Installation**
   ```bash
   ansible-playbook -i inventory.yml playbook.yml -e "jenkins_deployment_method=bare_metal"
   ```

2. **Docker Installation**
   ```bash
   ansible-playbook -i inventory.yml playbook.yml -e "jenkins_deployment_method=docker"
   ```

3. **Docker Compose Installation**
   ```bash
   ansible-playbook -i inventory.yml playbook.yml -e "jenkins_deployment_method=docker_compose"
   ```

4. **Kubernetes Installation**
   ```bash
   ansible-playbook -i inventory.yml playbook.yml -e "jenkins_deployment_method=kubernetes"
   ```

## Testing Locally

To test on your local Ubuntu machine:

1. Install required dependencies:
   ```bash
   sudo apt-get update
   sudo apt-get install -y ansible docker.io docker-compose
   ```

2. Clone this repository:
   ```bash
   git clone https://github.com/cloud4next/ansible-role-jenkins.git
   ```

3. Create a test inventory file:
   ```bash
   echo "localhost ansible_connection=local" > inventory
   ```

4. Create a test playbook:
   ```yaml
   ---
   - hosts: localhost
     become: true
     roles:
       - ansible-role-jenkins
   ```

5. Run the playbook:
   ```bash
   ansible-playbook -i inventory playbook.yml -e "jenkins_deployment_method=docker"
   ```

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

### Deployment Method Configuration
```yaml
jenkins_deployment_method: "bare_metal"  # Options: bare_metal, docker, docker_compose, kubernetes
company_name: "Cloud4Next"
environment: "production"  # Options: production, staging, development
```

### Docker Configuration
```yaml
jenkins_docker_image: "jenkins/jenkins"
jenkins_docker_tag: "lts"
jenkins_docker_network: "jenkins_network"
jenkins_docker_container_name: "cloud4next_jenkins"
jenkins_docker_volumes:
  - "jenkins_home:/var/jenkins_home"
  - "/var/run/docker.sock:/var/run/docker.sock"
```

### Kubernetes Configuration
```yaml
jenkins_k8s_namespace: "jenkins"
jenkins_k8s_storage_class: "standard"
jenkins_k8s_pvc_size: "10Gi"
```

### Jenkins Configuration
```yaml
jenkins_package_state: present
jenkins_prefer_lts: false
jenkins_connection_delay: 5
jenkins_connection_retries: 60
jenkins_home: /var/lib/jenkins
jenkins_hostname: localhost
jenkins_http_port: 8080
```

## Dependencies

- For Docker deployments: Docker and Docker Compose
- For Kubernetes deployments: kubectl and access to a Kubernetes cluster
- For bare metal deployments: curl and Java 8+

## Example Playbooks

### Bare Metal Deployment
```yaml
- hosts: jenkins
  become: true
  
  vars:
    jenkins_deployment_method: "bare_metal"
    jenkins_hostname: jenkins.example.com
    java_packages:
      - openjdk-8-jdk

  roles:
    - role: cloud4next.jenkins
```

### Docker Deployment
```yaml
- hosts: jenkins
  become: true
  
  vars:
    jenkins_deployment_method: "docker"
    jenkins_docker_container_name: "customer_jenkins"
    jenkins_http_port: 8081

  roles:
    - role: cloud4next.jenkins
```

### Kubernetes Deployment
```yaml
- hosts: kubernetes
  become: true
  
  vars:
    jenkins_deployment_method: "kubernetes"
    jenkins_k8s_namespace: "customer-jenkins"
    jenkins_k8s_storage_class: "customer-storage"

  roles:
    - role: cloud4next.jenkins
```

## License

MIT (Expat) / BSD

## Author Information

This role was originally created by [Jeff Geerling](https://www.jeffgeerling.com/), and has been modified and maintained by Cloud4Next for enterprise deployment scenarios.
