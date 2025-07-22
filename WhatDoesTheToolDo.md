# Windows Web App Diagnostic Tool - User Guide

## What Does the Tool Do?

The Windows Web App Diagnostic Tool automatically analyzes your Azure web application to identify why it's experiencing issues or downtime. It acts like a digital detective that examines your app's health and pinpoints the root cause of problems.

## How Our Diagnostic Logic Works

### **Step 1: Initial Health Check**
- We first verify your web app is running on Windows with a supported service plan (Basic tier or higher)
- If there are permission issues, we help identify which Azure tenant contains your resource

### **Step 2: Availability Analysis**
- We examine your web app's request traffic over a specified time period
- We calculate overall availability by analyzing successful vs failed requests
- **Decision Point**: If availability is ≥99.99%, we declare your app healthy and stop here

### **Step 3: Failure Investigation (Only if Issues Found)**
When availability drops below 99.99%, we dive deeper:

- We identify which HTTP error codes are causing the most problems
- We prioritize analysis based on failure frequency (tackle the biggest problems first)
- We focus on the top failure patterns that represent 95% of your issues

### **Step 4: Root Cause Analysis by Error Type**

**For HTTP 500 Errors (Internal Server Errors):**
- **When we collect data:**
  - We always scan Event Logs for .NET application errors
  - We query Application Insights for exception telemetry (if configured)
  - **We collect profiler traces** when we detect .NET applications (ASP.NET or ASP.NET Core)

- **What we analyze:**
  - Identify which application module is failing (e.g., your application code, URL rewrite rules, authentication)
  - For .NET apps: We examine stack traces and exception details from all three data sources
  - We categorize the root cause (application code, configuration, authentication, etc.)
  - **Special focus on 500.30**: ASP.NET Core In-Process startup failures - we analyze Event Logs for detailed startup exceptions and use specialized tools to detect configuration issues
  - **500.30 Simulation**: When Event Logs don't contain startup exceptions, we identify the startup command from web.config and execute it directly to capture the actual failure details
  - **Run from Package Analysis**: When we detect Run from Package deployment issues, we test package URL accessibility, verify zip file integrity, and analyze deployment configuration

**For HTTP 502 Errors (Bad Gateway):**
- We focus on application startup and connectivity issues
- No profiler collection needed - these are typically infrastructure-related
- *Note: 502 error detection is work in progress - more comprehensive checks will be added soon*

**For HTTP 503 Errors (Service Unavailable):**
- We analyze resource exhaustion scenarios (too many requests, full queues)
- We provide scaling recommendations based on the specific sub-error code
- *Note: 503 error detection is work in progress - more comprehensive checks will be added soon*

### **When We Use Each Data Source:**

**Event Logs Scanning:**
- Triggered for all HTTP 500 errors when we detect .NET applications
- Looks for application crashes, startup failures, and unhandled exceptions
- **For 500.30 failures**: Primary source for ASP.NET Core startup exception details

**ASP.NET Core Startup Simulation:**
- **Only triggered for 500.30 errors** when Event Logs don't contain startup exceptions
- Analyzes web.config to identify the startup command (processPath and arguments)
- Executes the startup command directly in the App Service environment to capture real failure details
- Provides actual exception stack traces and error messages from the failed startup attempt

**Run from Package Deployment Analysis:**
- **Triggered when deployment issues are suspected** (especially for 500.30 scenarios)
- Tests package URL accessibility and authentication
- Verifies zip file integrity and structure
- Analyzes WEBSITE_RUN_FROM_PACKAGE configuration settings

**Application Insights Querying:**
- Used when your app has Application Insights configured
- Retrieves exception telemetry and dependency failures during the failure time window

**Profiler Trace Collection:**
- **Only triggered** when we identify .NET applications (ASP.NET or ASP.NET Core) experiencing HTTP 500 errors
- Provides the deepest level of analysis with actual code execution details
- **Uses Failed Request Poller** to monitor for new failures while profiler trace is being collected

**Failed Request Polling:**
- Continuously monitors for real-time failures during analysis
- **Critical for profiler collection** - ensures we capture failures that occur during trace collection
- Helps correlate timing between different failure types

### **Our Decision Tree:**
1. **Healthy app (99.99%+ availability)** → Analysis complete
2. **Minor issues (95-99.98% availability)** → Basic HTTP pattern analysis
3. **Critical issues (<95% availability)** → Full deep-dive analysis including:
   - Event log scanning for .NET apps
   - Application Insights querying (if available)
   - Profiler trace collection for .NET apps with 500 errors
   - **ASP.NET Core startup simulation** for 500.30 failures without Event Log data
   - **Run from Package deployment verification** when deployment issues suspected
   - Resource exhaustion analysis for 503 errors

### **What You Get:**
- **Root cause identification** with specific error categories
- **Actionable recommendations** tailored to your specific failure pattern
- **Priority-ranked issues** so you fix the most impactful problems first
- **Comprehensive analysis** covering 95% of your application's failures

## **Detailed Sub-Status Analysis**

### **HTTP 500 Sub-Status Codes We Analyze:**
- **500.0** - Application Code Errors
- **500.11-15** - Application Restart Issues (11, 12, 13, 15)
- **500.19-24** - Configuration Errors (19, 20, 21, 22, 23, 24)
- **500.30-39** - ASP.NET Core Module Issues:
  - **500.30** - In-Process Startup Failure (most common - detailed Event Log analysis)
  - **500.31-38** - Various ASP.NET Core Module errors (we provide targeted guidance for each specific sub-status)
- **500.50-53** - URL Rewrite Problems (50, 51, 52, 53)
- **500.70-72** - Client Certificate Authentication (70, 71, 72)
- **500.73-79** - EasyAuth Authentication (73-79)
- **500.121** - Azure App Service Request Timeout Issues
- **500.1000-1020** - IIS Node.js Issues (1000-1020)

### **HTTP 502 Analysis:**
- **Autoheal events analysis** - Detects automatic app restarts due to crashes, memory limits, or failures
- **Worker process crash analysis** - Identifies application crashes and startup failures
- **Low memory condition analysis** - Detects memory pressure and out-of-memory situations
- **Fallback recommendations** provided when no specific issues are detected

### **HTTP 503 Sub-Status Codes We Analyze:**
- **503.2** - Concurrent requests limit reached
- **503.3** - ASP.NET queue full  
- **503.4** - FastCGI queue full
- **503.12** - WebSocket request made for site with WebSocket disabled
- **503.13** - Maximum concurrent WebSocket requests reached
- **Comprehensive resource exhaustion analysis** including:
  - **Autoheal events analysis** - Automatic app restarts due to various failures
  - **Worker process crash detection** - Application crashes and startup issues
  - **Low memory condition monitoring** - Memory pressure and OOM scenarios
  - **Module-specific failure analysis** - ASP.NET Core Module issues
  - **Event log analysis with configurable depth** - Startup failures and unhandled exceptions
- **Prioritized analysis flow**: Module failures → Sub-status codes → Comprehensive resource analysis → General recommendations

## **Current Limitations**

**Performance Issues:**
- The tool currently focuses on availability and error diagnosis
- **Performance issues** (slow response times, high latency) are not yet supported
- Support for performance analysis will be added in future versions

The tool automatically adapts its analysis depth based on what type of application you're running and what kinds of errors it's experiencing, ensuring you get the most relevant insights without unnecessary overhead.
