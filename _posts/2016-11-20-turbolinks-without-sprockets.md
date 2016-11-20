---
title:        "Using Turbolinks 5 (from npm) without Sprockets"
# jekyll-seo-tag
description:  "Using Turbolinks 5 from npm without Sprockets"
author:       "guilleiguaran"
---

As you might know, I'm evaluating to [Webpack as alternative to
Sprockets](http://giguaran.co/2016/11/19/adventures-with-webpack/)
in upcoming Rails version and I'm trying to get everything
working for existing and new apps including existing assets gem.

One of the assets gem shipped by default in new Rails applications is
[Turbolinks](https://github.com/turbolinks/turbolinks). Turbolinks
JavaScript library can be obtained as [npm package](https://www.npmjs.com/package/turbolinks)
but you still need the [turbolinks gem](https://github.com/turbolinks/turbolinks-rails)
to have `redirect_to` method in Rails controllers working with Turbolinks.

Trying to use turbolinks gem without Sprockets will give you this error:

```
rescue in block (2 levels) in require': There was an error while trying to load the gem 'turbolinks'. (Bundler::GemRequireError)
Gem Load Error is: undefined method `assets' for #<Rails::Engine::Configuration:0x007fcf51d04350>
```

This error is caused by [Turbolinks::Engine trying to append turbolinks
asset path to Sprockets paths](https://github.com/turbolinks/turbolinks-rails/blob/master/lib/turbolinks.rb#L17)
and it fails since `config.assets` doesn't exist when Sprockets is
disabled.

As workaround is possible to disable autoloading of turbolinks gem in
the Gemfile with the `require: false` option:

```
gem "turbolinks", "~> 5.0", require: false
```

And then including `Turbolinks::Redirection` manually in the
ApplicationController of the app:

```
require "turbolinks/redirection"

class ApplicationController < ActionController::Base
  # (...)
  include Turbolinks::Redirection
  # (...)
```

and that's all!!

I've sent a [pull request](https://github.com/turbolinks/turbolinks-rails/pull/18)
to fix this and hopefully this note is useful for everyone having a problem similar to this.
