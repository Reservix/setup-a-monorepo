# Introduction

This guide asumes you are familiar with the term "monorepo". I guess you would not have ended up here otherwise. But for the sake of completeness, here is a quote from wikipedia:

> In revision control systems, a monorepo (syllabic abbreviation of monolithic repository) is a software development strategy where code for many projects are stored in the same repository. As of 2017 this software engineering practice was over a decade old, but had only recently been named.
> 
> [— Wikipedia](https://en.wikipedia.org/wiki/Monorepo)

If you want to know more about the benefits of using a monorepo or why you should not use a monorepo, here is a very short summary:

🍪 Simplified organization (single configuration for linting, building, ...)<br/>
🍪 Simplified dependency management<br/>
🍪 "Atomic changes" (single PR to coordinate changes affecting multiple modules)<br/>
🍪 Enforced architectural boundaries (modules instead of just folders)<br/>
🍪 Better discoverability of related modules<br/>

🍋 May become too large<br/>
🍋 Performance issues (number of commits)<br/>

In order to make lemonade out the aforementioned downsides, just remember that you're not Google, Twitter or any other super large organization. You don't need to have *one* monorepo for your whole company. Rather use monorepos to group things that belong together. A good example for this is [Babel](https://babeljs.io/) or [Atlassian's Atlaskit](https://atlaskit.atlassian.com/). 

Within your company each team can have its own monorepo. This way, you organize your software by domain/context and it is still clear who has responsibility for the code.

> Monolithic source control does not necessarily result in monolithic software.
> 
> [— Markus Oberlehner](https://medium.com/@maoberlehner/monorepos-in-the-wild-33c6eb246cb9)

## More information 

If you want to have morge informations, here are some links:

- [Advantages of monorepos](https://danluu.com/monorepo/)
- [Dependency Hell, Monorepos and beyond](https://www.youtube.com/watch?v=VNqmHJtItCs)
- [Monorepos in the Wild](https://medium.com/@maoberlehner/monorepos-in-the-wild-33c6eb246cb9)
- []()
- [Monorepos: Please don’t!](https://medium.com/@mattklein123/monorepos-please-dont-e9a279be011b)