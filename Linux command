#!/bin/bash

VERSION="v0.1.0"

# Display help message
function show_help() {
    echo "Usage: sysopctl [COMMAND] [ARGUMENTS]"
    echo ""
    echo "Commands:"
    echo "  service list              List all running services"
    echo "  service start <name>      Start a specified service"
    echo "  service stop <name>       Stop a specified service"
    echo "  system load               Display current system load"
    echo "  disk usage                Show disk usage by partition"
    echo "  process monitor           Monitor system processes (similar to 'top')"
    echo "  logs analyze              Analyze system logs for critical errors"
    echo "  backup <path>             Backup the specified directory using rsync"
    echo "  --help                    Show this help message"
    echo "  --version                 Show the command version"
}

# Display version information
function show_version() {
    echo "sysopctl $VERSION"
}

# List all running services
function list_services() {
    systemctl list-units --type=service --state=running
}

# Start a service
function start_service() {
    local service_name=$1
    if [ -z "$service_name" ]; then
        echo "Error: Service name required."
        exit 1
    fi
    systemctl start "$service_name"
    echo "$service_name started."
}

# Stop a service
function stop_service() {
    local service_name=$1
    if [ -z "$service_name" ]; then
        echo "Error: Service name required."
        exit 1
    fi
    systemctl stop "$service_name"
    echo "$service_name stopped."
}

# Show system load
function system_load() {
    uptime
}

# Show disk usage
function disk_usage() {
    df -h
}

# Monitor system processes
function process_monitor() {
    top
}

# Analyze system logs for errors
function logs_analyze() {
    journalctl -p crit -n 10  # Show critical log entries
}

# Backup specified directory
function backup_files() {
    local path=$1

    # Check if path is provided
    if [ -z "$path" ]; then
        echo "Error: Path required."
        exit 1
    fi

    # Ensure the source path exists
    if [ ! -d "$path" ]; then
        echo "Error: Source directory does not exist."
        exit 1
    fi

    # Ensure rsync is installed
    if ! command -v rsync &> /dev/null; then
        echo "Error: rsync command not found. Please install rsync."
        exit 1
    fi

    # Create /backup directory if it doesn't exist
    if [ ! -d "/backup" ]; then
        sudo mkdir -p /backup
        sudo chmod 755 /backup
        echo "/backup directory created."
    fi

    # Generate a timestamp for the backup
    local timestamp=$(date +"%Y-%m-%d_%H-%M-%S")

    # Create a backup folder with the current timestamp
    local backup_dir="/backup/$(basename "$path")_backup_$timestamp"
    mkdir -p "$backup_dir"

    # Perform the backup using rsync
    rsync -av --progress "$path" "$backup_dir"

    # Check if the backup succeeded
    if [ $? -eq 0 ]; then
        echo "Backup completed successfully for $path."
        echo "Backup stored in $backup_dir."
    else
        echo "Backup failed."
        exit 1
    fi
}


# Main script logic
case "$1" in
    service)
        case "$2" in
            list)
                list_services
                ;;
            start)
                start_service "$3"
                ;;
            stop)
                stop_service "$3"
                ;;
            *)
                echo "Error: Unknown service command."
                exit 1
                ;;
        esac
        ;;
    system)
        case "$2" in
            load)
                system_load
                ;;
            *)
                echo "Error: Unknown system command."
                exit 1
                ;;
        esac
        ;;
    disk)
        case "$2" in
            usage)
                disk_usage
                ;;
            *)
                echo "Error: Unknown disk command."
                exit 1
                ;;
        esac
        ;;
    process)
        case "$2" in
            monitor)
                process_monitor
                ;;
            *)
                echo "Error: Unknown process command."
                exit 1
                ;;
        esac
        ;;
    logs)
        case "$2" in
            analyze)
                logs_analyze
                ;;
            *)
                echo "Error: Unknown logs command."
                exit 1
                ;;
        esac
        ;;
    backup)
        backup_files "$2"
        ;;
    --help)
        show_help
        ;;
    --version)
        show_version
        ;;
    *)
        echo "Error: Unknown command."
        show_help
        exit 1
        ;;
esac
