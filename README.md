# A Comprehensive HPC Deployment for Large-Scale Protein Structure Analysis

This document outlines an academic-style deployment guide for establishing a high-performance computing (HPC) environment tailored to large-scale protein structure analysis. By leveraging automated provisioning, configuration management, and distributed storage/compute services, this project aims to streamline the end-to-end workflow—from dataset acquisition to in-depth analysis of protein data.

---
## 1. Introduction
Accurate analysis of protein structures at a large scale demands robust computational infrastructure and reproducible workflows. In this project, we employ Ansible for configuration management, Prometheus and Grafana for cluster monitoring, MinIO for object storage, and Hadoop for distributed data processing. This approach ensures:

- **Reproducibility**: Automatic configuration scripts guarantee a consistent environment.
- **Scalability**: Leveraging multiple cloud nodes allows efficient handling of high-throughput tasks.
- **Observability**: Real-time performance metrics (CPU, memory, disk I/O) facilitate informed resource management.
- **Modularity**: Each service (e.g., analytics pipeline, data storage) is isolated, enabling flexible updates and maintenance.

---

## 2. Prerequisites
1. Rancher: For creating and managing Kubernetes or standalone node clusters.
2. SSH Key Pairs: Necessary for secure, password-less configuration over Ansible.
3. Python Environment: Required to run Ansible playbooks and related automation scripts.
Before proceeding, ensure the relevant credentials, SSH keys, and domain settings (if any) are available and correctly configured in Rancher.

---

## 3. Deployment Steps

### 3.0 **Generate Key Pairs**
    - Upload to Rancher.
    - Edit `variables.tf` to use your own SSH keys.

### 3.1 **Configure cnc machine**

    cd cnc-provisioning
    ansible-playbook -i local-inventory.ini provision.yaml
    
### 3.2 **(Optional) Update Packages on All Machines**

    ansible-playbook -i generate_inventory.py update-almalinux.yaml


### 3.3 **Setup Monitoring Systems**  
   (**Prometheus**, **Grafana**)

    ansible-playbook -i generate_inventory.py setup-monitoring.yaml

Once the monitoring systems are set up, you can access:
- **Grafana** at [https://grafana-ucabiqx-x.comp0235.condenser.arc.ucl.ac.uk/](https://grafana-ucabiqx-x.comp0235.condenser.arc.ucl.ac.uk/)
  - **Default username:** `admin`
  - **Default password:** `william200212`

- **Prometheus** at [https://prometheus-ucabiqx-x.comp0235.condenser.arc.ucl.ac.uk/graph](https://prometheus-ucabiqx-x.comp0235.condenser.arc.ucl.ac.uk/graph)

### 3.4 **Configure All Nodes**

    ansible-playbook -i generate_inventory.py config-nodes.yaml

### 3.5 **Setup Minio**

    ansible-playbook -i generate_inventory.py setup-minio.yaml


### 3.6 **Download Datasets**  
   (**Human**, **Ecoli**, **Cath**)

    ansible-playbook -i generate_inventory.py setup-datasets.yaml


### 3.7 **Setup Pipeline and Parser**

    ansible-playbook -i generate_inventory.py setup-pipeline-parser.yaml


### 3.8 **Install Hadoop**

    ansible-playbook -i generate_inventory.py setup-hadoop.yaml


### 3.9 **Merizo Analysis**

    python analyser.py

---

# 4. Conclusion
By following this systematically structured deployment, researchers can efficiently manage the complexities of large-scale protein data analysis. This approach integrates modern HPC practices—automated configuration, distributed storage, and comprehensive monitoring—ensuring reproducibility and robustness suitable for cutting-edge biochemical research.
