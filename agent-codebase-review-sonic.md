# Service Status Indicator Agent

## Overview

The Service Status Indicator Agent is a comprehensive Python-based tool for managing and monitoring custom services on Linux systems. It provides a command-line interface to add, remove, list, and monitor custom services that run on scheduled intervals using systemd timers. The agent includes real-time log monitoring capabilities via WebSocket connections, making it suitable for building dashboard applications that display service statuses.

## Architecture

The project consists of several key components:

### Core Components

1. **CLI Tool (`src/cli.py`)**: Main command-line interface for service management
2. **Service Management (`src/service.py`)**: Core Service class that handles service lifecycle
3. **Log Watcher (`src/watcher.py`)**: File system watcher that monitors log files and broadcasts changes via WebSocket
4. **WebSocket Server (`src/server.py`)**: Server that receives log updates and can broadcast to connected clients
5. **Supporting Modules**:
   - `models.py`: Status enumeration (OK, UPDATE, WARNING, FAILURE, ERROR)
   - `parsers.py`: Log line parsing functionality
   - `validators.py`: Input validation for service metadata
   - `commands.py`: Systemd command wrappers
   - `constants.py`: Configuration constants and paths

### Directory Structure

```
/home/remi/workspace/projects/service-status-indicator/serivce_status_indicator_agent/
├── src/
│   ├── cli.py                 # Command-line interface
│   ├── service.py             # Service management logic
│   ├── watcher.py             # Log file watcher daemon
│   ├── server.py              # WebSocket server
│   ├── models.py              # Data models (Status enum)
│   ├── parsers.py             # Log parsing utilities
│   ├── validators.py          # Input validation
│   ├── commands.py            # Systemd command wrappers
│   ├── constants.py           # Configuration constants
│   └── templates/             # systemd unit templates
│       ├── base.service
│       ├── base.timer
│       └── logrotate.conf
├── examples/
│   ├── dummy-service.bash     # Example service script
│   └── system-updates.bash    # System updates monitoring script
├── tests/
├── pyproject.toml             # Poetry project configuration
├── poetry.lock                # Dependency lock file
├── README.md                  # Project documentation
└── .gitignore
```

## Key Features

### Service Management

- **Add Services**: Create new services from bash scripts with metadata
- **Remove Services**: Disable and remove services from the system
- **List Services**: Display all managed services with their status
- **Service Status**: View current status and detailed information for services
- **Manual Execution**: Run services on-demand

### Service Script Format

Services are defined as bash scripts with specific metadata comments:

```bash
#!/bin/bash

# name: Service Name
# description: Brief description of the service
# version: 1.0.0
# schedule: *:0/5  # Every 5 minutes in systemd OnCalendar format
# timeout: 30      # Optional timeout in seconds (default: 20)

# Service logic here
TIMESTAMP=$(date +"%Y-%m-%d %H:%M:%S")
echo "$TIMESTAMP, OK, Service completed successfully"
```

### System Integration

- **systemd Integration**: Uses systemd service and timer units for reliable scheduling
- **Log Management**: Centralized logging in `/var/log/service-status-indicator/`
- **Script Storage**: Service scripts stored in `/usr/local/bin/service-status-indicator/`
- **Unit Files**: systemd unit files created in `/usr/lib/systemd/system/` with `ssi_` prefix

### Real-time Monitoring

- **File System Watching**: Uses `watchdog` library to monitor log file changes
- **WebSocket Broadcasting**: Real-time updates sent to WebSocket server at `ws://localhost:5000`
- **Client Connections**: WebSocket server can handle multiple client connections
- **Automatic Reconnection**: Watcher daemon includes retry logic for WebSocket connections

## Dependencies

- **websockets**: For WebSocket server and client communication
- **watchdog**: For file system monitoring
- **click**: For command-line interface
- **pytest**: For testing framework

## Usage Examples

### Adding a Service

```bash
python -m src.cli add /path/to/service-script.bash
```

### Listing Services

```bash
python -m src.cli list
```

Output format:
```
ID               | Name            | Version | Schedule    | Status
ssi_dummy_service | Dummy Service   | 1       | *:0/1      | OK
```

### Checking Service Status

```bash
python -m src.cli status dummy_service
# Or with details
python -m src.cli status dummy_service --details
```

### Running the Watcher Daemon

```bash
python -m src.watcher
```

### Starting the WebSocket Server

```bash
python -m src.server
```

## Service Lifecycle

1. **Service Creation**:
   - Parse service script metadata
   - Validate inputs (name length, schedule format, etc.)
   - Install script to `/usr/local/bin/service-status-indicator/`
   - Create systemd service unit file
   - Create systemd timer unit file
   - Enable and start the timer

2. **Execution**:
   - systemd timer triggers service execution
   - Service runs bash script with timeout
   - Script outputs status line to stdout
   - Output captured in log file

3. **Monitoring**:
   - Watcher daemon monitors log directory
   - On log file modification, parses new lines
   - Sends parsed data via WebSocket
   - Clients receive real-time status updates

## Status System

The agent uses a standardized status system:

- **OK**: Service completed successfully
- **UPDATE**: Service has updates available
- **WARNING**: Service issues but still operational
- **FAILURE**: Service failed to complete
- **ERROR**: Service encountered an error

## Configuration

Key configuration constants in `constants.py`:

- **PREFIX**: `ssi_` - Prefix for systemd units
- **SERVICES_DIR**: `/usr/lib/systemd/system` - systemd unit directory
- **SCRIPTS_DIR**: `/usr/local/bin/service-status-indicator` - Script storage
- **LOG_DIR**: `/var/log/service-status-indicator` - Log storage

## Security Considerations

- Services run as systemd services (typically with system privileges)
- Log files contain service output that may include sensitive information
- WebSocket server runs on localhost (not exposed externally by default)
- Input validation prevents malicious service metadata

## Development and Testing

- Built with Poetry for dependency management
- Includes pytest for unit testing
- Example service scripts provided for testing
- Modular architecture for easy extension

## Potential Use Cases

- **System Monitoring**: Monitor system health checks
- **Backup Services**: Scheduled backup operations with status reporting
- **Update Monitoring**: Track software update availability
- **Custom Dashboards**: Build web interfaces showing service statuses
- **Automation Tasks**: Any scheduled script that needs status tracking

This agent provides a robust foundation for managing scheduled services with real-time status monitoring, making it ideal for system administrators and developers who need to track the health and execution of automated tasks.
