```
._   _       _            
| \ | | ___ | |_ ___  ___
|  \| |/ _ \| __/ _ \/ __|
| |\  | (_) | ||  __/\__ \
|_| \_|\___/ \__\___||___/

```

# Flow
- [Flow type cheat sheet - SaltyCrane Blog](http://www.saltycrane.com/blog/2016/06/flow-type-cheat-sheet/)
- Pro-tip: Put your @flowtype libdefs in a dir called "flow-typed" in the same dir as your .flowconfig. No need for any [libs] configuration! - [tweet](https://twitter.com/lbljeffmo/status/752705329180409856)
- `console` flow-type: https://github.com/jeffmo/jest/blob/fef19c897fcf646e66a7cd6120b4b743846159b5/flow-typed/console.js
- [Did you know you can use capture groups in #flowtype's name_mapper option? E.g. 'mylib/\(.*\)' -> '<PROJECT_ROOT>/dist/lib/\1' .. NEAT!](https://twitter.com/ryyppy/status/787979980714209280)
- How to read error messages: "Basically every error has two parts. They are related. Since Flow is about asserting how types "flow" into each other, an error may happen on one of two sides." - [abramov](https://twitter.com/dan_abramov/status/813794628575068160)

# Style
## `Array<Foo>`, not `Foo[]`
### Why?
- Because when you inevitably need to make it a union, `Array<Foo | Bar>` works but `Foo | Bar[]` doesn't
- Also, what if you had a set? `Set<Foo>`
- See https://twitter.com/spikebrehm/status/832675779070750720
