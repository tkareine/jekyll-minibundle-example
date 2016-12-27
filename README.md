# Jekyll Minibundle example site

An example site using [Jekyll][Jekyll] with
[Minibundle][JekyllMinibundle] plugin to fingerprint CSS and JavaScript
assets. It's deployed to
[GitHub pages][JekyllMinibundleExampleDeployment].

[Sass][Sass] compiles stylesheets written in SCSS syntax into a single
CSS output file. Minibundle includes the output file with `ministamp`
Liquid tag into the generated site. See `_includes/index/head.html`.
This demonstrates how to include the output of an external tool handling
asset compiling, bundling and minifying into Jekyll.

The JavaScript sources in this example demonstrate how to use
Minibundle's `minibundle` Liquid block to feed JavaScript source files
to [UglifyJS2][UglifyJS2] via STDIN, read the minified output from
STDOUT, and include the output in the generated site. See
`_includes/scripts/body.html`. The bundling features of Minibundle are
really that simple, and you probably won't use them much.

## Setup environment

The compile the site, you'll need [Ruby][Ruby] and [Node.js][NodeJs]
programming environments. For Ruby, you'll need [Bundler][Bundler] as
well.

0. `cd jekyll-minibundle-example`
0. `bundle install`
0. `npm install`

## Run the example

First, compile SCSS source files:

``` bash
bundle exec rake sass:compile
```

Alternatively, for more convenient workflow, run Sass in watch mode so
that you'll see the changes in the site when changing SCSS files:

``` bash
bundle exec rake sass:watch
```

Open another terminal for running Jekyll. You have the option to run
Minibundle in development mode or without it. In development mode,
there's no asset fingerprinting and files bundled with `minibundle`
Liquid block appear as is.

To launch Jekyll with Minibundle without development mode:

``` bash
bundle exec rake jekyll:prod
```

Alternatively, to launch Jekyll with Minibundle in development mode:

``` bash
bundle exec rake jekyll:dev
```

You can view the compiled site at
[http://localhost:8080/jekyll-minibundle-example/](http://localhost:8080/jekyll-minibundle-example/).

To see all commands available:

``` bash
bundle exec rake -T
```

[Bundler]: http://bundler.io/
[JekyllMinibundleExampleDeployment]: http://tkareine.org/jekyll-minibundle-example/
[JekyllMinibundle]: https://github.com/tkareine/jekyll-minibundle
[Jekyll]: https://jekyllrb.com/
[NodeJs]: https://nodejs.org/en/
[Ruby]: https://www.ruby-lang.org/en/
[Sass]: http://sass-lang.com/
[UglifyJS2]: https://github.com/mishoo/UglifyJS2
