# Serilog

## Console Application

### Core NuGet package

``` cmd
Serilog
Serilog.Extensions.Hosting
Serilog.Settings.Configuration
```

#### Logging to console

``` cmd
Serilog.Sinks.Console
```

#### Logging to Rolling File

``` cmd
Serilog.Sinks.RollingFile
```

#### Logging to Windows event log

``` cmd
Serilog.Sinks.EventLog
```

## Basic Setup

```csharp
// setups a logger that logs debug and above to console
Host
.CreateDefaultBuilder()
.UseSerilog((hostingContext, services, loggerConfiguration) => {
    loggerConfiguration
        .MinimumLevel.Debug()
        .WriteTo.Console()
        .Enrich.FromLogContext();
});
```

```csharp
// setups a logger that logs debug and above to console and rolling file
Host
.CreateDefaultBuilder()
.UseSerilog((hostingContext, services, loggerConfiguration) => {
    loggerConfiguration
        .MinimumLevel.Debug()
        .WriteTo.Console()
        .WriteTo.RollingFile("c://logs/log-{Date}.log")
        .Enrich.FromLogContext();
});
```

## Load configuration from appsettings.json

```c#
Host
.CreateDefaultBuilder()
.UseSerilog((hostingContext, services, loggerConfiguration) => {
    loggerConfiguration
        .ReadFrom.Configuration(hostingContext.Configuration)
        .Enrich.FromLogContext();
})
```

### Basic Configuration

```json
{
  "Serilog": {
    "MinimumLevel": {
      "Default": "Debug",
      "Override": {
        "Microsoft": "Information",
        "System": "Warning"
      }
    }
  }
}
```

### Configuration Containing WriteTo values

We still need to install the correct packages to allow the logs to be written to.

```json
{
  "Serilog": {
    "MinimumLevel": {
      "Default": "Debug",
      "Override": {
        "Microsoft": "Information",
        "System": "Warning"
      }
    },
    "Using": [ "Serilog.Sinks.Console", "Serilog.Sinks.File", "Serilog.Sinks.EventLog" ],
    "Enrich": [ "FromLogContext", "WithMachineName", "WithThreadId" ],
    "WriteTo": [
      {
        "Name": "RollingFile",
        "Args": { "pathFormat": "c://logs/log-{Date}.log" }
      },
      {
        "Name": "Console"
      },
      {
        "Name": "EventLog",
        "Args": {
          "source": "serilogger",
          "logName": "serilogger"
        }
      }
    ]
  }
}
```
