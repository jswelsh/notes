[Seven Interesting Things About React Native
](https://react.christmas/2020/1)

[War stories: The move from Flow to TypeScript](https://react.christmas/2020/2)
  flow support is potentially waning, this is obviously opinionated but 
      
  "During November 2020 11 authors have pushed    44 commits to Flow’s master branch and merged 0 pull-requests, compared to 23 authors and 100 commits to TypeScript’s master branch with 85 merged pull requests."

  the author notes serious anti-patterns used in using specific extensions for web vs native index.native.js vs index.web.js, they were instead written as native.js and web.js respectively

  They highlight a serious issue with any codebase of a significant size being unused code. ESLint can detect if a variable or method is unused but this becomes impossible the moment you involve internal imports/exports of files. This can be resolved with **[js-unused-exports!!](https://github.com/devbridge/js-unused-exports)**

**Choosing a strategy for conversion**

Big bang; nothing works until everything works
vs
Hybrid; two type systems exist simultaneously

complexity exists in the build or in the application,
transition phase is either long and non-blocking on releases or short, intense and blocking until all is done.

Big bang
- low complexity in the build
- high complexity in the application
- transition is short but very intensive
- everything needs to be working or none of it will

Hybrid
- high complexity in the build
- low complexity in the application
- transition phase is slow, ready as you go
- releases are non-blocking and allows for steady flow of releases

To do hybrid you need to configure babel to include presets for .ts and .tsx. You also need to get both the type systems to talk to each other, this is done through manually bridging the two with declaration files. Not hard, just very time consuming.

**"You have now erected a iron curtain across your application, and every time you jump from one side to the other your risk of getting shot in the face."**

the problem with Hybrid, the scope creeps, it creeps hard and fast.

"Kahn Academy saved me an unbelievable amount of work with their [flow-to-ts](https://github.com/Khan/flow-to-ts) utility"

when transitioning from Flow to Typescript, no refactoring should be done. The code should be as close to the origin code as possible, **only** changing the type system 