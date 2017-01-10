---
title:        "Adventures with Webpack and Rails Part. II"
# jekyll-seo-tag
description:  "Webpack as complement for Sprockets"
author:       "guilleiguaran"
---

So as many of you might now, Rails have been adopted officially Webpack
for Rails 5.1 (with the --webpack option) through the [Webpacker gem](https://github.com/rails/webpacker),
most of the decisions in the integration were based in the exploration work made
by DHH and me separately, being the most important difference that
DHH kept Sprockets for CSS/images/fonts/etc.

Some of the things we changed since [my last post about Webpack](http://giguaran.co/2016/11/19/adventures-with-webpack/) are:

- [Yarn is now a default](https://github.com/rails/rails/pull/27300) in Rails 5.1 apps!!

- Files processed by Webpacker live under `app/javascripts` and entry
  points under `app/javascripts/packs`

- CSS/Sass/fonts/images still supported through Sprockets and can be
  used by Webpacker through ERB.

- We have migrated to [Webpack2 RC](https://medium.com/webpack/webpack-2-2-the-release-candidate-2e614d05d75f#.80ptside5) enabling [Tree-Shaking](https://medium.com/modus-create-front-end-development/webpack-2-tree-shaking-configuration-9f1de90f3233#.g81y519s7).

- We have added [support for ES2015+](https://github.com/rails/webpacker/commit/af320768eb5dfe82952238468021aea308a048d8) (2015/2016/2017) through Babel.

- The [ERB Loader](https://github.com/rails/webpacker/pull/19) is included by default.

- Webpack (in watcher mode) should be run as a separated process.

We will continue working hard in Webpacker during the next months to bring the best
from JavaScript world to Rails developers.

[Ping me on Twitter](https://twitter.com/guilleiguaran) if your applications are already using Webpacker or
if you have any feedback about it.
