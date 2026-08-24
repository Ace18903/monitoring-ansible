# Monitoring & Centralized Logging with Ansible

Infrastructure as Code (IaC) project that automates the deployment of a complete **monitoring and centralized logging platform** on three Ubuntu servers using **Ansible and Podman**.

The platform combines **Prometheus + Node Exporter + Grafana** for infrastructure monitoring and **Vector + OpenSearch + OpenSearch Dashboards** for centralized log collection and visualization.

---

## Architecture

| Server                | IP Address     | Components                                             |
| --------------------- | -------------- | ------------------------------------------------------ |
| **server-monitoring** | `192.168.57.3` | Prometheus, Grafana, OpenSearch, OpenSearch Dashboards |
| **server-exporter**   | `192.168.57.4` | Node Exporter, Vector                                  |
| **server-app**        | `192.168.57.9` | Vector                                                 |

### Monitoring Flow

```text
server-exporter
      │
      ▼
Node Exporter
      │
      ▼
Prometheus
      │
      ▼
Grafana
```

### Centralized Logging Flow

```text
server-exporter ──┐
                  │
server-app ───────┤
                  ▼
                Vector
                  │
                  ▼
              OpenSearch
                  │
                  ▼
       OpenSearch Dashboards
```

---

## Technologies

* **Ansible** — Infrastructure automation and configuration management
* **Podman** — Container runtime
* **Prometheus** — Metrics collection and monitoring
* **Node Exporter** — Linux system metrics
* **Grafana** — Metrics visualization
* **Vector** — Log collection and forwarding
* **OpenSearch** — Log storage and search
* **OpenSearch Dashboards** — Log visualization and analysis
* **Ubuntu** — Server operating system
* **Git / GitHub** — Version control and project hosting

---

## Project Structure

```text
monitoring-ansible/
│
├── README.md
├── ansible.cfg
├── inventory.yml
├── site.yml
│
├── group_vars/
│   └── all/
│       └── vault.yml
│
├── playbooks/
│   ├── common.yml
│   ├── logging.yml
│   ├── monitoring.yml
│   └── node_exporter.yml
│
└── roles/
    ├── common/
    ├── podman/
    ├── prometheus/
    ├── grafana/
    ├── node_exporter/
    ├── vector/
    ├── opensearch/
    └── opensearch_dashboards/
```

The project uses reusable **Ansible roles** to keep the deployment modular, maintainable, and easy to reproduce.

---

## Deployment

### 1. Test Ansible Connectivity

```bash
ansible all_servers -m ping
```

### 2. Check the Playbook Syntax

```bash
ansible-playbook site.yml --syntax-check
```

### 3. Deploy the Complete Infrastructure

```bash
ansible-playbook site.yml
```

If privilege escalation requires a password:

```bash
ansible-playbook site.yml --ask-become-pass --ask-vault-pass
```

After deployment, Ansible configures the required servers, containers, services, volumes, and monitoring/logging components.

---

## Services

Once the deployment is complete, the main interfaces are available at:

| Service                   | URL                         |
| ------------------------- | --------------------------- |
| **Grafana**               | `http://192.168.57.3:3000`  |
| **Prometheus**            | `http://192.168.57.3:9090`  |
| **OpenSearch Dashboards** | `http://192.168.57.3:5601`  |
| **OpenSearch API**        | `https://192.168.57.3:9200` |

---

## Main Objectives

* Automate infrastructure deployment using **Ansible**
* Monitor server resources using **Prometheus and Node Exporter**
* Visualize infrastructure metrics using **Grafana**
* Collect and centralize logs using **Vector**
* Store and search logs using **OpenSearch**
* Visualize and analyze logs using **OpenSearch Dashboards**
* Use reusable Ansible roles for maintainability
* Automate container deployment with **Podman**
* Provide a reproducible **Infrastructure as Code** environment
* Keep monitoring and logging data persistent using dedicated storage

---

## Security

Sensitive configuration is managed using **Ansible Vault**

---

## Deployment in One Command

The complete infrastructure can be deployed with:

```bash
ansible-playbook site.yml --ask-become-pass --ask-vault-pass
```

This is the main entry point for reproducing the monitoring and centralized logging environment.

---

## Project Goal

The goal of this project is to provide a **fully automated, reproducible monitoring and centralized logging infrastructure** using modern DevOps and Infrastructure as Code practices.
