---
seo:
  title: TAKO - The missing framework for Go terminal apps.
  description: A structured foundation for Go terminal applications. It provides dependency injection, an event bus, and a plugin system, while staying completely UI-agnostic.
---

::u-page-hero{.hero-uneven}
---
orientation: horizontal
reverse: true
---
#headline
::div{.flex.items-center.gap-3.md:block}
  <img src="/logo.webp" alt="Logo" class="w-16 h-16 md:hidden" />

  **TAKO**{.text-2xl.md:text-3xl.font-black.text-primary.tracking-widest.uppercase}
::

#title
The missing framework for Go terminal apps.

#description
A structured foundation for Go terminal applications. It provides dependency injection, an event bus, and a plugin system, while staying completely UI-agnostic.

#links
  :::u-button
  ---
  color: primary
  variant: outline
  size: xl
  to: /getting-started/installation
  trailing-icon: i-lucide-arrow-right
  ---
  Get Started
  :::

  :::u-button
  ---
  color: neutral
  variant: outline
  size: xl
  to: https://github.com/takoterm/tako
  ---
  View on GitHub
  :::

#default
  ::div{.w-full.hidden.md:flex.justify-center.items-center.min-h-[400px]}
    <img src="/logo.webp" alt="Logo" class="w-[450px] h-auto animate-float" />
  ::
::
 
::div{.max-w-5xl.mx-auto.mb-8.md:mb-12.px-4}
  :::div{.grid.grid-cols-2.md:grid-cols-4.divide-y-0.md:divide-y-0.md:divide-x.divide-gray-200.dark:divide-gray-800.text-center.mb-8}
    ::::div{.flex.flex-col.items-center.gap-1.py-4.px-2}
      :icon{name="i-lucide-terminal" class="w-6 h-6 text-primary mb-2"}
      <div class="font-bold text-lg">Go 1.26+</div>
      <div class="text-[10px] text-gray-500 dark:text-gray-400 tracking-widest uppercase mt-1">Minimum Go Version</div>
    ::::
    ::::div{.flex.flex-col.items-center.gap-1.py-4.px-2}
      :icon{name="i-lucide-feather" class="w-6 h-6 text-primary mb-2"}
      <div class="font-bold text-lg">Minimal</div>
      <div class="text-[10px] text-gray-500 dark:text-gray-400 tracking-widest uppercase mt-1">Dependencies</div>
    ::::
    ::::div{.flex.flex-col.items-center.gap-1.py-4.px-2}
      :icon{name="i-lucide-puzzle" class="w-6 h-6 text-primary mb-2"}
      <div class="font-bold text-lg">100%</div>
      <div class="text-[10px] text-gray-500 dark:text-gray-400 tracking-widest uppercase mt-1">UI Library Agnostic</div>
    ::::
    ::::div{.flex.flex-col.items-center.gap-1.py-4.px-2}
      :icon{name="i-lucide-file-badge" class="w-6 h-6 text-primary mb-2"}
      <div class="font-bold text-lg">MIT</div>
      <div class="text-[10px] text-gray-500 dark:text-gray-400 tracking-widest uppercase mt-1">Open-Source License</div>
    ::::
  :::

  <p class="text-center text-sm text-gray-600 dark:text-gray-400 max-w-2xl mx-auto">
    A structured framework for building production-grade terminal applications in Go — without the global state, spaghetti keybindings, or renderer lock-in.
  </p>
::

::LandingHowItWorks
::

::LandingDivider
::

::LandingCorePrimitives
::

::LandingDivider
::

::LandingDualKernel
::

::LandingDivider
::

::LandingPluginSystem
#manifest
```go
func (p *Plugin) Register() tako.Manifest {
  return tako.Manifest{
    Name: "my-plugin",
    Deps: []string{"logger"},
  }
}
```

#boot
```go
func init() {
  tako.RegisterPlugin(&Plugin{})
}
```

#logic
```go
func (p *Plugin) Boot(app tako.App) {
  app.EventBus().On("search", p.onSearch)
}
```
::

::LandingDivider
::

::LandingArchitectureComparison
::

::LandingDivider
::

::LandingGetStarted
#code
```go
package main

import (
  "log"

  "github.com/takoterm/tako"
  "github.com/takoterm/tako/contracts"
  "github.com/takoterm/tako/pkg/adapter/bubbletea"
)

func main() {
  app := tako.NewApp()

  app.Keys().Bind("ctrl+c", func() {
    app.Shutdown()
  })

  // Use the official Bubble Tea adapter — the most popular
  // Go TUI library is supported out of the box.
  app.Container().Singleton(
    new(contracts.UIRenderer),
    bubbletea.NewAdapter(app.Context(), app.EventBus(), app.Router(), &myLayout{}),
  )

  if err := tako.Run(app); err != nil {
    log.Fatalf("tako: %v", err)
  }
}
```
::

::LandingDivider
::

::LandingFaq
::

::LandingDivider
::

::LandingDirectory
::