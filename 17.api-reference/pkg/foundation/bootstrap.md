---
title: bootstrap
description: "API Reference documentation for the bootstrap package in Tako framework"
navigation:
  icon: i-lucide-package
---
# bootstrap

```go
import "gettako.dev/tako/pkg/foundation/bootstrap"
```

Package bootstrap provides application initialization logic.

## Index

- [func Boot\(app \*foundation.Application\) error](<#Boot>)
- [func DefaultBootstrappers\(\) \[\]foundation.Bootstrapper](<#DefaultBootstrappers>)
- [type BootProviders](<#BootProviders>)
  - [func \(b \*BootProviders\) Bootstrap\(app \*foundation.Application\) error](<#BootProviders.Bootstrap>)
- [type EnsureDirectories](<#EnsureDirectories>)
  - [func \(b \*EnsureDirectories\) Bootstrap\(\_ \*foundation.Application\) error](<#EnsureDirectories.Bootstrap>)
- [type LoadConfiguration](<#LoadConfiguration>)
  - [func \(b \*LoadConfiguration\) Bootstrap\(app \*foundation.Application\) error](<#LoadConfiguration.Bootstrap>)
- [type LoadLanguageFiles](<#LoadLanguageFiles>)
  - [func \(b \*LoadLanguageFiles\) Bootstrap\(app \*foundation.Application\) error](<#LoadLanguageFiles.Bootstrap>)
- [type RecordMetrics](<#RecordMetrics>)
  - [func \(b \*RecordMetrics\) Bootstrap\(app \*foundation.Application\) error](<#RecordMetrics.Bootstrap>)
- [type RegisterCache](<#RegisterCache>)
  - [func \(b \*RegisterCache\) Bootstrap\(app \*foundation.Application\) error](<#RegisterCache.Bootstrap>)
- [type RegisterCoreCommands](<#RegisterCoreCommands>)
  - [func \(b \*RegisterCoreCommands\) Bootstrap\(app \*foundation.Application\) error](<#RegisterCoreCommands.Bootstrap>)
- [type RegisterLogger](<#RegisterLogger>)
  - [func \(b \*RegisterLogger\) Bootstrap\(app \*foundation.Application\) error](<#RegisterLogger.Bootstrap>)
- [type RegisterProfiler](<#RegisterProfiler>)
  - [func \(b \*RegisterProfiler\) Bootstrap\(app \*foundation.Application\) error](<#RegisterProfiler.Bootstrap>)
- [type RegisterStorage](<#RegisterStorage>)
  - [func \(b \*RegisterStorage\) Bootstrap\(app \*foundation.Application\) error](<#RegisterStorage.Bootstrap>)


<a name="Boot"></a>
## func [Boot](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/default.go#L24>)

```go
func Boot(app *foundation.Application) error
```

Boot is a convenience function that runs the default bootstrappers on the application. It is primarily used by tako.Run and in testing.

<a name="DefaultBootstrappers"></a>
## func [DefaultBootstrappers](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/default.go#L7>)

```go
func DefaultBootstrappers() []foundation.Bootstrapper
```

DefaultBootstrappers returns the standard array of bootstrappers used to initialize a Tako application in the correct order.

<a name="BootProviders"></a>
## type [BootProviders](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/providers.go#L11>)

BootProviders validates dependencies and boots all registered service providers.

```go
type BootProviders struct{}
```

<a name="BootProviders.Bootstrap"></a>
### func \(\*BootProviders\) [Bootstrap](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/providers.go#L14>)

```go
func (b *BootProviders) Bootstrap(app *foundation.Application) error
```

Bootstrap executes dependency validation then boots providers sequentially.

<a name="EnsureDirectories"></a>
## type [EnsureDirectories](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/directories.go#L11>)

EnsureDirectories creates the necessary application directories.

```go
type EnsureDirectories struct{}
```

<a name="EnsureDirectories.Bootstrap"></a>
### func \(\*EnsureDirectories\) [Bootstrap](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/directories.go#L14>)

```go
func (b *EnsureDirectories) Bootstrap(_ *foundation.Application) error
```

Bootstrap executes the initialization step.

<a name="LoadConfiguration"></a>
## type [LoadConfiguration](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/config.go#L13>)

LoadConfiguration loads \(or creates\) the application configuration. It is a no\-op if a contracts.Config is already bound in the container.

```go
type LoadConfiguration struct{}
```

<a name="LoadConfiguration.Bootstrap"></a>
### func \(\*LoadConfiguration\) [Bootstrap](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/config.go#L16>)

```go
func (b *LoadConfiguration) Bootstrap(app *foundation.Application) error
```

Bootstrap executes the initialization step.

<a name="LoadLanguageFiles"></a>
## type [LoadLanguageFiles](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/i18n.go#L11>)

LoadLanguageFiles scans external configuration directories for \`lang/\` folders and auto\-registers any JSON or YAML dictionary files found.

```go
type LoadLanguageFiles struct{}
```

<a name="LoadLanguageFiles.Bootstrap"></a>
### func \(\*LoadLanguageFiles\) [Bootstrap](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/i18n.go#L14>)

```go
func (b *LoadLanguageFiles) Bootstrap(app *foundation.Application) error
```

Bootstrap registers language directories into the i18n manager.

<a name="RecordMetrics"></a>
## type [RecordMetrics](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/metrics.go#L11>)

RecordMetrics emits boot\-time metrics to the profiler if it is active.

```go
type RecordMetrics struct{}
```

<a name="RecordMetrics.Bootstrap"></a>
### func \(\*RecordMetrics\) [Bootstrap](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/metrics.go#L14>)

```go
func (b *RecordMetrics) Bootstrap(app *foundation.Application) error
```

Bootstrap executes the initialization step.

<a name="RegisterCache"></a>
## type [RegisterCache](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/cache.go#L11>)

RegisterCache sets up the file\-backed cache and registers it in the IoC container.

```go
type RegisterCache struct{}
```

<a name="RegisterCache.Bootstrap"></a>
### func \(\*RegisterCache\) [Bootstrap](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/cache.go#L14>)

```go
func (b *RegisterCache) Bootstrap(app *foundation.Application) error
```

Bootstrap executes the initialization step.

<a name="RegisterCoreCommands"></a>
## type [RegisterCoreCommands](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/commands.go#L14>)

RegisterCoreCommands adds all built\-in CLI commands to the registry.

```go
type RegisterCoreCommands struct{}
```

<a name="RegisterCoreCommands.Bootstrap"></a>
### func \(\*RegisterCoreCommands\) [Bootstrap](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/commands.go#L17>)

```go
func (b *RegisterCoreCommands) Bootstrap(app *foundation.Application) error
```

Bootstrap executes the initialization step.

<a name="RegisterLogger"></a>
## type [RegisterLogger](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/logger.go#L13>)

RegisterLogger sets up the internal file logger and registers its cleanup handler.

```go
type RegisterLogger struct{}
```

<a name="RegisterLogger.Bootstrap"></a>
### func \(\*RegisterLogger\) [Bootstrap](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/logger.go#L16>)

```go
func (b *RegisterLogger) Bootstrap(app *foundation.Application) error
```

Bootstrap executes the initialization step.

<a name="RegisterProfiler"></a>
## type [RegisterProfiler](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/profiler.go#L15>)

RegisterProfiler creates and registers the profiler and debug server if enabled.

```go
type RegisterProfiler struct{}
```

<a name="RegisterProfiler.Bootstrap"></a>
### func \(\*RegisterProfiler\) [Bootstrap](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/profiler.go#L18>)

```go
func (b *RegisterProfiler) Bootstrap(app *foundation.Application) error
```

Bootstrap executes the initialization step.

<a name="RegisterStorage"></a>
## type [RegisterStorage](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/storage.go#L13>)

RegisterStorage sets up the persistent key\-value store.

```go
type RegisterStorage struct{}
```

<a name="RegisterStorage.Bootstrap"></a>
### func \(\*RegisterStorage\) [Bootstrap](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrap/storage.go#L16>)

```go
func (b *RegisterStorage) Bootstrap(app *foundation.Application) error
```

Bootstrap executes the initialization step.

Generated by [gomarkdoc](<https://github.com/princjef/gomarkdoc>)
