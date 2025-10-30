# MachineDialogue-Framework

## Overview

MachineDialogue is a comprehensive UNIX CLI tool development framework that provides building blocks for creating interactive command-line applications. It handles menu systems, function binding, WiFi connectivity, user input handling, logging, and much more.

Version: 2.6 (LookingGlass)
Developer: Alveare Solutions

## Architecture

### Framework Structure

MachineDialogue-Framework/
├── machine-dialogue              # Main executable
├── conf/
│   └── machine-dialogue.conf     # Main configuration
├── src/
│   ├── md-*.sh                   # Modular source files
│   ├── FlowCTRL-Framework/       # Automation sub-framework
│   └── wifi-commander.sh         # WiFi management utility
├── logs/
│   └── machine-dialogue.log      # Application logs
└── dox/
    └── md-*.dox                  # Documentation files


## Core Design Principles

    Modular Architecture: Split into focused, reusable components
    Menu-Driven Interface: Hierarchical menu controllers for navigation
    Safety First: Built-in safety mechanisms to prevent accidental damage
    Extensive Logging: Comprehensive logging system with multiple levels
    Plugin System: Easy integration of external tools and scripts

## Core Components

1. Configuration Management

    File: conf/machine-dialogue.conf

    - Centralized configuration with default values
    - Support for JSON configuration files
    - Dynamic configuration reloading
    - Environment-specific settings

2. Menu System

    Files: md-controllers.sh, md-init.sh

    - Dynamic menu controller creation
    - Hierarchical menu navigation
    - Action binding to menu options
    - Extended banner support
    - Persistent and single-use menu patterns

3. User Interaction

    Files: md-fetchers.sh, md-display.sh

    - Multiple input types (strings, numbers, passwords, files)
    - Data validation and sanitization
    - Color-coded messaging system
    - Progress indicators and loading messages

4. Security & Safety

    Files: md-checkers.sh, md-setters.sh

    - Privilege escalation checks
    - Safety flag system
    - Input validation
    - Secure file operations

5. System Operations

    Files: md-general.sh, md-creators.sh

    - File and directory management
    - Process control
    - Network operations
    - System administration tasks

6. Logging System

    Files: md-loggers.sh

    - Multiple log levels (INFO, DEBUG, ERROR, WARNING, etc.)
    - Configurable log rotation
    - Structured logging format
    - Log viewing utilities

## Configuration

### Main Configuration File

The framework uses a comprehensive configuration system:
```bash
# Core Settings
SCRIPT_NAME='MachineDialogue'
PS3='MachineDialogue> '
MD_SAFETY='on'
MD_LOGGING='on'
MD_ROOT='off'

# Default Paths
MD_DEFAULT=(
    ['project-path']="$MD_DIRECTORY"
    ['tmp-dir']='/tmp'
    ['log-file']="${MD_DIRECTORY}/logs/machine-dialogue.log"
    ['file-editor']="${EDITOR:-vim}"
)

# Checksum Algorithms
MD_CHECKSUM_ALGORITHMS=(
    ['md5']='md5sum'
    ['sha1']='sha1sum'
    ['sha256']='sha256sum'
    ['sha512']='sha512sum'
)
```

### Modular Configuration
```bash
    MD_CONTROLLERS: Menu controller definitions
    MD_CONTROLLER_OPTIONS: Menu option configurations
    MD_SOURCE: Source file imports
    MD_CARGO: Integrated external scripts
    MD_APT_DEPENDENCIES: System package dependencies
```

## Usage

### Basic Initialization
```bash
#!/bin/bash
source "/path/to/machine-dialogue"
lock_and_load
```

### Creating a Simple Menu
```bash
# Define menu controller
create_menu_controller "MainMenu" "Main Application Menu"

# Set menu options
set_menu_controller_options "MainMenu" "Option1,Option2,Option3,Exit"

# Bind actions to options
bind_controller_option "to_action" "MainMenu" "Option1" "handle_option1"
bind_controller_option "to_action" "MainMenu" "Option2" "handle_option2"
bind_controller_option "to_menu" "MainMenu" "SubMenu" "SubMenuController"

# Initialize menu
init_menu "MainMenu"
```

### Advanced Menu with Extended Banner
```bash
# Create controller with extended banner
set_menu_controller_extended_banner "AdvancedMenu" "display_advanced_banner"

# Custom banner function
function display_advanced_banner() {
    display_script_banner
    echo "Additional information for Advanced Menu"
    echo "Current user: $(whoami)"
    echo "System time: $(date)"
}
```

## Module Reference

### Core Modules

1. Display Module (md-display.sh)

    Color-coded messaging (info_msg, error_msg, warning_msg, etc.)
    Banner display (display_script_banner, display_menu_controller_banner)
    Progress indicators (display_loading_message)
    System information display

2. Fetchers Module (md-fetchers.sh)

    User input handling (fetch_data_from_user, fetch_selection_from_user)
    System information (fetch_local_ipv4_address, fetch_external_ipv4_address)
    Network discovery (fetch_available_wireless_access_point_essid_set)
    File system queries (fetch_file_path_from_user, fetch_directory_from_user)

3. Actions Module (md-actions.sh)

    System operations (action_install_dependencies, action_self_destruct)
    File operations (action_edit_temporary_file, action_clear_log_file)
    Network operations (action_connect_to_wireless_access_point)
    Security operations (action_set_safety_on/off)

4. Checkers Module (md-checkers.sh)

    Validation functions (check_value_is_number, check_file_exists)
    Security checks (check_privileged_access, check_safety_on)
    System checks (check_internet_access, check_util_installed)
    Data validation (check_is_ipv4_address, check_is_mac_address)

5. Setters Module (md-setters.sh)

    Configuration management (set_log_file, set_file_editor)
    System configuration (set_wireless_interface, set_checksum_algorithm)
    Menu configuration (set_menu_controller, set_menu_controller_options)

Utility Modules

6. General Operations (md-general.sh)

    File operations (shred_file, remove_directory, archive_file)
    System operations (create_system_user, create_system_group)
    Network operations (lan_scan, connect_to_wireless_access_point)
    Process control (flowctrl_start, flowctrl_stop)

7. Installation Management (md-installers.sh)

    Package management (apt_install_dependencies, pip_install_dependencies)
    Dependency checking (check_python3_library_installed)
    Installation automation

8. Setup & Initialization (md-setup.sh)

    Framework initialization (load_script_name, load_prompt_string)
    Dependency loading (load_apt_dependencies, load_pip_dependencies)
    Menu setup (setup_menu_controller_action_option)

## Integration

FlowCTRL Automation Framework

The framework includes FlowCTRL, a procedure automation system:
```bash
# Start automation procedure
flowctrl_start "/path/to/procedure.sketch"

# Control automation
flowctrl_pause
flowctrl_resume
flowctrl_stop
```

Procedure Sketch Format:
```json
{
    "Stage 1": [
        {
            "name": "Test Action",
            "time": "5m",
            "cmd": "ls -la",
            "setup-cmd": "echo 'Starting...'",
            "teardown-cmd": "echo 'Finished'",
            "on-ok-cmd": "echo 'Success'",
            "on-nok-cmd": "echo 'Failed'",
            "fatal-nok": false,
            "timeout": "10m"
        }
    ]
}
```

### WiFi Commander Integration

Built-in WiFi management capabilities:
```bash
# Connect to WiFi
connect_to_wireless_access_point "password-on" "MyWiFi" "password123"

# Scan for networks
display_available_wireless_access_points

# Disconnect
disconnect_from_wireless_access_point
```

## Development Guide

### Creating a New Module

    Create source file in src/ directory
    Define functions with proper documentation
    Update configuration in machine-dialogue.conf
    Add to MD_SOURCE array for automatic loading
    Create documentation in dox/ directory

### Best Practices

1. Function Naming Convention

    Actions: action_* (e.g., action_install_dependencies)
    Checkers: check_* (e.g., check_file_exists)
    Fetchers: fetch_* (e.g., fetch_data_from_user)
    Setters: set_* (e.g., set_log_file)
    Display: display_*, *_msg (e.g., display_banner, info_msg)

2. Error Handling

```bash
function example_function() {
    local FILE_PATH="$1"

    # Input validation
    check_file_exists "$FILE_PATH"
    if [ $? -ne 0 ]; then
        error_msg "File not found: $FILE_PATH"
        return 1
    fi

    # Main logic with error checking
    if ! perform_operation "$FILE_PATH"; then
        error_msg "Operation failed"
        return 2
    fi

    ok_msg "Operation completed successfully"
    return 0
}
```

3. Logging Standards

```bash
function example_function() {
    debug_msg "Starting operation with parameters: $@"

    # Operation logic

    if [ $? -eq 0 ]; then
        ok_msg "Operation completed"
        log_message "INFO" "Operation completed successfully"
    else
        error_msg "Operation failed"
        log_message "ERROR" "Operation failed with parameters: $@"
    fi
}
```

## Extension Points

1. Adding New Menu Controllers
```bash
# Define controller
create_menu_controller "NewModule" "New Module Description"

# Add options
set_menu_controller_options "NewModule" "Option1,Option2,Back"

# Bind actions
bind_controller_option "to_action" "NewModule" "Option1" "handle_new_option1"
```

2. Integrating External Tools

```bash
# Add to cargo
load_cargo "new-tool" "/path/to/tool.sh"

# Use in actions
function action_use_new_tool() {
    ${MD_CARGO['new-tool']} --parameter value
    return $?
}
```

3. Custom Validation

```bash
function check_custom_validation() {
    local INPUT="$1"

    # Custom validation logic
    if [[ "$INPUT" =~ ^[A-Z]+$ ]]; then
        debug_msg "Input validation passed"
        return 0
    else
        warning_msg "Input validation failed"
        return 1
    fi
}
```

Advanced Features

1. Safety System

    Global safety flag to prevent destructive operations
    Privilege escalation checks for root operations
    Confirmation prompts for critical actions
    Input validation for all user-provided data

2. Logging System

    Multiple log levels with color coding
    Structured log format with timestamps
    Configurable log rotation
    Multiple output viewers (tail, head, more)

3. Plugin Architecture

    Cargo system for external script integration
    Import system for file-based data
    Modular source loading for extensibility
    Hook system for custom functionality

4. Network Capabilities

    WiFi management with WPA supplicant integration
    Network discovery and scanning
    LAN machine enumeration
    Internet connectivity testing

## Troubleshooting

Common Issues

    Permission Denied
        Check MD_ROOT setting
        Verify script execution permissions
        Ensure proper user privileges

    Missing Dependencies
        Run action_install_dependencies
        Check MD_APT_DEPENDENCIES configuration
        Verify Python library installations

    Configuration Errors
        Validate configuration file syntax
        Check path permissions
        Verify environment variables

Debug Mode

Enable debug logging by setting appropriate log levels and checking debug messages:

```bash
# Check debug messages
tail -f ${MD_DEFAULT['log-file']} | grep DEBUG

# Enable verbose output
MD_LOGGING_LEVELS+=('DEBUG')
```

