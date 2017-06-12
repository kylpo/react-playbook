# A Naming Convention for Null Components
## What is a Null Component?

```jsx
render() {
  // Potentially do logic

  return null
}
```
(I got the name from this oooold issue: https://github.com/facebook/react/issues/1058)

## Can You Spot Them?
Can you tell me which of these components renders nothing to the DOM?

```jsx
import Redirct from 'react-router'

<View>
  <Redirect />
</View>
```

I can't either. I'd need prior knowledge of the components or read through how they were implemented.

If you are familiar with React Router V4, you may know that it doesn't render anything, but what if you could know without prior knowledge?

## Introducing the `<_RendersNull_ />` Convention

With this naming convention, you quickly see exactly what renders nothing. Even better, you can use tooling to better highlight it. My vim config highlights these a different color.


Now, which of these components renders nothing in the DOM?


```jsx
<View>
  <Style_>
    <View />
  </Style_>

  <_Redirect_ />
</View>
```

## It Gets Better

## And better tooling

## Other naming conventions
Link to other naming conventions
