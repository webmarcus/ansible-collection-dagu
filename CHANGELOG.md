# Changelog

All notable changes to this Ansible collection will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.8] - 2025-11-11

### Fixed
- **Initial DAG Sync API Call:**
  - Fixed API request to `/api/v2/dags/{dagName}/start` endpoint
  - Added required `Content-Type: application/json` and `Accept: application/json` headers
  - Added empty JSON body `{}` as required by Dagu API specification
  - Resolves 500 Internal Server Error when triggering initial Git DAG sync during deployment

## [1.1.7] - 2025-11-11

### Fixed
- **Systemd Service Multiline Format:**
  - Fixed Jinja2 template formatting for multiline ExecStart in systemd service
  - Coordinator advertise address now correctly set without trailing text from next line
  - Resolves issue where `--coordinator.advertise` included "Restart=always" suffix

## [1.1.6] - 2025-11-11

### Fixed
- **Coordinator Service Activation:**
  - Systemd service now passes coordinator CLI flags to `dagu start-all` command
  - Coordinator is properly started in distributed mode when `dagu_distributed_mode: "coordinator"`
  - Fixes issue where coordinator remained inactive despite config file settings
  - CLI flags override config file for coordinator activation (as per Dagu design)

### Changed
- Systemd service ExecStart now includes `--coordinator.host`, `--coordinator.port`, and `--coordinator.advertise` flags when in coordinator mode

## [1.1.5] - 2025-11-11

### Added
- **Coordinator Advertise Address Support:**
  - Added optional `dagu_coordinator_advertise` variable to specify the address workers use to connect
  - Enables distributed mode over VPN, Tailscale, or any network topology
  - Auto-detects hostname if not specified (maintains backward compatibility)
  - Example: `dagu_coordinator_advertise: "workstation"` for Tailscale hostname

### Changed
- Coordinator configuration now properly advertises address in service registry for worker connections

## [1.1.4] - 2025-11-11

### Fixed
- **Scheduler and Coordinator Services:**
  - Changed systemd service from `dagu server` to `dagu start-all` to properly start scheduler, web UI, and coordinator services
  - Updated scheduler configuration to use official parameters: `port`, `lockStaleThreshold`, `lockRetryInterval`, `zombieDetectionInterval`
  - Removed non-existent configuration options: `scheduler.enabled` and `scheduler.evaluationInterval`
  - Fixed distributed mode worker service to use `dagu worker` command
  - This resolves the issue where Scheduler Service and Coordinator Service showed as "Inactive" in the System Status dashboard

### Changed
- Systemd service template now uses `dagu_distributed_mode` instead of deprecated `dagu_node_type`
- Scheduler configuration now follows official Dagu documentation standards

## [1.1.3] - 2025-11-11

### Fixed
- **Scheduler Configuration:**
  - Added missing scheduler configuration section to config.yaml template
  - This was causing scheduled DAGs to never run (e.g., 03:00 daily sync)

## [1.1.2] - 2025-11-10

### Fixed
- **Git Sync DAG:**
  - Changed from `mktemp -d` to persistent named directory `/tmp/dagu-git-sync-temp`
  - Fixes "ERROR: Temp directory not found" when sync-dags step runs
  - Ensures temp directory persists between Dagu step executions

## [1.1.1] - 2025-11-10

### Fixed
- **Git Sync DAG:**
  - Attempted fix for temp directory issue (unsuccessful)
  - Removed EXIT trap

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

[1.1.8]: https://github.com/webmarcus/ansible-collection-dagu/compare/v1.1.7...v1.1.8
[1.1.7]: https://github.com/webmarcus/ansible-collection-dagu/compare/v1.1.6...v1.1.7
[1.1.6]: https://github.com/webmarcus/ansible-collection-dagu/compare/v1.1.5...v1.1.6
[1.1.5]: https://github.com/webmarcus/ansible-collection-dagu/compare/v1.1.4...v1.1.5
[1.1.4]: https://github.com/webmarcus/ansible-collection-dagu/compare/v1.1.3...v1.1.4
[1.1.3]: https://github.com/webmarcus/ansible-collection-dagu/compare/v1.1.2...v1.1.3
[1.1.2]: https://github.com/webmarcus/ansible-collection-dagu/compare/v1.1.1...v1.1.2
[1.1.1]: https://github.com/webmarcus/ansible-collection-dagu/compare/v1.1.0...v1.1.1
[1.1.0]: https://github.com/webmarcus/ansible-collection-dagu/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/webmarcus/ansible-collection-dagu/releases/tag/v1.0.0
