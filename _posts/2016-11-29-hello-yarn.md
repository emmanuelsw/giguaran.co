---
title:        "Hello, Yarn"
# jekyll-seo-tag
description:  "Yarn is now part of Rails"
author:       "guilleiguaran"
---

Starting from Rails 5.1, [Yarn](https://yarnpkg.com/)
the awesome package manager from JavaScript community will
be part of default Ruby on Rails stack and you can start to use it
instead of having to find wrapper gems for all the front end
libraries of your app (good bye `angularjs-rails`, `jquery-rails`,
`backbone-rails`, etc).

We are going to ship a `package.json` under `vendor` folder so the npm modules
will be downloaded by default to `vendor/node_modules` folder and we have a
`yarn` binstub under `bin` folder (so you can run `bin/yarn add` in
Rails root and it will be run under `vendor`).

Additionally the next version of [Sprockets](https://github.com/rails/sprockets)
is going to support [resolving of npm modules using the "main" property from package.json](https://github.com/rails/sprockets/pull/405) (and ["style" for stylesheets](https://github.com/rails/sprockets/pull/411))
so you can require modules by name, e.g: `//= require jquery`
instead of having to refer to full path, e.g: `//= require
jquery/dist/jquery`.

Thanks to [Liceth](https://github.com/liceth) for all the work she put getting the [Pull
Request for Yarn](https://github.com/rails/rails/pull/26836) ready. Definitely Rails has been yarning for this
since some time ago.
