---
title:        "Adventures with Webpack and Rails"
# jekyll-seo-tag
description:  "Webpack as replacement for Sprockets"
author:       "guilleiguaran"
---

**Disclaimer**: This is not a introductorial post about Webpack or about
using Webpack with Rails, if you need that I would recommend you read the
[awesome post by Samuel Mullen about this topic](http://pixelatedworks.com/articles/replacing-the-rails-asset-pipeline-with-webpack-and-yarn/).

I started to play with Webpack a couple of days ago exploring it as
alternative to Sprockets in the upcoming version of Rails (5.1),
with that on mind I wanted to check how far is possible to go
integrating Rails with Webpack without having to change
current Rails conventions (e.g folder structure, routes for assets,
support for sass/coffee, etc), after of checking many integration
projects ([react_on_rails](https://github.com/shakacode/react_on_rails),
[webpack-rails](https://github.com/mipearson/webpack-rails), etc) and based in the
configuration suggested by Samuel Mullen in his [blog post](http://pixelatedworks.com/articles/replacing-the-rails-asset-pipeline-with-webpack-and-yarn/) and many (so many!)
other blog posts I was able to get my fork of the TodoMVC app (forked from the
[Rails + Angular.js + Yarn + Babel version of TodoMVC by Liceth](https://github.com/Liceth/Todo)) working:

[https://github.com/guilleiguaran/Todo](https://github.com/guilleiguaran/Todo)

You can check the [history of commits](https://github.com/guilleiguaran/Todo/commits/master)
to see how everything was done but I want to leave some additional
notes about the process here:

- Yarn is used for dependencies, that was already done in the
[demo](https://github.com/Liceth/Todo) that I used as based and it is a
upcoming feature in Rails 5.1 ([Pull Request](https://github.com/rails/rails/pull/26836)) thanks to
[Liceth](https://github.com/Liceth)!!

- I was able to keep standard Rails folder structure
(`app/assets/{javascripts,stylesheets,images,...}`, `public/assets`)

- Using loaders I added support for Sass, CoffeeScript (for compatibility
with our current standards) and Babel (TheFuture™ is now).

- I used [ExtractTextPlugin](https://github.com/webpack/extract-text-webpack-plugin)
to deal with CSS, in the example is not used for fonts/images inside of
CSS but I tested it and worked successfully, one thing that I see was that
during development I had to refer to images/fonts using relative paths
(e.g inside of `app/assets/stylesheets/application`.css I had `url
(../images/rails.png)` to get the image living under `app/assets/images/rails.png`),
I need to research more about this.

- I added a [preloader for erb files](https://github.com/usabilityhub/rails-erb-loader)
for the erb files that are part of the asset pipeline, I forked it and
changed it to use Erubis instead of the stdlib ERB. (Thanks to [rhys-vdw](https://github.com/rhys-vdw)
for merging the [Pull Request](https://github.com/usabilityhub/rails-erb-loader/pull/7))
and releasing a new version so fast!!)

- I moved `application.css` to `application.scss` to be able to load external
stylesheets using the sass `@import`.

- Moved the entire app from globals to modules, it wasn't so hard because
the app was pretty small but I can imagine that this would be a hard work
in bigger applications not based in modules right now.

- I disabled Sprockets entirely, so I don't have to deal with problems like
double-fingerprinting. In new apps this is easier to do passing the
`--skip-sprockets` option to `rails new` command.

- Generated a manifest using [webpack-manifest-plugin](https://github.com/danethurber/webpack-manifest-plugin)

- I wrote a [small helper](https://github.com/guilleiguaran/Todo/blob/master/config/initializers/assets.rb#L1-L28)
to get manifest working with current Rails view helpers (`javascript_include_tag`,
`stylesheet_link_tag`, `image_tag`, etc)

- I was able to run the app (webpack + rails) with one single command in two ways:

  1. Using `webpack-dev-server` to run `bin/rails` as [child process](https://github.com/guilleiguaran/Todo/blob/master/webpack.config.js#L1-L8) and [proxying requests to Rails app](https://github.com/guilleiguaran/Todo/blob/master/webpack.config.js#L43-L48), I'm just assuming that Rails app is running in the standard `http://localhost:3000` to make things easier.

  2. Using `rails` to run `webpack` (as child process) in watch mode and emitting the assets to `tmp/webpack` in dev mode and using `ActionDispatch::Static` to serve the assets. This is all done in one [initializer](https://github.com/guilleiguaran/Todo/blob/master/config/initializers/assets.rb#L30-L35)

- I discovered that Webpack 2 is almost ready (looks like only the docs
 are stopping the release of the new version), it has so [many exciting
new features](https://gist.github.com/sokra/27b24881210b56bbaff7) and that
probably I should be using that version, so I'll work later in the upgrade
to Webpack 2 beta.

I would continue checking what else can be improved and I'll update this
post with my new findings.

Thanks to [Liceth Ovalles](https://github.com/Liceth) for the example app used as base,
to [Samuel Mullen](https://github.com/samullen) for the base Webpack configuration, and to
[Fernando Montoya](https://github.com/montogeek), [Richard Roncancio](https://github.com/batusai513),
[Jonathan Alvarez](https://github.com/jonalvarezz) and the other folks
in #javascript channel from Colombia-dev that used part of their time
to check and help me to improve the Webpack configuration.
