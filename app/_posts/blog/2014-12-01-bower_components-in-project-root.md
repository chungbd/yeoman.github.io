---
layout: blog
title: Why Bower_Components was moved to a Project's Root Directory
author: Eddie Monge Jr
---

> this is a post by team member [Eddie Monge Jr](https://github.com/eddiemonge)

**tl;dr** Because I said so. lol j/k. `/app` should be for your files, not third party libraries.

There has been a few questions and discussions regarding the decision to move the **bower_components** folder outside of the `/app` folder.

Matija ([silvenon](https://github.com/silvenon)) summarized some of the thoughts nicely:

1. I want the `/app` folder to be for user-generated stuff
1. As external dependencies, I feel they are better located externally. Next to node_modules
1. It's hard to run any command in the terminal which goes through all files, (like tree), because you always have to ignore bower_components

Here are those reasons point by point with some explanations.

## User Generated Files

> I want the `/app` folder to be for user-generated stuff

There are people who edit files in the `/app` folder then wonder why their changes disappear when they do a `bower update/install`. Not just beginners but also people who should know better. They think "oh its in the `/app` folder so I can freely edit it without worrying about it". That was the main expectation this change wanted to make true.

## External Dependencies

> As external dependencies, I feel they are better located externally. Next to node_modules

This was the single biggest reason for this change. `/app` should be for the *user* web app/site files, emphasis on **user**. It shouldn't be for external or third-party files. It is the responsibility of the tool authors to make having the files located elsewhere completely painless (and even transparent) to end users. Make the tools deal with the complexity, and give the users an easy workflow.

There is already a paradigm of this existing in Ruby on Rails. Not saying RoR is a good example to follow, but it is popular enough to say its what the community expects to see. It has also been in the [Angular generator](https://github.com/yeoman/generator-angular) for awhile now and there haven't been problems or complaints about it for a long time. Some people try to PR (pull request) moving it into the `/app` folder because they think it is supposed to be there but when pointed out how it works, they generally seem to be happy with the change.

## Tooling

> It's hard to run any command in the terminal which goes through all files, (like tree), because you always have to ignore bower_components

It is easier to do a global search in the `/app` folder when it isn't cluttered with external libraries. Although a counter to that is:

> If your command line tools give you data you don't need I'd say you should use them properly - [Peter Müller](https://github.com/Munter)

Which is a valid point. It is easy enough to set up a filter to exclude those files. But this kind of goes with the point that tools should get out of the way of the user. Bower is another tool and having it put its files in the users files is not getting out of the way, it's adding complexity.


## Downsides

Nothing is perfect (well almost nothing) so some downsides should be mentioned.

1. Requires a build/serve system to work (like the one generated by the official Yeoman generators).

  Counter argument: if Sass/Coffee/Autoprefixer/whatever else is needed then a build system is probably required anyway. If not, or if a pure flat site is wanted, then `grunt/gulp/whatever build` the site and use the outputted files. Yeoman is supposed to be a starting point, not the be-all-end-all build system/workflow.

1. Requires more configuration complexity.

  Counter argument: Doesn't Sass require some setup? Doesn't asset-graph require some complexity? Complexity in the initial setup isn't bad because once configured, it usually doesn't change much or often later. The tools could also be made to be smarted about figuring things out so configuration might not even be required (convention over configuration).

## Conclusion

Hopefully some compelling reasons have been provided for the change and persuaded some, if not all, nay-sayers as to the wisdom of the change. If not, well this change isn't the only way to do things. If an individual generator wanted to do things differently thats acceptable, if not encouraged. Trying new things is the way change happens and can sometimes lead to better ways of doing things. Sometimes trying a different way will lead you back to the original way and further reinforce why it was that way to begin with.
