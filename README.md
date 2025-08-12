# WindowsWebAppDiagnoser

## Overview
WindowsWebAppDiagnoser is a command-line diagnostic tool designed to help you quickly identify and troubleshoot availability issues in your Windows-based Azure Web Apps. The tool analyzes HTTP request patterns and provides automated root cause analysis for server errors with actionable recommendations.

> **⚠️ Preview Notice**: This tool is currently in preview. New diagnostic scenarios and capabilities are being added regularly.

## What WindowsWebAppDiagnoser Can Do For You

For detailed information about the tool's capabilities and diagnostic logic, see:
- **[What Does The Tool Do](WhatDoesTheToolDo.md)** - Comprehensive guide to diagnostic capabilities and analysis workflow
- **[Workflow Diagram](WorkflowDiagram.md)** - Visual representation of the diagnostic process and decision flow


## Key Benefits

### ⚡ **Fast Problem Resolution**
- Get actionable insights in seconds for HTTP 5xx error analysis
- Automated root cause analysis for the most common server error scenarios
- Clear priority-ordered recommendations based on error frequency

### 🎯 **Focused Diagnostics**
- Specifically targets Windows web app HTTP availability issues
- Uses Azure's SRE detector APIs for accurate failure data
- Concentrates analysis on the most impactful errors (95% coverage approach)

### 📋 **Clear Reporting**
- Easy-to-understand health status (Healthy/Warning/Critical/Unknown)
- Detailed observations with specific error counts and percentages
- Structured recommendations tailored to each HTTP error type


## When to Use WindowsWebAppDiagnoser

- ✅ **HTTP 5xx Error Investigation** - When your web app is experiencing server errors (500, 502, 503, etc.)
- ✅ **Availability Issues** - When your web app availability has dropped below expected levels
- ✅ **Incident Response** - During active outages to quickly identify the type and frequency of errors
- ✅ **Post-Incident Analysis** - To understand what HTTP errors occurred during specific time periods
- ✅ **Error Pattern Analysis** - To identify which HTTP error types are most impactful
- ✅ **Basic Health Checks** - To get a quick overview of web app availability status


# Download - Windows
Get the latest release from the releases section, or run the following command to download the tool on Windows

```
curl -L -o WindowsWebAppDiagnoser.exe https://github.com/puneetg1983/WindowsWebAppDiagnoser/releases/latest/download/WindowsWebAppDiagnoser.exe
```

# Azure Console (aka Cloud Shell)
You can run this program from Azure Console by going to http://portal.azure.com/#cloudshell and running the below

```
curl -L -o WindowsWebAppDiagnoser https://github.com/puneetg1983/WindowsWebAppDiagnoser/releases/latest/download/WindowsWebAppDiagnoser && chmod +x WindowsWebAppDiagnoser
```

Then, run the tool like this
```
./WindowsWebAppDiagnoser -r <Azure_Resource_Id>
```

> Note: For this to work, you "must" 'Switch to Bash' cloud shell because WindowsWebAppDiagnoser (not WindowsWebAppDiagnoser.exe) is a Linux package
