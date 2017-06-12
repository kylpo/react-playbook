# Spacing Components

First, some clarifications:
- __Margin__ is a css property for spacing __outside__ of a component. A div with a yellow background and margin __will not__ see the yellow background in the margin spacing.
- __Padding__ is a css property for spacing __inside__ of a component. A div with a red background and padding __will__ see the red background in the padding spacing.

So, when building a component, how should the two properties be used?

Well, for the most part, components are a powerful construct because they only care about themselves, and are not concerned about the world around them. So, `padding` should be used for all spacing needs. This maximizes reusability and separates their concerns from the world outside them.

But in the real world, we do need spacing outside of components for composing them into pages and scenes. And this is where `margin` creeps into component code: for spacing compositions of components.

Should `margin` really be the concern of a component though? Given my `<Button />` component, should it really accept a `marginTop` or `marginLeft` prop? How about on a bigger scale? Should __ALL__ of my composite components accept these `margin` props? That doesn't seem scalable.

A better solution might be to wrap my Button component with another component whose sole purpose is to apply margin. Something like:
```jsx
<Margin top={20}>
  <Button ... />
</Margin>
```

Nice! We have a clear separation here, and we haven't muddied our Button API.

This Margin component visually breaks down though (yes, it is a totally contrived example):
```jsx
<Button ... />
<Margin top={20}>
  <Button ... />
</Margin>
<Margin bottom={20}>
  <Button ... />
</Margin>
```

The above button group is pretty difficult to skim and understand the spacing. We can do better!

```jsx
<Button ... />
<Space size={20} />
<Button ... />
<Button ... />
<Space size={20} />
```

And this is what I've settled on using for page/scene spacing: the `<Space />` component. `Space` accepts a single `size` prop, which is used to set its `flex-basis`. Note: I can get away with only using `flex-basis` because I use flexbox exclusively, in React Native __and__ web.

Conveniently, this ends the debate of [margin-bottom or margin-top](https://css-tricks.com/margin-bottom-margin-top/) for my code.

## Try it
Feel free to try `constelation-space` on web and native, or make your own

## Credits
- props to [kilvin/Spacer.js](https://github.com/rofrischmann/kilvin/blob/master/modules/components/Spacer.js) for using `flex` css attribute and a `size` prop.
- Android Studio actually has a [Space](https://developer.android.com/reference/android/widget/Space.html) widget in their wysiwyg editor

---

Note: I publish these to learn from your responses! Please let me know if you have any thoughts on the subject.

--------

### Below are the random notes taken in writing ^ post. Don't bother reading them - they're for me.

So, I have chosen to replace all `margin` usage with a new `<Space />` component. This Space component accepts one prop: `size`.
In adding `margin` into our components, haven't we just broken the

Any spacing related within the bounds of the outer-most `div` of your component
 Particularly useful for layout of an entire page, where components need to have a certain spacing between them or the viewport.

Well, for me, a component is only concerned with itself

In my React Native projects, I noticed something strange about my `padding` and `margin` usage in components: they were mostly used interchangeably. This is because flexbox, the layout React Native uses, does not honor margin collapsing. If a `View` (`div` in the web world) has `marginBottom={20}` and a `View` below it has `marginTop={20}`, the resulting space between the two is `40`, not `20`. So, in a sense, `margin` is the same as `padding` _within_ components.

Recently, I noticed my code base consisted of inconsistent `margin` and `padding` usage.

Where is Margin still useful?
- If you aren't using flexbox everywhere, and would like to levarage margin-collapsing


margin vs padding (future blog post)
- Margin used solely for layout between components? Padding used solely for layout within components?
- can I have some <Margin /> component that doesn't add an extra div/View and be used specifically for scene layouts?
  - a Margin component would solve my problem with needing to wrap Text with a View just to add some padding between words, too

Since I currently use flexbox for all of my layouts, web and native, margin does not collapse and has been used almost interchangeably with padding.

Consider a template-only primitive for margin, maybe named `<Spacer />` as an alternative to requiring all of your composite components to accept a `margin` prop

When thinking about component design, I find it helps to imagine putting your components in a wysiwyg tool, where a user would visually be able to create components. To add space between two components, like two buttons or icons, would you expect them to select one and apply a `paddingRight`? Or add some new `Spacer` component? I think they'd rather user `Spacer`.
- Android Studio actually has a `Space` widget
![](https://2.bp.blogspot.com/-jSZ8PNpvBiA/WLhTRsb9WeI/AAAAAAAAD8M/Y51t1L6PeNYkYzlIljJglYNjIwtM6a6UwCLcB/s1600/Screen%2BShot%2B2017-03-02%2Bat%2B9.14.58%2BAM.png)
- [native-navigation/navigator-spacer.md at master · airbnb/native-navigation](https://github.com/airbnb/native-navigation/blob/master/docs/api/navigator-spacer.md)
