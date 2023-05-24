# Logging in Aras R14 (and newer)

[Link to root: Aras Ext Core](../../README.md)

From Aras Innovator R14 and forward the server logging in Aras Innovator has been revamped. And the old school way to write to basic files to server is obsolete. (But still working as of 2023)

## Old vs. New way of logging

The old way of logging would be like: 

``` csharp
    // Old school log to file
    string fileName = "my_method_log";
    string logMessage = "This is the log message";
    CCO.Utilities.WriteDebug(fileName,logMessage);
```

This will write the file **webapp/Server/temp/my_method_log.log**
> 05/24/2023 15:58:21 :
This is the log message

The basic new way would be like:

``` csharp
    // Old school log to file
    string LoggerName = "my_method_log";
    string logMessage = "This is the log message";
    ILogger logger = CCO.LoggerFactory.CreateLogger(LoggerName);
    logger.LogInformation(logMessage);
```

Running this will not do anything if you don't reconfigure the **webapp/Server/appsettings.json** file to log on a different log level - and restart the application pool.

> "LoggerConfiguration": {
    "MinimumLevel": "Fatal"

From default Fatal to **Information**. See more on log levels on [Microsoft](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.logging.loglevel?view=dotnet-plat-ext-7.0)

Restart the application pool from terminal:

``` powershell
# Run this in an Admin terminal/powershell
Restart-WebAppPool -Name "Aras Innovator AppPool ASP.NET Core"
```

Now the you execute the method you will get something like:

> 2023-05-24T16:23:33.645 [INFO] Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker | TraceId: 3cd55b925ba17be3aef0f274b4d9a4d6 | SpanId: 6b6f741267e21e91 | Route matched with {action = "ProcessInnovatorServerRequest", controller = "InnovatorServer"}. Executing controller action with signature System.Xml.XmlDocument ProcessInnovatorServerRequest(System.String, System.Xml.XmlDocument) on controller Aras.Server.Controllers.InnovatorServerController (Aras.Server). | SessionId: 0b9ccf39-c211-5257-c81d-7f280330e2f9
> 2023-05-24T16:23:36.173 [INFO] **HC_LoggingExamples** | TraceId: 3cd55b925ba17be3aef0f274b4d9a4d6 | SpanId: 6b6f741267e21e91 | **This is the log message** | SessionId: 0b9ccf39-c211-5257-c81d-7f280330e2f9
> 2023-05-24T16:23:36.177 [INFO] Microsoft.AspNetCore.Mvc.Infrastructure.ObjectResultExecutor | TraceId: 3cd55b925ba17be3aef0f274b4d9a4d6 | SpanId: 6b6f741267e21e91 | Executing ObjectResult, writing value of type 'System.Xml.XmlDocument'. | SessionId: 0b9ccf39-c211-5257-c81d-7f280330e2f9

this will be written to file **webapp/Server/logs/Innovator_{date}.log** with a lot of other logs. as Aras mentions in [Aras Innovator Release 14 release notes](https://www.aras.com/-/media/files/documentation/release-notes/en/14-0-0/aras-innovator-14---release-notes.ashx) "...and writes server methods logs along
with other server's logs."

## Managing the new way of logging

As the example mentions above, the new way of logging will come with other logging, which we may or may not want. If we want to exclude all Microsoft logging i.e. we could modify the **appsettings.json** file.

First this is the OOTB (2023 file) with the MiniLevel change from Fatal to Information

``` json
{
 "SessionOptions": {
  "IdleTimeout": "08:00:00"
 },
 "LoggerConfiguration": {
  "MinimumLevel": "Information",
  "WriteTo": [
   { "Name": "Console" },
   { "Name": "File" },
   { "Name": "EventLog" }
  ]
 },
 "Kestrel": {
  "Limits": {
   "MinRequestBodyDataRate": {
    "BytesPerSecond": "240",
    "GracePeriod": "00:00:20"
   }
  }
 }
}
```

