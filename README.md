# Ansible Jenkins Deployment

This Ansible role deploys Jenkins using various methods including Docker, Docker Compose, Kubernetes, and bare metal installation.

## Features

- Multiple deployment methods:
  - Docker
  - Docker Compose
  - Kubernetes
  - Bare Metal
- Configurable Jenkins settings
- Plugin management
- Security configuration

## Prerequisites

- Ansible 2.9 or higher
- Docker (for Docker deployment)
- kubectl (for Kubernetes deployment)
- Python 3.6 or higher
- Sudo privileges (for bare metal deployment)

## Installation

1. Clone this repository:
```bash
git clone https://github.com/YucelFuat/Ansible-Jenkins-Deployment.git
cd Ansible-Jenkins-Deployment
```

2. Run the playbook with your desired deployment method:
```bash
# For Docker deployment
ansible-playbook -i inventory playbook.yml -e "jenkins_deployment_method=docker"

# For bare metal deployment (requires sudo)
ansible-playbook -i inventory playbook.yml -e "jenkins_deployment_method=bare_metal" -K
```

The `-K` flag is required for bare metal deployment as it prompts for the sudo password needed to install Jenkins and its dependencies.

Available deployment methods:
- `docker`
- `docker_compose`
- `kubernetes`
- `bare_metal`

## Accessing Jenkins

### Docker Deployment
After successful deployment, Jenkins will be available at:
```
http://localhost:8085
```

### Bare Metal Deployment
After successful deployment, Jenkins will be available at:
```
http://localhost:8085
```

### Initial Setup
1. When accessing Jenkins for the first time, you'll need to unlock it using the initial admin password.
2. To get the initial admin password:
   - For Docker deployment:
     ```bash
     docker exec cloud4next_jenkins cat /var/jenkins_home/secrets/initialAdminPassword
     ```
   - For bare metal deployment:
     ```bash
     sudo cat /var/lib/jenkins/secrets/initialAdminPassword
     ```
     or check the system journal:
     ```bash
     sudo journalctl -u jenkins | grep -A 1 "Please use the following password"
     ```
3. Copy the displayed password and paste it into the Jenkins unlock page.
4. Follow the setup wizard to:
   - Install suggested plugins
   - Create your first admin user
   - Configure your Jenkins instance

## Configuration

The role can be configured using the following variables:

```yaml
jenkins_deployment_method: "docker"  # Options: docker, docker_compose, kubernetes, bare_metal
jenkins_http_port: 8085
jenkins_hostname: localhost
```

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a new Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.
