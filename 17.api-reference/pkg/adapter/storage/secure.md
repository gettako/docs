---
title: secure
description: "API Reference documentation for the secure package in Tako framework"
navigation:
  icon: i-lucide-package
---
# secure

```go
import "gettako.dev/tako/pkg/adapter/storage/secure"
```

## Index

- [type Adapter](<#Adapter>)
  - [func New\(baseStore contracts.KVStore\) \*Adapter](<#New>)
  - [func NewWithService\(serviceName string, baseStore contracts.KVStore\) \*Adapter](<#NewWithService>)
  - [func \(a \*Adapter\) Close\(\) error](<#Adapter.Close>)
  - [func \(a \*Adapter\) Delete\(key string\) error](<#Adapter.Delete>)
  - [func \(a \*Adapter\) Get\(key string, ptr any\) error](<#Adapter.Get>)
  - [func \(a \*Adapter\) GetSecret\(key string, ptr any\) error](<#Adapter.GetSecret>)
  - [func \(a \*Adapter\) Has\(key string\) bool](<#Adapter.Has>)
  - [func \(a \*Adapter\) Set\(key string, value any\) error](<#Adapter.Set>)
  - [func \(a \*Adapter\) SetSecret\(key string, val any\) error](<#Adapter.SetSecret>)
- [type Provider](<#Provider>)
  - [func \(p \*Provider\) Boot\(app \*foundation.Application\) error](<#Provider.Boot>)
  - [func \(p \*Provider\) Name\(\) string](<#Provider.Name>)
  - [func \(p \*Provider\) Register\(app \*foundation.Application\) error](<#Provider.Register>)


<a name="Adapter"></a>
## type [Adapter](<https://github.com/takoterm/tako/blob/main/pkg/adapter/storage/secure/adapter.go#L13-L16>)

Adapter wraps an existing KVStore to provide OS\-level secure storage for SetSecret and GetSecret methods using the system keychain.

```go
type Adapter struct {
    // contains filtered or unexported fields
}
```

<a name="New"></a>
### func [New](<https://github.com/takoterm/tako/blob/main/pkg/adapter/storage/secure/adapter.go#L22>)

```go
func New(baseStore contracts.KVStore) *Adapter
```

New creates a new securestore Adapter. The service namespace used in the OS keyring is automatically resolved based on the application's binary name. baseStore is the underlying KVStore used for all non\-secret operations.

<a name="NewWithService"></a>
### func [NewWithService](<https://github.com/takoterm/tako/blob/main/pkg/adapter/storage/secure/adapter.go#L31>)

```go
func NewWithService(serviceName string, baseStore contracts.KVStore) *Adapter
```

NewWithService creates a new securestore Adapter with a custom service namespace.

<a name="Adapter.Close"></a>
### func \(\*Adapter\) [Close](<https://github.com/takoterm/tako/blob/main/pkg/adapter/storage/secure/adapter.go#L63>)

```go
func (a *Adapter) Close() error
```

Close delegates to the base store.

<a name="Adapter.Delete"></a>
### func \(\*Adapter\) [Delete](<https://github.com/takoterm/tako/blob/main/pkg/adapter/storage/secure/adapter.go#L49>)

```go
func (a *Adapter) Delete(key string) error
```

Delete attempts to delete from both the keyring and the base store.

<a name="Adapter.Get"></a>
### func \(\*Adapter\) [Get](<https://github.com/takoterm/tako/blob/main/pkg/adapter/storage/secure/adapter.go#L39>)

```go
func (a *Adapter) Get(key string, ptr any) error
```

Get delegates to the base store.

<a name="Adapter.GetSecret"></a>
### func \(\*Adapter\) [GetSecret](<https://github.com/takoterm/tako/blob/main/pkg/adapter/storage/secure/adapter.go#L77>)

```go
func (a *Adapter) GetSecret(key string, ptr any) error
```

GetSecret retrieves the JSON string from the OS keychain and unmarshals it into ptr.

<a name="Adapter.Has"></a>
### func \(\*Adapter\) [Has](<https://github.com/takoterm/tako/blob/main/pkg/adapter/storage/secure/adapter.go#L55>)

```go
func (a *Adapter) Has(key string) bool
```

Has checks the keyring first, then delegates to the base store.

<a name="Adapter.Set"></a>
### func \(\*Adapter\) [Set](<https://github.com/takoterm/tako/blob/main/pkg/adapter/storage/secure/adapter.go#L44>)

```go
func (a *Adapter) Set(key string, value any) error
```

Set delegates to the base store.

<a name="Adapter.SetSecret"></a>
### func \(\*Adapter\) [SetSecret](<https://github.com/takoterm/tako/blob/main/pkg/adapter/storage/secure/adapter.go#L68>)

```go
func (a *Adapter) SetSecret(key string, val any) error
```

SetSecret marshals the value to JSON and stores it in the OS keychain.

<a name="Provider"></a>
## type [Provider](<https://github.com/takoterm/tako/blob/main/pkg/adapter/storage/secure/provider.go#L10>)

Provider is a Service Provider that automatically wraps the existing KVStore with the OS\-level secure storage adapter.

```go
type Provider struct{}
```

<a name="Provider.Boot"></a>
### func \(\*Provider\) [Boot](<https://github.com/takoterm/tako/blob/main/pkg/adapter/storage/secure/provider.go#L23>)

```go
func (p *Provider) Boot(app *foundation.Application) error
```

Boot wraps the existing KVStore with the secure adapter.

<a name="Provider.Name"></a>
### func \(\*Provider\) [Name](<https://github.com/takoterm/tako/blob/main/pkg/adapter/storage/secure/provider.go#L13>)

```go
func (p *Provider) Name() string
```

Name returns the provider name.

<a name="Provider.Register"></a>
### func \(\*Provider\) [Register](<https://github.com/takoterm/tako/blob/main/pkg/adapter/storage/secure/provider.go#L18>)

```go
func (p *Provider) Register(app *foundation.Application) error
```

Register does nothing for this provider.

Generated by [gomarkdoc](<https://github.com/princjef/gomarkdoc>)
