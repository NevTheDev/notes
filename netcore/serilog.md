# Installing Serilog

## Console Application

### Core NuGet package

```
Serilog
Serilog.Extensions.Hosting
Serilog.Settings.Configuration
```

#### Logging to console

```text
Serilog.Sinks.Console
```

#### Logging to Rolling File

```txt
Serilog.Sinks.RollingFile
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

## appsettings.json

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