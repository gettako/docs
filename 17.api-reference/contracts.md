---
title: contracts
description: "API Reference documentation for the contracts package in Tako framework"
navigation:
  icon: i-lucide-box
---
# contracts

```go
import "gettako.dev/tako/contracts"
```

Package contracts defines the interfaces and shared data structures for the Tako framework.

Package contracts provides functionality for the Tako framework.

Package contracts defines the interfaces and shared data structures for the Tako framework.

Package contracts provides functionality for the Tako framework.

Package contracts provides functionality for the Tako framework.

Package contracts defines the interfaces and shared data structures for the Tako framework.

Package contracts defines the interfaces and shared data structures for the Tako framework.

Package contracts defines the interfaces and shared data structures for the Tako framework.

Package contracts provides functionality for the Tako framework.

Package contracts provides functionality for the Tako framework.

Package contracts provides functionality for the Tako framework.

Package contracts defines the interfaces and shared data structures for the Tako framework.

## Index

- [Constants](<#constants>)
- [type Cache](<#Cache>)
- [type Component](<#Component>)
- [type Config](<#Config>)
- [type Container](<#Container>)
- [type DebugServer](<#DebugServer>)
- [type DialogService](<#DialogService>)
- [type Event](<#Event>)
- [type EventBus](<#EventBus>)
- [type EventHandler](<#EventHandler>)
- [type EventQueue](<#EventQueue>)
- [type JobBuilder](<#JobBuilder>)
- [type JobResult](<#JobResult>)
- [type KVStore](<#KVStore>)
- [type KeyManager](<#KeyManager>)
- [type LangManager](<#LangManager>)
- [type Logger](<#Logger>)
- [type MouseComponent](<#MouseComponent>)
- [type MouseManager](<#MouseManager>)
- [type NativeComponent](<#NativeComponent>)
- [type OverlayStack](<#OverlayStack>)
- [type RPCBus](<#RPCBus>)
- [type RPCCaller](<#RPCCaller>)
- [type RPCHandler](<#RPCHandler>)
- [type RPCRequest](<#RPCRequest>)
- [type RPCResponse](<#RPCResponse>)
- [type RPCRouter](<#RPCRouter>)
- [type Recorder](<#Recorder>)
- [type Scheduler](<#Scheduler>)
- [type StateChangedEvent](<#StateChangedEvent>)
- [type StateManager](<#StateManager>)
- [type StateObserver](<#StateObserver>)
- [type StateProducer](<#StateProducer>)
- [type ThemeManager](<#ThemeManager>)
- [type UIManager](<#UIManager>)
- [type UIRenderer](<#UIRenderer>)
- [type ZoneKeyManager](<#ZoneKeyManager>)
- [type ZoneMouseManager](<#ZoneMouseManager>)


## Constants

<a name="EventStateChanged"></a>Built\-in event name constants for framework\-emitted events. Use these instead of raw string literals to avoid typos and to benefit from IDE auto\-completion and static analysis.

Example:

```
bus.On(contracts.EventStateChanged, func(e contracts.Event) {
    // ...
})
```

```go
const (
    // EventStateChanged is emitted by StateManager.Set() for any key change.
    // Payload: map[string]any{"key": string, "value": any}
    EventStateChanged = "state:changed"

    // EventThemeChanged is emitted when the active theme is switched.
    // Payload: string (new theme name)
    EventThemeChanged = "theme:changed"

    // EventLangChanged is emitted when the active language is switched.
    // Payload: string (new language code)
    EventLangChanged = "lang:changed"

    // EventStackPush is emitted after a layer is pushed onto the focus stack.
    // Payload: string (layer ID)
    EventStackPush = "stack:push"

    // EventStackPop is emitted after a layer is popped from the focus stack.
    // Payload: string (layer ID that was removed)
    EventStackPop = "stack:pop"

    // EventSchedulerJobDone is emitted by the Scheduler when a dispatched job completes.
    // Payload: JobResult
    EventSchedulerJobDone = "scheduler:job:done"

    // EventSchedulerJobStarted is emitted when a dispatched job begins execution.
    EventSchedulerJobStarted = "scheduler:job_started"

    // EventSchedulerJobFinished is emitted when a dispatched job finishes execution.
    EventSchedulerJobFinished = "scheduler:job_finished"

    // EventSchedulerTickerStarted is emitted when a ticker (Every) begins.
    EventSchedulerTickerStarted = "scheduler:ticker_started"

    // EventSchedulerTickerFinished is emitted when a ticker (Every) stops.
    EventSchedulerTickerFinished = "scheduler:ticker_finished"
)
```

<a name="Cache"></a>
## type [Cache](<https://github.com/GetTako/tako/blob/main/contracts/cache.go#L9-L39>)

Cache defines the interface for a TTL\-based key\-value cache.

Implementations must be safe for concurrent use from multiple goroutines.

```go
type Cache interface {
    // Has returns true if the key exists and has not expired.
    Has(key string) bool

    // Get retrieves a cached value and unmarshals it into ptr.
    // Returns false if the key is missing or has expired.
    Get(key string, ptr any) bool

    // Set stores value under key with the given TTL.
    // A TTL of 0 means the entry never expires.
    Set(key string, value any, ttl time.Duration) error

    // Remember returns the cached value if it exists. Otherwise it calls fn,
    // stores the result with the given TTL, and returns it.
    // This is the primary entry point for cache-aside patterns.
    //
    //	data, err := ctx.Cache().Remember("github:stars", 5*time.Minute, func() (any, error) {
    //	    return fetchGithubStars()
    //	})
    Remember(key string, ttl time.Duration, fn func() (any, error)) (any, error)

    // Forget removes a specific key from the cache.
    Forget(key string) error

    // Clear removes all entries from the cache.
    Clear() error

    // Flush persists any pending changes to disk (for file-based drivers).
    // A no-op for in-memory drivers.
    Flush() error
}
```

<a name="Component"></a>
## type [Component](<https://github.com/GetTako/tako/blob/main/contracts/component.go#L57-L71>)

Component bundles a UI view and its keybindings into a self\-contained, self\-describing unit. Registering a Component via app.Overlay\(\).Register\(c\) automatically wires the render hook, zone keybindings, and stack management.

Implement Component in your plugin:

```
type SearchBox struct{ query string }

func (s *SearchBox) ID() string  { return "search" }
func (s *SearchBox) Render() any { return "[ search: " + s.query + " ]" }
func (s *SearchBox) RegisterKeys(keys contracts.KeyManager) {
    keys.Zone(s.ID()).Bind("esc", func() { /* close */ })
}
```

Then show it:

```
ctx.Overlay().ShowComponent(&SearchBox{})
```

```go
type Component interface {
    // ID returns a unique identifier used as the layer ID and the default zone
    // name for this component's keybindings.
    ID() string

    // Render returns the component's visual output. The return type is any so
    // the framework stays UI-agnostic; the developer provides whatever their
    // rendering adapter consumes (string, ANSI styled output, etc.).
    Render() any

    // RegisterKeys is called once when the component is wired. Use the provided
    // KeyManager to declare the component's keybindings. Keep this method
    // stateless where possible — the bindings persist until the layer is popped.
    RegisterKeys(keys KeyManager)
}
```

<a name="Config"></a>
## type [Config](<https://github.com/GetTako/tako/blob/main/contracts/config.go#L5-L10>)

Config defines the interface for the framework's configuration system.

```go
type Config interface {
    String(key string, def ...string) string
    Int(key string, def ...int) int
    Bool(key string, def ...bool) bool
    All() map[string]any
}
```

<a name="Container"></a>
## type [Container](<https://github.com/GetTako/tako/blob/main/contracts/container.go#L5-L31>)

Container defines the interface for the Inversion of Control \(IoC\) Service Container.

```go
type Container interface {
    // Singleton registers a singleton instance for a given contract.
    Singleton(contract any, implementation any)

    // Lazy registers a factory that is only called once when Make is first invoked.
    Lazy(contract any, factory func() (any, error))

    // Transient registers a factory that is called every time Make is invoked.
    Transient(contract any, factory func() (any, error))

    // Make resolves the implementation for a given contract.
    // contract must be a pointer to the interface you want to resolve.
    //
    //	// Resolve:
    //	var logger contracts.Logger
    //	container.Make(&logger)
    Make(contract any) error

    // Call invokes the given function, automatically resolving its parameters
    // from the container via reflection. This enables Laravel-style dependency
    // injection where you pass a closure and the container fills in the arguments.
    //
    //	app.Container().Call(func(registry *cli.Registry) {
    //	    registry.Register(&MigrateCommand{})
    //	})
    Call(fn any) error
}
```

<a name="DebugServer"></a>
## type [DebugServer](<https://github.com/GetTako/tako/blob/main/contracts/debug.go#L9-L14>)

DebugServer is the interface for the framework's optional debug/profiling HTTP server. It is implemented by internal/debug/debugger.Server and registered in the IoC container so that the TUI kernel and other consumers remain decoupled from the concrete type.

```go
type DebugServer interface {
    // Start begins listening on the configured address.
    Start() error
    // Stop gracefully shuts down the server within the given context deadline.
    Stop(ctx context.Context) error
}
```

<a name="DialogService"></a>
## type [DialogService](<https://github.com/GetTako/tako/blob/main/contracts/dialog.go#L21-L35>)

DialogService provides common interaction primitives as specialized overlays. Dialogs are UI\-agnostic: the framework handles key routing and event emission; your layout handles the visual display via hooks.

Obtain via the OverlayManager's Dialog\(\) method — never as a standalone top\-level accessor:

```
// Host app
app.Overlay().Dialog().Confirm("Delete?", func(yes bool) { ... })

// Plugin
ctx.Overlay().Dialog().Confirm("Sure?", func(yes bool) { ... })
```

The DialogService can be cached when calling multiple dialog methods:

```
d := ctx.Overlay().Dialog()
d.Confirm("Step 1?", cb1)
```

```go
type DialogService interface {
    // Confirm pushes a confirmation overlay and waits for a y/n or
    // enter/esc keystroke. When the user responds, onResult is called with
    // true (accepted) or false (cancelled), and the overlay is automatically
    // dismissed.
    //
    // The message is exposed as the render output of the
    // "tako.overlay.tako.dialog.confirm" hook so layouts can display and style
    // it however they wish.
    //
    // Events emitted on the EventBus:
    //   - "dialog:confirmed" — user pressed y or enter
    //   - "dialog:cancelled" — user pressed n or esc
    Confirm(message string, onResult func(yes bool))
}
```

<a name="Event"></a>
## type [Event](<https://github.com/GetTako/tako/blob/main/contracts/bus.go#L7-L10>)

Event represents a system event.

```go
type Event struct {
    Name string
    Data any
}
```

<a name="EventBus"></a>
## type [EventBus](<https://github.com/GetTako/tako/blob/main/contracts/bus.go#L16-L35>)

EventBus defines the interface for asynchronous communication between plugins.

```go
type EventBus interface {
    // Publish broadcasts an event to all subscribers. Handlers are called synchronously
    // on the caller's goroutine before Publish returns.
    Emit(event string, data any)

    // Subscribe registers a handler for the given event name.
    // The returned function unsubscribes the handler when called.
    // When ctx is cancelled, the subscriber is automatically pruned on the next Publish.
    Subscribe(ctx context.Context, event string, handler EventHandler) func()

    // On is sugar for Subscribe using context.Background — use when the subscription
    // should live for the duration of the process (or until the returned cancel is called).
    On(event string, handler EventHandler) func()

    // Once registers a handler that fires exactly once for the given event, then auto-unsubscribes.
    Once(ctx context.Context, event string, handler EventHandler)

    // Subscribers returns a map of event names to the number of active subscribers.
    Subscribers() map[string]int
}
```

<a name="EventHandler"></a>
## type [EventHandler](<https://github.com/GetTako/tako/blob/main/contracts/bus.go#L13>)

EventHandler is a function that handles an event.

```go
type EventHandler func(event Event)
```

<a name="EventQueue"></a>
## type [EventQueue](<https://github.com/GetTako/tako/blob/main/contracts/bus.go#L40-L42>)

EventQueue is an internal interface for adapters that need to drain the event queue on each render tick. It is not part of the public EventBus API and should not be used by plugins or application code.

```go
type EventQueue interface {
    Flush() []Event
}
```

<a name="JobBuilder"></a>
## type [JobBuilder](<https://github.com/GetTako/tako/blob/main/contracts/scheduler.go#L15-L21>)

JobBuilder provides a fluent API to hook into a dispatched job's lifecycle.

```go
type JobBuilder interface {
    // OnSuccess registers a callback to be executed if the job returns no error.
    OnSuccess(func(data any)) JobBuilder

    // OnError registers a callback to be executed if the job returns an error.
    OnError(func(err error)) JobBuilder
}
```

<a name="JobResult"></a>
## type [JobResult](<https://github.com/GetTako/tako/blob/main/contracts/scheduler.go#L9-L12>)

JobResult represents the outcome of a dispatched background job.

```go
type JobResult struct {
    Data any
    Err  error
}
```

<a name="KVStore"></a>
## type [KVStore](<https://github.com/GetTako/tako/blob/main/contracts/storage.go#L5-L20>)

KVStore defines the interface for a Key\-Value storage mechanism.

```go
type KVStore interface {
    // Set stores a value under the given key.
    Set(key string, val any) error
    // Get retrieves a value by key and unmarshals it into the provided pointer.
    Get(key string, ptr any) error
    // Has checks if a key exists in the store.
    Has(key string) bool
    // Delete removes a key from the store.
    Delete(key string) error
    // Close safely flushes data and shuts down the store.
    Close() error
    // SetSecret stores a sensitive value under the given key.
    SetSecret(key string, val any) error
    // GetSecret retrieves a sensitive value by key and unmarshals it into the provided pointer.
    GetSecret(key string, ptr any) error
}
```

<a name="KeyManager"></a>
## type [KeyManager](<https://github.com/GetTako/tako/blob/main/contracts/component.go#L10-L24>)

KeyManager is a subset of the key\-registration API exposed to a Component's RegisterKeys method. It allows a Component to declare its own keybindings without receiving the full internal Router, keeping the Component interface clean and testable.

Obtain via app.Overlay\(\).Register\(c\) or ctx.Overlay\(\).Register\(c\).

```go
type KeyManager interface {
    // Bind registers a global keybinding. keys specify the key sequence(s).
    Bind(key any, handler func())

    // Unbind removes a global keybinding.
    Unbind(key any)

    // Zone returns a ZoneKeyManager scoped to the given zone name and optional
    // stack level (defaults to 0).
    Zone(zone string, level ...int) ZoneKeyManager

    // OnFallback registers a handler that captures unnormalized, raw keystrokes
    // when no explicit global or zone binding matches.
    OnFallback(handler func(key string))
}
```

<a name="LangManager"></a>
## type [LangManager](<https://github.com/GetTako/tako/blob/main/contracts/i18n.go#L6-L21>)

LangManager defines the internationalization \(i18n\) service.

```go
type LangManager interface {
    // Register adds a dictionary for a specific locale (e.g., "id").
    Register(locale string, dict map[string]string)

    // Load scans directories for language files and auto-registers them.
    Load(embedded fs.FS, embedDir string, externalDirs ...string) error

    // SetLocale changes the active language, typically emitting a "lang:changed" event.
    SetLocale(locale string)

    // T translates a key. Optional arguments can be provided for formatted strings.
    T(key string, args ...any) string

    // Active returns the currently active locale.
    Active() string
}
```

<a name="Logger"></a>
## type [Logger](<https://github.com/GetTako/tako/blob/main/contracts/logger.go#L5-L24>)

Logger defines the interface for the framework's logging system.

```go
type Logger interface {
    Debug(msg string, args ...any)
    Info(msg string, args ...any)
    Warn(msg string, args ...any)
    Error(msg string, args ...any)

    // With returns a new Logger that always includes the given key-value pairs
    // in every log entry. Keys must be strings; values can be any type.
    //
    //	log := ctx.Logger().With("plugin", "network", "req_id", reqID)
    //	log.Info("request started") // automatically includes plugin=network and req_id=...
    With(args ...any) Logger

    // WithGroup returns a new Logger that prefixes all key-value pairs with
    // the given group name, creating a namespaced sub-object in the log output.
    //
    //	log := ctx.Logger().WithGroup("db")
    //	log.Info("query executed", "duration_ms", 12) // logged as db.duration_ms=12
    WithGroup(name string) Logger
}
```

<a name="MouseComponent"></a>
## type [MouseComponent](<https://github.com/GetTako/tako/blob/main/contracts/component.go#L110-L113>)

MouseComponent is an optional extension of [Component](<#Component>) for components that want to declare mouse zones and handlers. The overlay manager will call RegisterMouse when wiring the component if it implements this interface.

```
type FilePanel struct{}
func (p *FilePanel) ID() string { return "files" }
func (p *FilePanel) Render() any { return "..." }
func (p *FilePanel) RegisterKeys(km contracts.KeyManager) {}
func (p *FilePanel) RegisterMouse(mm contracts.MouseManager) {
    mm.Zone(p.ID()).OnClick(func(x, y int) { /* select item */ })
}
```

```go
type MouseComponent interface {
    Component
    RegisterMouse(m MouseManager)
}
```

<a name="MouseManager"></a>
## type [MouseManager](<https://github.com/GetTako/tako/blob/main/contracts/component.go#L78-L86>)

MouseManager is a subset of the mouse\-registration API exposed to a component's RegisterMouse method. It allows a Component to declare its own mouse zones and handlers without receiving the full internal MouseRegistry.

Obtain by implementing the [MouseComponent](<#MouseComponent>) interface.

```go
type MouseManager interface {
    // UpdateHitbox sets the bounding box for the given zone at the component's
    // default stack level. Call this every render cycle with the current position.
    UpdateHitbox(zoneID string, x, y, width, height int)

    // Zone returns a ZoneMouseManager scoped to the given zone name and optional
    // stack level (defaults to 0).
    Zone(zone string, level ...int) ZoneMouseManager
}
```

<a name="NativeComponent"></a>
## type [NativeComponent](<https://github.com/GetTako/tako/blob/main/contracts/component.go#L121-L124>)

NativeComponent is an optional extension of [Component](<#Component>) for components that need to process low\-level framework events \(such as tea.Msg in BubbleTea\) that are not captured by the Router or need to be processed natively.

The signature uses any to remain UI\-agnostic. When using the BubbleTea adapter, msg will be tea.Msg, and the return values should be \(tea.Model, tea.Cmd\).

```go
type NativeComponent interface {
    Component
    UpdateNative(msg any) (any, any)
}
```

<a name="OverlayStack"></a>
## type [OverlayStack](<https://github.com/GetTako/tako/blob/main/contracts/ui.go#L6-L9>)

OverlayStack is a minimal interface for pushing and popping overlay layers. It is used internally by dialog.Service to avoid depending on the full UIManager.

```go
type OverlayStack interface {
    Show(layerID string, render func() any)
    Unmount()
}
```

<a name="RPCBus"></a>
## type [RPCBus](<https://github.com/GetTako/tako/blob/main/contracts/rpc.go#L44-L49>)

RPCBus defines the interface for synchronized inter\-plugin communication.

```go
type RPCBus interface {
    // Route returns an RPCRouter to register a new RPC endpoint.
    Route(name string) RPCRouter
    // Call returns an RPCCaller to invoke an RPC endpoint.
    Call(name string) RPCCaller
}
```

<a name="RPCCaller"></a>
## type [RPCCaller](<https://github.com/GetTako/tako/blob/main/contracts/rpc.go#L32-L41>)

RPCCaller provides a fluent API to make an RPC request.

```go
type RPCCaller interface {
    // WithPayload attaches the data to send.
    WithPayload(payload any) RPCCaller
    // WithContext attaches a context for deadlines and cancellation.
    WithContext(ctx context.Context) RPCCaller
    // Retry configures the number of retries if the call fails.
    Retry(times int) RPCCaller
    // Await executes the call and blocks until a response or error is received.
    Await() (RPCResponse, error)
}
```

<a name="RPCHandler"></a>
## type [RPCHandler](<https://github.com/GetTako/tako/blob/main/contracts/rpc.go#L19>)

RPCHandler is a function that processes an RPC request.

```go
type RPCHandler func(ctx context.Context, req RPCRequest) (RPCResponse, error)
```

<a name="RPCRequest"></a>
## type [RPCRequest](<https://github.com/GetTako/tako/blob/main/contracts/rpc.go#L9-L11>)

RPCRequest represents an incoming payload to an RPC handler.

```go
type RPCRequest struct {
    Payload any
}
```

<a name="RPCResponse"></a>
## type [RPCResponse](<https://github.com/GetTako/tako/blob/main/contracts/rpc.go#L14-L16>)

RPCResponse represents an outgoing payload from an RPC handler.

```go
type RPCResponse struct {
    Data any
}
```

<a name="RPCRouter"></a>
## type [RPCRouter](<https://github.com/GetTako/tako/blob/main/contracts/rpc.go#L22-L29>)

RPCRouter provides a fluent API to register an RPC endpoint.

```go
type RPCRouter interface {
    // Timeout sets the absolute deadline for the handler execution.
    Timeout(d time.Duration) RPCRouter
    // Guard adds an access control check that runs before the handler.
    Guard(fn func(ctx context.Context) error) RPCRouter
    // Handle binds the handler function to the route.
    Handle(handler RPCHandler)
}
```

<a name="Recorder"></a>
## type [Recorder](<https://github.com/GetTako/tako/blob/main/contracts/recorder.go#L6-L25>)

Recorder provides a fluent API for event and state "time\-travel" recording.

```go
type Recorder interface {
    // Path sets the directory where trace logs will be stored.
    Path(path string) Recorder
    // MaxFileSize sets the maximum size in bytes before log rotation occurs.
    MaxFileSize(size int) Recorder
    // IncludeInputs determines whether keyboard inputs should be recorded.
    IncludeInputs(include bool) Recorder
    // MaskFields redacts specific field names from the recorded payload (deep recursive).
    MaskFields(fields ...string) Recorder
    // IncludeEvents records only events matching the given prefixes.
    IncludeEvents(prefixes ...string) Recorder
    // ExcludeEvents skips events matching the given prefixes.
    ExcludeEvents(prefixes ...string) Recorder
    // Compress enables gzip compression on trace files.
    Compress(enabled bool) Recorder
    // Start begins recording events to the file system.
    Start(ctx context.Context) error
    // Stop flushes buffers and stops recording.
    Stop() error
}
```

<a name="Scheduler"></a>
## type [Scheduler](<https://github.com/GetTako/tako/blob/main/contracts/scheduler.go#L24-L47>)

Scheduler defines a background task runner that safely bridges concurrency with the TUI.

```go
type Scheduler interface {
    // Dispatch runs a function in a background goroutine and provides a JobBuilder to handle the result.
    Dispatch(fn func(ctx context.Context) (any, error)) JobBuilder

    // Every schedules a function to run repeatedly at the specified interval. Returns a function to stop it.
    Every(interval time.Duration, fn func()) func()

    // Cron schedules a function using a standard 5-field cron expression.
    // The function runs in a background goroutine and is automatically stopped
    // when CancelAll is called. Returns a stop function and any parse error.
    //
    // Supported syntax: "min hour day-of-month month day-of-week"
    //
    // Examples:
    //   "0 * * * *"   — every hour on the hour
    //   "0 0 * * *"   — every day at midnight
    //   "*/5 * * * *" — every 5 minutes
    //   "0 9 * * 1"   — every Monday at 09:00
    //   "0 0 1 * *"   — first day of every month at midnight
    Cron(expr string, fn func()) (stop func(), err error)

    // CancelAll gracefully shuts down all running jobs and intervals.
    CancelAll()
}
```

<a name="StateChangedEvent"></a>
## type [StateChangedEvent](<https://github.com/GetTako/tako/blob/main/contracts/events.go#L52-L55>)

StateChangedEvent is the structured payload for [EventStateChanged](<#EventStateChanged>).

```go
type StateChangedEvent struct {
    Key   string `json:"key"`
    Value any    `json:"value"`
}
```

<a name="StateManager"></a>
## type [StateManager](<https://github.com/GetTako/tako/blob/main/contracts/state.go#L9-L31>)

StateManager defines an in\-memory reactive state store.

```go
type StateManager interface {
    // Set updates the value of a state key and automatically emits a "state:changed:<key>" event.
    Set(key string, value any)

    // Get retrieves the current value of a state key. Returns nil if not found.
    Get(key string) any

    // Has checks if a key exists in the state store.
    Has(key string) bool

    // Key returns a StateProducer for a specific key using a fluent API.
    Mutate(key string) StateProducer

    // Observe returns a StateObserver for a specific key using a fluent API.
    Watch(key string) StateObserver

    // MaxKeys sets the upper limit on number of keys stored. When exceeded, oldest keys are evicted.
    MaxKeys(n int) StateManager

    // Shutdown signals all pending TTL goroutines to stop.
    // Must be called once during application teardown.
    Shutdown()
}
```

<a name="StateObserver"></a>
## type [StateObserver](<https://github.com/GetTako/tako/blob/main/contracts/state.go#L46-L53>)

StateObserver provides a fluent API for subscribing to state changes.

```go
type StateObserver interface {
    // Debounce delays the execution of the update callback to prevent excessive re-renders.
    Debounce(d time.Duration) StateObserver
    // OnUpdate sets the callback to be invoked when the state changes.
    OnUpdate(fn func(oldVal, newVal any)) StateObserver
    // Subscribe starts listening for changes. Returns a function to unsubscribe.
    Subscribe(ctx context.Context) func()
}
```

<a name="StateProducer"></a>
## type [StateProducer](<https://github.com/GetTako/tako/blob/main/contracts/state.go#L34-L43>)

StateProducer provides a fluent API for publishing state changes.

```go
type StateProducer interface {
    // Value sets the value to be broadcasted.
    Value(val any) StateProducer
    // TTL sets an expiration time for the state key.
    TTL(d time.Duration) StateProducer
    // Pin marks this key as protected from LRU eviction.
    Pin() StateProducer
    // Broadcast publishes the state change to all observers.
    Broadcast()
}
```

<a name="ThemeManager"></a>
## type [ThemeManager](<https://github.com/GetTako/tako/blob/main/contracts/theme.go#L4-L16>)

ThemeManager defines an agnostic theming engine.

```go
type ThemeManager interface {
    // Register adds a new theme with its corresponding color palette (e.g. hex codes).
    Register(name string, palette map[string]string)

    // Use switches the active theme, typically emitting a "theme:changed" event.
    Use(name string)

    // Get retrieves a color string (e.g., "#bd93f9") for a semantic token (e.g., "primary").
    Get(token string) string

    // Active returns the name of the currently active theme.
    Active() string
}
```

<a name="UIManager"></a>
## type [UIManager](<https://github.com/GetTako/tako/blob/main/contracts/ui.go#L47-L114>)

UIManager is the single high\-level facade for all UI layer management in Tako. It wraps the low\-level Stack, Focus, Hook, and Key subsystems into one coherent entry point.

Three categories of operations live here:

1. Base Layer — set the primary component for the application.
2. Basic overlays — show/close arbitrary render layers.
3. Component overlays — show self\-contained [Component](<#Component>) values that bundle their own render output and keybindings.
4. Dialog sub\-namespace — common interaction primitives \(confirm, alert, prompt\) accessible via UIManager.Dialog.

All render operations are UI\-agnostic: render functions and Component.Render return any, so the framework never assumes a specific rendering library.

Obtain via app.UI\(\) or ctx.UI\(\) inside a plugin.

Base Layer:

```
app.UI().SetBase(&Dashboard{})
```

Basic overlay:

```
app.UI().Show("search", func() any { return searchView })
app.UI().Unmount()
app.UI().UnmountAll()
```

Component overlay:

```
app.UI().MountOverlay(&SearchBox{})
```

Dialog primitives:

```
app.UI().Dialog().Confirm("Delete?", func(yes bool) { ... })
```

```go
type UIManager interface {

    // SetLayout sets the master layout component (e.g. Sidebar, Header).
    // This is the outer shell of the application. It registers keybindings at
    // stack level 0. The Layout component should call RenderView() to inject
    // the active view into its slot.
    Layout(c Component)

    // SetView clears the focus stack (above the layout), registers the given
    // component's keybindings at level 1, and pushes it as the primary view.
    // This is analogous to a page navigation or routing view change.
    MountView(c Component)

    // RenderView returns the visual output of the currently active View component.
    // This is typically called by the Layout component's Render() method.
    RenderView() any

    // Show pushes layerID onto the focus stack, registers render as the hook
    // provider for "tako.overlay.<layerID>", and shifts keyboard focus to the
    // new layer. Calling Show when layerID is already the top layer is a no-op.
    Show(layerID string, render func() any)

    // Close pops the topmost overlay from the stack and reverts focus to the
    // layer below. If the stack is already at the base layer, Close is a no-op.
    Unmount()

    // CloseAll pops every overlay down to (but not including) the base layer.
    UnmountAll()

    // Top returns the ID of the currently active overlay, or an empty string
    // if no overlay is shown (i.e. the base layer is active).
    Top() string

    // IsActive reports whether any overlay is currently shown.
    IsActive() bool

    // Register wires the component's render hook and keybindings without
    // pushing it onto the stack. Use this to pre-register a component before
    // its layer becomes active.
    Register(c Component)

    // ShowOverlay wires the component (hook + keybindings) and immediately
    // pushes it onto the overlay stack, shifting keyboard focus to it.
    // Equivalent to Register(c) followed by Show(c.ID(), c.Render).
    MountOverlay(c Component)

    // Dialog returns the DialogService for this manager, providing
    // common interaction primitives (confirm, alert, prompt, etc.).
    //
    // Dialogs are specialized overlays — they are pushed and dismissed through
    // the same stack managed by this UIManager.
    //
    // Usage:
    //
    //	app.UI().Dialog().Confirm("Delete?", func(yes bool) { ... })
    //
    // Inside a plugin:
    //
    //	ctx.UI().Dialog().Confirm("Sure?", func(yes bool) { ... })
    Dialog() DialogService
}
```

<a name="UIRenderer"></a>
## type [UIRenderer](<https://github.com/GetTako/tako/blob/main/contracts/renderer.go#L5-L8>)

UIRenderer defines the interface for rendering the UI.

```go
type UIRenderer interface {
    Render() error
    Stop() error
}
```

<a name="ZoneKeyManager"></a>
## type [ZoneKeyManager](<https://github.com/GetTako/tako/blob/main/contracts/component.go#L28-L38>)

ZoneKeyManager is a scoped key\-registration helper returned by KeyManager.Zone\(\). It binds keys to a specific zone and stack level.

```go
type ZoneKeyManager interface {
    // Bind registers a keybinding scoped to this zone and stack level.
    Bind(key any, handler func())

    // Unbind removes a keybinding scoped to this zone and stack level.
    Unbind(key any)

    // OnFallback registers a handler that captures unnormalized, raw keystrokes
    // within this zone when no explicit binding matches.
    OnFallback(handler func(key string))
}
```

<a name="ZoneMouseManager"></a>
## type [ZoneMouseManager](<https://github.com/GetTako/tako/blob/main/contracts/component.go#L89-L97>)

ZoneMouseManager is a scoped mouse\-registration helper returned by MouseManager.Zone\(\).

```go
type ZoneMouseManager interface {
    OnClick(handler func(x, y int))
    OnRightClick(handler func(x, y int))
    OnMiddleClick(handler func(x, y int))
    OnScrollUp(handler func(x, y int))
    OnScrollDown(handler func(x, y int))
    OnDragStart(handler func(x, y int))
    OnDrop(handler func(fromZone string, x, y int))
}
```

Generated by [gomarkdoc](<https://github.com/princjef/gomarkdoc>)
