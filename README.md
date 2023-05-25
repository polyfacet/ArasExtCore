# ArasExtCore

Misc. generic implementations, concepts and ideas from my years as a Aras developer consultant.

## Contents

- [Contents](#contents)
  - [Logging (Aras Innovator 14)](#logging-aras-innovator-14)

### Logging (Aras Innovator 14)

The old OOTB/basic way of writing logs from within an Aras (C#) server method would look like this.

``` csharp
    string fileName = "my_method_log";
    string logMessage = "This is the log message";
    CCO.Utilities.WriteDebug(fileName,logMessage);
```

When compiling this (2023 release, and more releases) a compile warning will occur like:

>Line number 35, Warning Number: CS0618, 'Utilities.WriteDebug(string, string)' is obsolete: 'Obsolete since 14.0. Use CallContext.Logger instead.'

From the [Aras Innovator Release 14 release notes](https://www.aras.com/-/media/files/documentation/release-notes/en/14-0-0/aras-innovator-14---release-notes.ashx)
The following can be found.

>2.2 Server Logging Updates
To make development, testing and troubleshooting easier, the following server logging enhancements
have been implemented:

and

>• ASP.NET Core native logging mechanism (ILogger and ILoggerFactory) is available in CallContext
public object. This mechanism uses server's log configuration and writes server methods logs along
with other server's logs. Developers of server methods should use this mechanism for new
development. Old public log API in CallContext is depracated with appropriate messages and will be
removed in the future.

So how do we use this mechanism? Well, for that continue to [Logging in Aras R14](./logging/docs/LoggingInArasR14.md)
