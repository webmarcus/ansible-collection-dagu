# Ansible Role: dagu

Comprehensive Dagu workflow scheduler deployment role with support for distributed execution, standalone mode, authentication, and advanced features.

## Description

This role deploys and configures Dagu, a powerful workflow scheduler and orchestrator, supporting:
- Distributed coordinator/worker architecture with gRPC
- Standalone single-instance deployment
- Queue system for task concurrency management
- Multiple authentication methods (Basic, OIDC, Token)
- Git integration for DAG synchronization
- Remote node execution via SSH

## Requirements

- Ansible 2.14+
- Target systems: Debian/Ubuntu, RHEL/CentOS
- Docker (optional, for container-based execution)

## Role Variables

See the collection's main README.md for comprehensive variable documentation and examples.

### Key Variables

```yaml
# Version and paths
dagu_version: "1.24.0"
dagu_install_dir: "/opt/dagu"
dagu_config_dir: "/etc/dagu"

# Network
dagu_host: "0.0.0.0"
dagu_port: 8080

# Distributed mode
dagu_distributed_mode: "standalone"  # or "coordinator" or "worker"
```

## Dependencies

None

## Example Playbook

```yaml
- hosts: dagu_servers
  become: true
  roles:
    - role: webmarcus.dagu.dagu
      vars:
        dagu_version: "1.24.0"
        dagu_port: 8080
```

## License

MIT

## Author

Marcus Hernandez (marcus@webmarcus.com)
