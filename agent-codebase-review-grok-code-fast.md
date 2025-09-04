# Service Status Indicator Agent

This is a comprehensive **Service Status Indicator Agent** - a Python-based system for managing, monitoring, and reporting on custom service health checks. It's designed as a backend agent that integrates with systemd to provide real-time service monitoring capabilities.

## Core Architecture

**Technology Stack:**
- **Python 3.13+** with Poetry for dependency management
- **systemd** integration for service scheduling and execution
- **WebSocket** server for real-time communication
- **Watchdog** library for file system monitoring
- **Click** framework for CLI interface

## Key Components

### 1. Service Management System
- **Service Definition**: Services are defined as bash scripts with embedded metadata:
  ```bash
  #!/bin/bash
  # name: System Updates
  # description: Checking for system updates on Debian/Ubuntu
  # version: 1.0
  # schedule: *:0/01:00
  # timeout: 20
  ```
- **Metadata Parsing**: Extracts service information (name, description, version, schedule, timeout) from script comments
- **Systemd Integration**: Automatically creates `.service` and `.timer` units for each service
- **Service Lifecycle**: Full CRUD operations (Create, Read, Update, Delete) for services

### 2. CLI Interface (`cli.py`)
Provides a comprehensive command-line interface:
- `add <script>`: Register a new service from a bash script
- `remove <service_id>`: Remove an existing service
- `list`: Display all registered services in a formatted table
- `status <service_id>`: Show current status of specific/all services
- `run <service_id>`: Manually trigger service execution

### 3. Real-time Monitoring System
- **WebSocket Server** (`server.py`): Runs on `ws://localhost:5000` to handle client connections
- **File Watcher** (`watcher.py`): Uses watchdog to monitor log directory for changes
- **Log Parsing** (`parsers.py`): Parses log entries in format: `timestamp, status, message`
- **Status Broadcasting**: Automatically sends service status updates to connected WebSocket clients

### 4. Status System
Services report status through standardized exit codes:
- **OK**: Service operating normally
- **UPDATE**: Updates available or maintenance needed
- **WARNING**: Issues detected but service still operational
- **FAILURE**: Service failure occurred
- **ERROR**: Service in error state

## Service Execution Flow

1. **Service Registration**: User provides a bash script with metadata
2. **Unit Creation**: Agent creates systemd service and timer units
3. **Scheduled Execution**: systemd timer triggers service at specified intervals
4. **Script Execution**: Service script runs and outputs status to log file
5. **Log Monitoring**: File watcher detects log changes and parses new entries
6. **Real-time Updates**: Status changes broadcast to WebSocket clients

## Directory Structure
```
/usr/lib/systemd/system/          # systemd unit files (ssi_*.service, ssi_*.timer)
/usr/local/bin/service-status-indicator/  # installed service scripts
/var/log/service-status-indicator/        # service execution logs
```

## Example Use Cases

**System Monitoring Service** (like the provided `system-updates.bash`):
- Checks for available system updates
- Reports "OK" when system is up-to-date
- Reports "UPDATE" with count when updates are available

**Custom Health Checks**:
- Database connectivity monitoring
- API endpoint availability
- Disk space monitoring
- Network connectivity checks

## Key Features

- **Automated Scheduling**: Uses systemd timers with OnCalendar format for flexible scheduling
- **Real-time Monitoring**: WebSocket-based real-time status updates
- **Centralized Logging**: All service outputs logged to standardized locations
- **Service Discovery**: Automatic detection and management of registered services
- **Error Handling**: Comprehensive error handling and validation
- **Timeout Management**: Configurable execution timeouts (1-60 seconds)
- **Force Removal**: Emergency cleanup functionality for problematic services

This system essentially transforms any bash script into a managed, monitored systemd service with real-time status reporting capabilities, making it ideal for infrastructure monitoring and automated health checking scenarios.
