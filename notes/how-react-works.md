```
._   _       _            
| \ | | ___ | |_ ___  ___
|  \| |/ _ \| __/ _ \/ __|
| |\  | (_) | ||  __/\__ \
|_| \_|\___/ \__\___||___/

```

# React Core understanding
jsx converts
```js
<div height={5} >
  <Child />
</div>
```
into
```js
React.createElement('div', {height:5},
  React.createElement(Child, {})
)
```
`React.createElement()` returns a `reactElement`, which could be a single node, or tree of nodes.

Note: this means even if the parent div did not render Child, React will still create the Element. This is an argument for using Child Functions for conditionally rendering components. See the Usage section of [react-media](https://github.com/ReactTraining/react-media#usage) for more.

# Lifecycle Methods
![](https://github.com/kylpo/react-playbook/blob/master/assets/lifecycle.png?raw=true)
*from [this](https://gist.github.com/chris-martin/2424924e7a7494d5f98c) gist*