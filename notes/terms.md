```
._   _       _            
| \ | | ___ | |_ ___  ___
|  \| |/ _ \| __/ _ \/ __|
| |\  | (_) | ||  __/\__ \
|_| \_|\___/ \__\___||___/

```

# Terms
- __Relay__ is a data fetcher and manager.
- __Redux__ is an (app) state manager (using pure functions).
- __MobX__ is an (app) state manager (using observables).
- __Higher Order Component__ is a *function* that *takes a component* and *returns a new component*
- __immutable component__ is a component that, after rendering once, can **not** re-render.
- __injector component__ is a component that takes props, optionally computes new ones, then injects them into its child via `React.cloneElement()`.
- __null component__ is a component that returns `null`, and likely uses lifecycle hooks to make side effects.