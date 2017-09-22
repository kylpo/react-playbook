```
._   _       _            
| \ | | ___ | |_ ___  ___
|  \| |/ _ \| __/ _ \/ __|
| |\  | (_) | ||  __/\__ \
|_| \_|\___/ \__\___||___/

```

# React's BSD-3 + License
There has been quite the uproar lately about React's (and other OSS Facebook projects') license agreement. I can see both sides of the argument, but I'm leaning towards the side that will avoid using these projects where possible, unfortunately.

*Update: Looks like Wordpress is [actively removing](https://ma.tt/2017/09/on-react-and-wordpress/) their React usage*

Here are some good reads about it:
- [3 Points to Consider before Migrating Away from React Because of Facebook’s ‘BSD\+ Patent’ License](https://blog.cloudboost.io/3-points-to-consider-before-migrating-away-from-react-because-of-facebooks-bsd-patent-license-b4a32562d268)
- [The Facebook Patent License Punishes You For Suing Facebook, But Lets Them Sue You](http://yehudakatz.com/2017/09/16/facebook-patent-clause-protects-facebook-not-you/)
  - And its associated [twitter thread](https://twitter.com/wycats/status/909134245334999040)
- [If you’re a startup, you should not use React \(reflecting on the BSD \+ patents license\)](https://medium.com/@raulk/if-youre-a-startup-you-should-not-use-react-reflecting-on-the-bsd-patents-license-b049d4a67dd2)
- [An analysis of the licenses used by 75\+ Open Source projects across 35 companies](https://medium.com/@raulk/list-of-companies-and-popular-projects-by-the-open-source-licenses-they-use-35a53eaf1c80)
  - React, Jest, Draft.js, Flow, Immutable.js, and GraphQL reference impl under this license
- [Brain dump: notes and questions arising from “Facebook’s BSD\-3 \+ strong patent retaliation” license](https://medium.com/@raulk/further-notes-and-questions-arising-from-facebooks-bsd-3-strong-patent-retaliation-license-c6386e8e1d60)
- [Explaining React's license \| Engineering Blog \| Facebook Code \| Facebook](https://code.facebook.com/posts/112130496157735/explaining-react-s-license/)
- [The React license for founders and CTOs – James Ide](https://medium.com/@ji/the-react-license-for-founders-and-ctos-b38d2538f3e5)

And some discussions on Twitter:
- "You know what would suck? If I invented a really cool new thing, patented it, implemented it in React, then Facebook decided to copy me"
  - [@thejameskyle](https://twitter.com/thejameskyle/status/898964687303327744)
- "None of this was a surprise tho; everyone was warned (including by me) that this time bomb would eventually go off."
  - [@slightlylate](https://twitter.com/slightlylate/status/898866730000306178)
- "React is dead https://news.ycombinator.com/item?id=15050841"
  - [@levelsio](https://twitter.com/levelsio/status/898770971229700096)
- "Disappointed with Facebook's response to the ASF license issue. They're not the only big company releasing OSS. https://t.co/7YxoA2SMLP"
  - [@slicknet](https://twitter.com/slicknet/status/898728697854808065)
- "The React license allows Facebook to violate patents of companies that use React, and those companies can't sue to stop Facebook"
  - [@feross](https://twitter.com/feross/status/898730336082776064)
- "Ok so I did some patent searching."
  - [@_developit](https://twitter.com/_developit/status/899669300033855490)
- [Jon Kuperman on Twitter: "@mjackson @reactjs C\) Work for a company with an experienced legal team that has decided it isn't worth the risks\."](https://twitter.com/jkup/status/908819995022340096)
  - [Dan Abramov on Twitter: "@jkup @mjackson @sprjrx @reactjs What gets revoked is the separate patent grant\. At which point you’re in the same situation as if it didn’t exist in the first place\."](https://twitter.com/dan_abramov/status/908825390378045446)
- [Rich Harris on Twitter: "@floydophone @lukeed05 @some\_day\_man @dan\_abramov @ThisIsMissEm @facebook uh, wow\. please elaborate on that statement\. Which patents are being infringed?"](https://twitter.com/Rich_Harris/status/908782891982901249)
  - [Pete Hunt on Twitter: "@Rich\_Harris @lukeed05 @some\_day\_man @dan\_abramov @ThisIsMissEm @facebook When I'm back at my computer I'll show you the "download data and render Dom" patent from some random dead Dutch company"](https://twitter.com/floydophone/status/908787530983706626)
  - [Brandon Dail on Twitter: "@Rich\_Harris @floydophone @lukeed05 @some\_day\_man @dan\_abramov @ThisIsMissEm @facebook I \*think\* maybe he means that patents exist for such broad concepts that almost everything is infringing somehow, even if it's unenforceable"](https://twitter.com/aweary/status/908790460130353154)
- [Mattias P Johansson on Twitter: "I don't understand why people are suggesting Vue instead of React b/c of license\. Isn't MIT\-only WORSE than Facebooks license?"](https://twitter.com/mpjme/status/909452245372162049)

@floydophone has been digging up some other patents (point being that we're all screwed if we try to sue any of these big companies):
- [Pete Hunt on Twitter: "Remember everyone: if you are doing responsive web design, you are infringing on a Google patent: https://t\.co/jWHvlvKuK9"](https://twitter.com/floydophone/status/910542586988957697)
- [Pete Hunt on Twitter: "Better hope you aren't preloading JS with your favorite bundler\! It infringes on another Google patent https://t\.co/EGvol1kStJ"](https://twitter.com/floydophone/status/910544627320619008)
- [Pete Hunt on Twitter: "Looks like ever webapp that supports third\-party plugins \(Wordpress etc\) likely violates this Google patent: https://t\.co/TCMKZJGEpM"](https://twitter.com/floydophone/status/910545768410488838)
- [Pete Hunt on Twitter: "You guys thought @tomdale invented progressive enhancement but it was actually Google\. Now pay up\! https://t\.co/Akfl39LNXQ"](https://twitter.com/floydophone/status/910547163637325824)