---
layout: post
title: "Dotnet custom logger"
date: 2021-08-12 15:00:00 +0100
categories:
---

```c#
public class FileLogger : ILogger
{
	string logFilePath;

	public FileLogger(string logFilePath)
	=> (this.logFilePath) = (logFilePath);

	public IDisposable BeginScope<TState>(TState state)
	=> null;

	public bool IsEnable(LogLevel logLeve)
	=> logLevel != LogLevel.None;

	public void Log<TState>
				 ( LogLevel logLevel
				 , EventId eventId
				 , TState state
				 , Exception exception
				 , Func<TState, Exception, string> formatter )
	{
		if(!IsEnabled(logLevel)) return;

		var logMessage 
			 = string.Format
				( "{0} {1} {2}"
				, DateTimeOffset.UtcNow.ToString("yyyy-MM-dd::HH:mm:ss")
				, logLevel.ToString()
				, formatter(state,exception) );

		using writer = new StreamWrite(logFilePath);
		writer.WriteLine( logMessage );

	}
}

public class FileLoggerOptions
{
	public string FilePath { get; set; }
	public string DirectoryPath { get; set; }
}

[ProviderAlias("FileLogger")]
public class FileLoggerProvider : ILoggerProvider
{
	string logFilePath;

	public FileLoggerProvider(FileLoggerOptions options)
	{
		if(!Directory.Exists(options.DirectoryPath)
			Directory.CreateDirectory(options.DirectoryPath);

		logFilePath = Path.Combine(options.DirectoryPath, options.FilePath);
	}

	public ILogger CreateLogger(string categoryName)
	=> new FileLoger(logFilePath);

	public void Dispose()
	{}
}

public static class FileLoggerExtensions
{
    public static ILoggingBuilder AddFileLogger
								  ( this ILoggingBuilder builder
								  , Action<FileLoggerOptions> configure )
    {
        builder.Services.AddSingleton<ILoggerProvider, FileLoggerProvider>();
        builder.Services.Configure(configure);
        return builder;
    }
}


public class Program
{
	...

	public static IHostBuilder CreateHost(string[] args)
	=> Host
		.ConfigureWebHostDefaults( webBuilder =>
		{
			webBuilder.UseStartUp<Startup>();
		})
		.ConfigureLogging((hostBuilderContext, logging) =>
		{
			logging.AddFileLogger( options =>
			{
				hostBuilderContext
					.Configuration
					.GetSection("Logging")
					.GetSection("FileLogger")
					.GetSection("Options")
					.Bind(options)
					;
			});
		});
}

{
  "Logging": {
    ...,
    "FileLogger": {
      "Options": {
        "FolderPath": "C:\\var\\logs",
        "FilePath": "bookings.log"
      },
      "LogLevel": {
        "wipm.booking.Controllers": "Information",
        "wipm.booking.Domain": "Information"
      }
    }
  },
  ...
}

```

