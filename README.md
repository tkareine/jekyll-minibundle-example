# Jekyll Minibundle example site

An example site using [Jekyll] with [Minibundle][JekyllMinibundle]
plugin to fingerprint CSS and JavaScript assets. It's deployed with
[GitHub Pages][GitHubPages] at
[https://tkareine.github.io/jekyll-minibundle-example/](https://tkareine.github.io/jekyll-minibundle-example/).

[Sass] compiles stylesheets written in SCSS syntax into a single CSS
output file. Minibundle includes the output file of Sass with the
`ministamp` Liquid tag into the generated site. The path of the output
file includes the MD5 fingerprint of the file's contents. See
[_includes/index/head.html](_includes/index/head.html). This
demonstrates how to include the output of an external tool handling
asset compiling, bundling and minifying into Jekyll. The same mechanism
works with JavaScript compilation tools, such as [Browserify] and
[webpack], too. You can use the `ministamp` tag to fingerprint any type
of file.

The JavaScript sources in this example demonstrate how to use
Minibundle's `minibundle` Liquid block to feed JavaScript source files
to [UglifyJS][UglifyJS2] via stdin, read the minified output from
stdout, and include the output file in the generated site. The path of
the output file includes the MD5 fingerprint of the file's contents. See
[_includes/scripts/body.html](_includes/scripts/body.html). The bundling
features of `minibundle` are really that lightweight, but sometimes
that's just enough.

## Setup environment

The compile the site, you'll need [Ruby] and [Node.js][NodeJs]
programming environments. For Ruby, you'll need [Bundler] as well.

1. `cd jekyll-minibundle-example`
2. `bundle install`
3. `npm ci`

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
bundle exec rake jekyll:watch:prod
```

Alternatively, to launch Jekyll with Minibundle in development mode:

``` bash
bundle exec rake jekyll:watch:dev
```

You can view the compiled site at
[http://localhost:8080/jekyll-minibundle-example/](http://localhost:8080/jekyll-minibundle-example/).

To see all the commands available:

``` bash
bundle exec rake -D
```

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/80x15.png)][CC0] This work
is dedicated into public domain.

[Browserify]: http://browserify.org/
[Bundler]: http://bundler.io/
[CC0]: https://creativecommons.org/publicdomain/zero/1.0/
[GitHubPages]: https://docs.github.com/en/pages
[JekyllMinibundle]: https://github.com/tkareine/jekyll-minibundle
[Jekyll]: https://jekyllrb.com/
[NodeJs]: https://nodejs.org/en/
[Ruby]: https://www.ruby-lang.org/en/
[Sass]: http://sass-lang.com/
[UglifyJS2]: https://github.com/mishoo/UglifyJS2
[webpack]: https://webpack.github.io/
