# PowerShell Lab 1: To-Do List CLI App

# This script creates a basic to-do list where users can add, list, and remove tasks
# Tasks are stored in a simple text file for persistence

# Define the file where tasks will be stored
$taskFile = "tasks.txt"

# Function to add a task
function Add-Task {
    param (
        [string]$Task
    )
    "$Task" | Out-File -Append -FilePath $taskFile
    Write-Host "Task added: $Task"
}

# Function to list all tasks
function List-Tasks {
    if (Test-Path $taskFile) {
        Get-Content $taskFile
    } else {
        Write-Host "No tasks found."
    }
}

# Function to delete a task by line number
function Remove-Task {
    param (
        [int]$TaskNumber
    )
    if (Test-Path $taskFile) {
        $tasks = Get-Content $taskFile
        if ($TaskNumber -gt 0 -and $TaskNumber -le $tasks.Count) {
            $tasks = $tasks | Where-Object {$_ -ne $tasks[$TaskNumber - 1]}
            $tasks | Set-Content $taskFile
            Write-Host "Task removed."
        } else {
            Write-Host "Invalid task number."
        }
    } else {
        Write-Host "No tasks found."
    }
}

# Display menu
function Show-Menu {
    Write-Host "1. Add Task"
    Write-Host "2. List Tasks"
    Write-Host "3. Remove Task"
    Write-Host "4. Exit"
    $choice = Read-Host "Enter your choice"
    return $choice
}

# GitHub Auto-Push Function
function Push-To-GitHub {
    git add .
    git commit -m "Updated To-Do List CLI App"
    git push origin main
    Write-Host "Changes pushed to GitHub!"
}

# Main script loop
while ($true) {
    $choice = Show-Menu
    switch ($choice) {
        "1" {
            $task = Read-Host "Enter task description"
            Add-Task -Task $task
        }
        "2" { List-Tasks }
        "3" {
            $taskNumber = Read-Host "Enter task number to remove"
            Remove-Task -TaskNumber $taskNumber
        }
        "4" {
            Write-Host "Goodbye!"
            Push-To-GitHub
            break
        }
        default {
            Write-Host "Invalid choice, try again."
        }
    }
}
