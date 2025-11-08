# Ansible Collection - webmarcus.dagu

Comprehensive Dagu workflow scheduler deployment with distributed execution support.

## Overview

This collection provides a complete Dagu role supporting all 50+ configuration options including:
- **Distributed Execution** - Coordinator/worker architecture with gRPC
- **Standalone Mode** - Single-instance deployment
- **Queue System** - Task concurrency management
- **Authentication** - Basic auth, OIDC, token-based
- **Remote Nodes** - SSH-based execution
- **Git Integration** - DAG synchronization from Git repositories

## Requirements

- Ansible 2.14+
- Target systems: Debian/Ubuntu, RHEL/CentOS
- Docker (optional, for container-based execution)

## Installation

### From Ansible Galaxy

```bash
ansible-galaxy collection install webmarcus.dagu
```

### From Source (Local Development)

```bash
# Clone this repository
git clone https://github.com/webmarcus/ansible-collection-dagu.git

# Install locally
cd ansible-collection-dagu
ansible-galaxy collection build
ansible-galaxy collection install webmarcus-dagu-*.tar.gz --force
```

## Role Variables

### Core Configuration

```yaml
# Dagu version
dagu_version: "1.24.0"

# Installation paths
dagu_install_dir: "/opt/dagu"
dagu_config_dir: "/etc/dagu"
dagu_log_dir: "/var/log/dagu"

# User configuration
dagu_user: "dagu"
dagu_group: "dagu"

# Network binding
dagu_host: "0.0.0.0"
dagu_port: 8080
```

### Distributed Mode - Coordinator

```yaml
# Coordinator mode
dagu_distributed_mode: "coordinator"
dagu_coordinator_host: "0.0.0.0"
dagu_coordinator_port: 50051

# Queue system
dagu_queues_enabled: true
dagu_queues_config:
  default:
    maxConcurrency: 5
```

### Distributed Mode - Worker

```yaml
# Worker mode
dagu_distributed_mode: "worker"
dagu_headless: true
dagu_worker_id: "{{ inventory_hostname }}"
dagu_worker_coordinator_host: "coordinator.example.com"
dagu_worker_coordinator_port: 50051

# Worker labels for task routing
dagu_worker_labels:
  type: "general"
  region: "eu-north-1"
```

### Authentication

```yaml
# Basic authentication
dagu_basic_auth_enabled: true
dagu_basic_auth_username: "admin"
dagu_basic_auth_password: "changeme"

# OIDC
dagu_oidc_enabled: false
dagu_oidc_provider: "https://auth.example.com"
dagu_oidc_client_id: "dagu"
dagu_oidc_client_secret: "secret"
```

## Usage Examples

### Standalone Deployment

```yaml
---
- name: Deploy Dagu Standalone
  hosts: dagu_servers
  become: true

  roles:
    - role: webmarcus.dagu.dagu
```

### Distributed Mode - Coordinator

```yaml
---
- name: Deploy Dagu Coordinator
  hosts: dagu_coordinator
  become: true

  vars:
    dagu_distributed_mode: "coordinator"
    dagu_coordinator_port: 50051
    dagu_queues_enabled: true

  roles:
    - role: webmarcus.dagu.dagu
```

### Distributed Mode - Worker

```yaml
---
- name: Deploy Dagu Worker
  hosts: dagu_workers
  become: true

  vars:
    dagu_distributed_mode: "worker"
    dagu_headless: true
    dagu_worker_coordinator_host: "{{ groups['dagu_coordinator'][0] }}"
    dagu_worker_labels:
      type: "gpu"
      gpu: "nvidia-a100"

  roles:
    - role: webmarcus.dagu.dagu
```

## Distributed Architecture

```
Coordinator
┌──────────────────────────────┐
│ Web UI :8080                 │
│ gRPC Coordinator :50051      │
│ Queue System                 │
└──────────┬───────────────────┘
           │ gRPC
    ┌──────┴──────┬──────────┐
    │             │          │
┌───▼────┐   ┌───▼────┐ ┌───▼────┐
│Worker 1│   │Worker 2│ │Worker 3│
│Headless│   │Headless│ │Headless│
│general │   │gpu     │ │cpu     │
└────────┘   └────────┘ └────────┘
```

## Configuration Options

See `roles/dagu/defaults/main.yml` for all 50+ configuration options.

## Testing

```bash
# Syntax check
ansible-playbook --syntax-check playbook.yml

# Dry run
ansible-playbook --check playbook.yml

# Deploy
ansible-playbook playbook.yml
```

## Verification

```bash
# Check service status
systemctl status dagu

# View logs
journalctl -u dagu -f

# Test Web UI (standalone/coordinator)
curl http://localhost:8080

# Check gRPC port (coordinator)
netstat -tlnp | grep 50051
```

## Documentation

- **Dagu Official Docs**: https://dagu.readthedocs.io/
- **Dagu GitHub**: https://github.com/dagu-org/dagu

## License

MIT

## Author

Marcus Hernandez (marcus@webmarcus.com)
