# Changelog

All notable changes to this Ansible collection will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2025-11-09

### Added
- **Git Integration Enhancements:**
  - Optional scheduled sync - users can now disable schedule by setting `git_sync_schedule: ""`
  - Initial DAG sync trigger via Ansible - ensures all DAGs are synced on first deployment
  - GitHub Actions support for incremental DAG sync (see `.github/workflows/sync-to-dagu.yml` in DAG repository)
  - Three sync strategies documented: GitHub Actions only, GitHub Actions + Schedule, Schedule only

### Changed
- Git sync schedule is now optional in `sync-dags-from-git.yaml.j2` template
- Default `git_sync_schedule` changed from `*/5 * * * *` to `0 3 * * *` (daily backup at 03:00)
- Enhanced documentation in `group_vars/dagu_coordinator.yml` with detailed sync strategy examples
- Updated role defaults with comprehensive Git integration documentation

### Fixed
- N/A

## [1.0.0] - 2025-11-08

### Added
- Initial release of webmarcus.dagu collection
- Complete Dagu installation and configuration role
- Support for distributed coordinator/worker architecture via gRPC
- NFS-based DAG file sharing
- Git integration for DAG synchronization
- Comprehensive configuration options (authentication, TLS, queues, etc.)
- Systemd service management
- Multi-environment support via remote nodes
- Worker labels for task routing

### Changed
- N/A

### Fixed
- N/A

[1.1.0]: https://github.com/webmarcus/ansible-collection-dagu/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/webmarcus/ansible-collection-dagu/releases/tag/v1.0.0
