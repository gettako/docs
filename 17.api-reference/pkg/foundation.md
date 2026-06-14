---
title: foundation
description: "API Reference documentation for the foundation package in Tako framework"
navigation:
  icon: i-lucide-package
---
# foundation

```go
import "gettako.dev/tako/pkg/foundation"
```

Package foundation provides functionality for the Tako framework.

## Index

- [func Make\[T any\]\(app \*Application\) \(T, error\)](<#Make>)
- [type Application](<#Application>)
  - [func NewApplication\(\) \*Application](<#NewApplication>)
  - [func \(a \*Application\) BootPending\(\) error](<#Application.BootPending>)
  - [func \(a \*Application\) BootstrapWith\(bootstrappers \[\]Bootstrapper\) error](<#Application.BootstrapWith>)
  - [func \(a \*Application\) CliRegistry\(\) \*cli.Registry](<#Application.CliRegistry>)
  - [func \(a \*Application\) Commands\(cmds ...cli.Command\) \*Application](<#Application.Commands>)
  - [func \(a \*Application\) Config\(\) contracts.Config](<#Application.Config>)
  - [func \(a \*Application\) Container\(\) contracts.Container](<#Application.Container>)
  - [func \(a \*Application\) Context\(\) \*tako.Context](<#Application.Context>)
  - [func \(a \*Application\) CreatedAt\(\) time.Time](<#Application.CreatedAt>)
  - [func \(a \*Application\) Emit\(event string, data any\) \*Application](<#Application.Emit>)
  - [func \(a \*Application\) Events\(\) contracts.EventBus](<#Application.Events>)
  - [func \(a \*Application\) Focus\(zoneID string\) \*Application](<#Application.Focus>)
  - [func \(a \*Application\) GetProviders\(\) \[\]ServiceProvider](<#Application.GetProviders>)
  - [func \(a \*Application\) Hooks\(\) \*builders.HookBuilder](<#Application.Hooks>)
  - [func \(a \*Application\) IsRegistered\(name string\) bool](<#Application.IsRegistered>)
  - [func \(a \*Application\) Jobs\(\) contracts.Scheduler](<#Application.Jobs>)
  - [func \(a \*Application\) Keys\(\) \*builders.KeyBuilder](<#Application.Keys>)
  - [func \(a \*Application\) Logger\(\) contracts.Logger](<#Application.Logger>)
  - [func \(a \*Application\) Make\(abstract any\) error](<#Application.Make>)
  - [func \(a \*Application\) MarkAsBooted\(\)](<#Application.MarkAsBooted>)
  - [func \(a \*Application\) Mount\(renderer contracts.UIRenderer\) \*Application](<#Application.Mount>)
  - [func \(a \*Application\) Mouse\(\) \*builders.MouseBuilder](<#Application.Mouse>)
  - [func \(a \*Application\) OnDestroy\(fn func\(\)\) \*Application](<#Application.OnDestroy>)
  - [func \(a \*Application\) Plugins\(\) \*plugin.Manager](<#Application.Plugins>)
  - [func \(a \*Application\) RPC\(\) contracts.RPCBus](<#Application.RPC>)
  - [func \(a \*Application\) RegisterProviders\(providers ...ServiceProvider\) error](<#Application.RegisterProviders>)
  - [func \(a \*Application\) Router\(\) \*router.Router](<#Application.Router>)
  - [func \(a \*Application\) Shutdown\(\)](<#Application.Shutdown>)
  - [func \(a \*Application\) Spawn\(fn func\(ctx context.Context\)\) \*Application](<#Application.Spawn>)
  - [func \(a \*Application\) Stack\(\) \*router.Stack](<#Application.Stack>)
  - [func \(a \*Application\) State\(\) contracts.StateManager](<#Application.State>)
  - [func \(a \*Application\) Storage\(\) contracts.KVStore](<#Application.Storage>)
  - [func \(a \*Application\) UI\(\) contracts.UIManager](<#Application.UI>)
  - [func \(a \*Application\) WithConfig\(config contracts.Config\) \*Application](<#Application.WithConfig>)
  - [func \(a \*Application\) WithEvents\(bus contracts.EventBus\) \*Application](<#Application.WithEvents>)
  - [func \(a \*Application\) WithLogger\(logger contracts.Logger\) \*Application](<#Application.WithLogger>)
  - [func \(a \*Application\) WithStorage\(storage contracts.KVStore\) \*Application](<#Application.WithStorage>)
  - [func \(a \*Application\) WithoutDefaultQuit\(\) \*Application](<#Application.WithoutDefaultQuit>)
- [type Bootstrapper](<#Bootstrapper>)
- [type DependsOn](<#DependsOn>)
- [type Disposable](<#Disposable>)
- [type HasManifest](<#HasManifest>)
- [type PluginManifest](<#PluginManifest>)
- [type ServiceProvider](<#ServiceProvider>)


<a name="Make"></a>
## func [Make](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L481>)

```go
func Make[T any](app *Application) (T, error)
```

Make fluently resolves a service of type T from the application container.

<a name="Application"></a>
## type [Application](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L31-L52>)

Application represents the core foundation engine and dependency holder.

```go
type Application struct {
    // contains filtered or unexported fields
}
```

<a name="NewApplication"></a>
### func [NewApplication](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L55>)

```go
func NewApplication() *Application
```

NewApplication creates a new Application with an initialized dependency container.

<a name="Application.BootPending"></a>
### func \(\*Application\) [BootPending](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L197>)

```go
func (a *Application) BootPending() error
```

BootPending boots any providers that were dynamically registered after the initial boot phase.

<a name="Application.BootstrapWith"></a>
### func \(\*Application\) [BootstrapWith](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L252>)

```go
func (a *Application) BootstrapWith(bootstrappers []Bootstrapper) error
```

BootstrapWith runs the given bootstrappers sequentially to initialize the application.

<a name="Application.CliRegistry"></a>
### func \(\*Application\) [CliRegistry](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L353>)

```go
func (a *Application) CliRegistry() *cli.Registry
```

CliRegistry returns the command registry.

<a name="Application.Commands"></a>
### func \(\*Application\) [Commands](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L358>)

```go
func (a *Application) Commands(cmds ...cli.Command) *Application
```

Commands fluently registers one or more CLI commands directly to the application.

<a name="Application.Config"></a>
### func \(\*Application\) [Config](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L376>)

```go
func (a *Application) Config() contracts.Config
```

Config returns the resolved Config from the application context.

<a name="Application.Container"></a>
### func \(\*Application\) [Container](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L366>)

```go
func (a *Application) Container() contracts.Container
```

Container returns the underlying dependency\-injection container.

<a name="Application.Context"></a>
### func \(\*Application\) [Context](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L321>)

```go
func (a *Application) Context() *tako.Context
```

Context returns the framework context.

<a name="Application.CreatedAt"></a>
### func \(\*Application\) [CreatedAt](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L262>)

```go
func (a *Application) CreatedAt() time.Time
```

CreatedAt returns the time the application was instantiated.

<a name="Application.Emit"></a>
### func \(\*Application\) [Emit](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L386>)

```go
func (a *Application) Emit(event string, data any) *Application
```

Emit publishes an event on the Event Bus.

<a name="Application.Events"></a>
### func \(\*Application\) [Events](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L381>)

```go
func (a *Application) Events() contracts.EventBus
```

Events returns the resolved EventBus from the application context.

<a name="Application.Focus"></a>
### func \(\*Application\) [Focus](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L300>)

```go
func (a *Application) Focus(zoneID string) *Application
```

Focus sets the focused zone at the current top stack level.

<a name="Application.GetProviders"></a>
### func \(\*Application\) [GetProviders](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L182>)

```go
func (a *Application) GetProviders() []ServiceProvider
```

GetProviders returns all registered service providers.

<a name="Application.Hooks"></a>
### func \(\*Application\) [Hooks](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L274>)

```go
func (a *Application) Hooks() *builders.HookBuilder
```

Hooks returns a \[builders.HookBuilder\] for configuring application hooks fluently.

<a name="Application.IsRegistered"></a>
### func \(\*Application\) [IsRegistered](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L187>)

```go
func (a *Application) IsRegistered(name string) bool
```

IsRegistered checks if a provider with the given name has been registered.

<a name="Application.Jobs"></a>
### func \(\*Application\) [Jobs](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L336>)

```go
func (a *Application) Jobs() contracts.Scheduler
```

Jobs returns the Scheduler from the application context.

<a name="Application.Keys"></a>
### func \(\*Application\) [Keys](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L279>)

```go
func (a *Application) Keys() *builders.KeyBuilder
```

Keys returns a \[builders.KeyBuilder\] for registering keybindings fluently.

<a name="Application.Logger"></a>
### func \(\*Application\) [Logger](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L371>)

```go
func (a *Application) Logger() contracts.Logger
```

Logger returns the resolved Logger from the application context.

<a name="Application.Make"></a>
### func \(\*Application\) [Make](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L399>)

```go
func (a *Application) Make(abstract any) error
```

Make is a shorthand for a.Context\(\).Container\(\).Make\(\) to resolve dependencies.

<a name="Application.MarkAsBooted"></a>
### func \(\*Application\) [MarkAsBooted](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L192>)

```go
func (a *Application) MarkAsBooted()
```

MarkAsBooted marks the application as booted.

<a name="Application.Mount"></a>
### func \(\*Application\) [Mount](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L445>)

```go
func (a *Application) Mount(renderer contracts.UIRenderer) *Application
```

Mount fluently registers a custom UI renderer into the IoC container.

<a name="Application.Mouse"></a>
### func \(\*Application\) [Mouse](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L284>)

```go
func (a *Application) Mouse() *builders.MouseBuilder
```

Mouse returns a \[builders.MouseBuilder\] for registering mouse zones and handlers fluently.

<a name="Application.OnDestroy"></a>
### func \(\*Application\) [OnDestroy](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L347>)

```go
func (a *Application) OnDestroy(fn func()) *Application
```

OnDestroy registers a callback to be executed when the application shuts down.

<a name="Application.Plugins"></a>
### func \(\*Application\) [Plugins](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L267>)

```go
func (a *Application) Plugins() *plugin.Manager
```

Plugins returns the plugin manager for this application.

<a name="Application.RPC"></a>
### func \(\*Application\) [RPC](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L331>)

```go
func (a *Application) RPC() contracts.RPCBus
```

RPC returns the RPCBus from the application context.

<a name="Application.RegisterProviders"></a>
### func \(\*Application\) [RegisterProviders](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L140>)

```go
func (a *Application) RegisterProviders(providers ...ServiceProvider) error
```

RegisterProviders registers one or more service providers. Each provider's Register\(\) is called immediately. If the app is already booted, providers are queued for deferred boot.

<a name="Application.Router"></a>
### func \(\*Application\) [Router](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L289>)

```go
func (a *Application) Router() *router.Router
```

Router returns the application's key\-routing engine.

<a name="Application.Shutdown"></a>
### func \(\*Application\) [Shutdown](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L405>)

```go
func (a *Application) Shutdown()
```

Shutdown tears down providers \(LIFO\), cancels context, and runs cleanup. Safe to call multiple times — only the first call executes.

<a name="Application.Spawn"></a>
### func \(\*Application\) [Spawn](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L341>)

```go
func (a *Application) Spawn(fn func(ctx context.Context)) *Application
```

Spawn starts a background goroutine tied to the framework's lifecycle.

<details><summary>Example</summary>
<p>



```go
package main

import (
	"context"
	"fmt"

	"gettako.dev/tako/pkg/foundation"
)

func main() {
	app := foundation.NewApplication()

	// Spawn a background goroutine tied to the application's lifecycle.
	// It will automatically be waited for during app.Shutdown()
	// up to a maximum timeout of 3 seconds.
	app.Spawn(func(ctx context.Context) {
		<-ctx.Done()
		// Clean up resources when context is canceled
		fmt.Println("Background job shutting down cleanly.")
	})

	app.Shutdown()
}
```

#### Output

```
Background job shutting down cleanly.
```

</p>
</details>

<a name="Application.Stack"></a>
### func \(\*Application\) [Stack](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L311>)

```go
func (a *Application) Stack() *router.Stack
```

Stack returns the focus stack from the application router.

<a name="Application.State"></a>
### func \(\*Application\) [State](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L326>)

```go
func (a *Application) State() contracts.StateManager
```

State returns the state manager from the application context.

<a name="Application.Storage"></a>
### func \(\*Application\) [Storage](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L394>)

```go
func (a *Application) Storage() contracts.KVStore
```

Storage returns the persistent key\-value store from the application context.

<a name="Application.UI"></a>
### func \(\*Application\) [UI](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L316>)

```go
func (a *Application) UI() contracts.UIManager
```

UI returns the high\-level layer manager facade \(base layer, overlays, dialogs\).

<a name="Application.WithConfig"></a>
### func \(\*Application\) [WithConfig](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L457>)

```go
func (a *Application) WithConfig(config contracts.Config) *Application
```

WithConfig fluently registers a custom configuration manager into the IoC container.

<a name="Application.WithEvents"></a>
### func \(\*Application\) [WithEvents](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L469>)

```go
func (a *Application) WithEvents(bus contracts.EventBus) *Application
```

WithEvents fluently registers a custom event bus into the IoC container.

<a name="Application.WithLogger"></a>
### func \(\*Application\) [WithLogger](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L451>)

```go
func (a *Application) WithLogger(logger contracts.Logger) *Application
```

WithLogger fluently registers a custom logger into the IoC container.

<a name="Application.WithStorage"></a>
### func \(\*Application\) [WithStorage](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L463>)

```go
func (a *Application) WithStorage(storage contracts.KVStore) *Application
```

WithStorage fluently registers a custom key\-value store into the IoC container.

<a name="Application.WithoutDefaultQuit"></a>
### func \(\*Application\) [WithoutDefaultQuit](<https://github.com/takoterm/tako/blob/main/pkg/foundation/application.go#L475>)

```go
func (a *Application) WithoutDefaultQuit() *Application
```

WithoutDefaultQuit disables the framework's default smart quit behaviour on ctrl\+c.

<a name="Bootstrapper"></a>
## type [Bootstrapper](<https://github.com/takoterm/tako/blob/main/pkg/foundation/bootstrapper.go#L5-L7>)

Bootstrapper defines the interface for application boot steps. It allows the application initialization process to be modular and extensible.

```go
type Bootstrapper interface {
    Bootstrap(app *Application) error
}
```

<a name="DependsOn"></a>
## type [DependsOn](<https://github.com/takoterm/tako/blob/main/pkg/foundation/provider.go#L13-L15>)

DependsOn is an optional interface. Providers that implement it declare which other providers must be registered before Boot is called. The framework validates \(not sorts\) — if a dependency is missing, Boot fails with a clear error.

```go
type DependsOn interface {
    Dependencies() []string
}
```

<a name="Disposable"></a>
## type [Disposable](<https://github.com/takoterm/tako/blob/main/pkg/foundation/provider.go#L26-L28>)

Disposable is an optional interface. Providers that implement it get their Shutdown method called during application teardown \(in LIFO order\).

```go
type Disposable interface {
    Shutdown() error
}
```

<a name="HasManifest"></a>
## type [HasManifest](<https://github.com/takoterm/tako/blob/main/pkg/foundation/provider.go#L20-L22>)

HasManifest is an optional interface. Providers that implement it are treated as "plugins" and appear in \`plugin:list\` CLI output. Internal providers should NOT implement this.

```go
type HasManifest interface {
    Manifest() PluginManifest
}
```

<a name="PluginManifest"></a>
## type [PluginManifest](<https://github.com/takoterm/tako/blob/main/pkg/foundation/provider.go#L31-L38>)

PluginManifest holds metadata for external plugins.

```go
type PluginManifest struct {
    Name        string
    Version     string
    Author      string
    Description string
    Repository  string
    License     string
}
```

<a name="ServiceProvider"></a>
## type [ServiceProvider](<https://github.com/takoterm/tako/blob/main/pkg/foundation/provider.go#L5-L8>)

ServiceProvider is the fundamental building block for all features in Tako. It follows a Two\-Phase Lifecycle: Register \(bind to container\) then Boot \(execute logic\).

```go
type ServiceProvider interface {
    Register(app *Application) error
    Boot(app *Application) error
}
```

Generated by [gomarkdoc](<https://github.com/princjef/gomarkdoc>)
