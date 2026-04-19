

<!-- https://medium.com/@stevenhoang/step-by-step-guide-installing-nginx-ingress-on-k3s-pi-4-cluster-835bbceb3eca -->
<!-- https://blog.thenets.org/how-to-create-a-k3s-cluster-with-nginx-ingress-controller/ -->

# K3S Labstack

> A lab environment for deploying and managing **K3S** (Lightweight Kubernetes) clusters using **Ansible** and Kubernetes manifests.

---

## 📋 Table of Contents

- [About](#about)
- [Directory Structure](#directory-structure)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
  - [1. Set Up the K3S Cluster](#1-set-up-the-k3s-cluster)
  - [2. Deploy Applications](#2-deploy-applications)
- [Makefile Commands](#makefile-commands)
- [Inventory Configuration](#inventory-configuration)
- [Contributing](#contributing)
- [License](#license)

---

## About

**K3S Labstack** is a collection of configurations and playbooks to simplify provisioning a K3S cluster in a lab or local environment. It combines:

- **Ansible** — for automated installation and configuration of K3S nodes
- **Kubernetes Manifests** — for deploying applications on top of the K3S cluster

Ideal for learning, experimentation, and staging environments.

---

## Directory Structure

```
k3s-labstack/
├── k3s-ansible/               # Ansible playbooks and configuration
│   ├── inventory.example.yaml   # Example host inventory file
│   ├── inventory.yaml           # Active inventory file (generated from example)
│   └── playbook.yaml            # Main playbook for K3S setup
├── k3s-deployment/            # Kubernetes manifests for application deployments
├── diagram.excalidraw          # Cluster architecture diagram
├── Makefile                    # Shortcut commands
├── .gitignore
└── README.md
```

---

## Prerequisites

Make sure the following tools are installed on your local machine:

| Tool | Minimum Version | Notes |
|------|----------------|-------|
| [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/) | >= 2.12 | For running playbooks |
| Python | >= 3.8 | Ansible dependency |
| SSH | — | Access to target nodes |
| `kubectl` | >= 1.24 | For interacting with the cluster |

Target nodes (servers/VMs) must:
- Be accessible via SSH from your local machine
- Run a Linux OS (Ubuntu 20.04+ / Debian recommended)
- Have internet access to download K3S binaries

---

## Getting Started

### 1. Set Up the K3S Cluster

#### Clone the repository

```bash
git clone https://github.com/didik-prabowo/k3s-labstack.git
cd k3s-labstack
```

#### Configure the inventory

Copy the example inventory file and adjust it to match your hosts:

```bash
cp k3s-ansible/inventory.example.yaml k3s-ansible/inventory.yaml
```

Edit `k3s-ansible/inventory.yaml` with your node IPs/hostnames and SSH credentials.

#### Run the cluster setup

Using the Makefile (quickest way):

```bash
make setup_cluster_k3s
```

Or run Ansible directly:

```bash
cd k3s-ansible
ansible-playbook playbook.yaml -i inventory.yaml
```

Once complete, the kubeconfig will be available on the master node. Copy it to your local machine for `kubectl` access:

```bash
scp user@<master-ip>:~/.kube/config ~/.kube/config
```

### 2. Deploy Applications

Once the cluster is running, apply the manifests from the `k3s-deployment` folder:

```bash
kubectl apply -f k3s-deployment/
```

---

## Makefile Commands

| Command | Description |
|---------|-------------|
| `make setup_cluster_k3s` | Copies the example inventory and runs the Ansible playbook to set up K3S |

---

## Inventory Configuration

The `k3s-ansible/inventory.example.yaml` file contains a host configuration template. Example:

```yaml
all:
  children:
    master:
      hosts:
        master-node:
          ansible_host: 192.168.1.10
          ansible_user: ubuntu
    workers:
      hosts:
        worker-node-1:
          ansible_host: 192.168.1.11
          ansible_user: ubuntu
        worker-node-2:
          ansible_host: 192.168.1.12
          ansible_user: ubuntu
```

> **Note:** Adjust the IPs, usernames, and SSH keys to match your environment. The `inventory.yaml` file is included in `.gitignore` to prevent it from being committed to the repository.

---

## Architecture

See the [`diagram.excalidraw`](./diagram.excalidraw) file for the cluster architecture diagram, viewable at [Excalidraw](https://excalidraw.com/).

---

## Contributing

Contributions are welcome! To contribute:

1. Fork this repository
2. Create a new feature branch (`git checkout -b feature/my-feature`)
3. Commit your changes (`git commit -m 'feat: add my feature'`)
4. Push to the branch (`git push origin feature/my-feature`)
5. Open a Pull Request

---

## License

This project is open-source. Feel free to use and modify it as needed.

---

<p align="center">
  Made with ❤️ for the Kubernetes community
</p>
