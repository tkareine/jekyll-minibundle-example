require 'rake/clean'
require 'rubocop/rake_task'

UGLIFYJS_CONFIG = {
  bin: 'node_modules/.bin/uglifyjs'
}.freeze

SASS_CONFIG = {
  common_options: '--scss --sourcemap=none',
  input:          '_assets/styles/site.scss',
  output:         '_tmp/site.css'
}.freeze

DEV_ENV = {
  'JEKYLL_MINIBUNDLE_MODE' => 'development'
}.freeze

directory '_tmp'

CLOBBER.include '_tmp'

namespace :sass do
  desc 'Compile assets with Sass'
  task compile: :_tmp do
    sh %{sass #{SASS_CONFIG.fetch(:common_options)} --style=compressed #{SASS_CONFIG.fetch(:input)} #{SASS_CONFIG.fetch(:output)}}
  end

  desc 'Compile and watch assets with Sass, recompiling when necessary'
  task watch: :_tmp do
    sh %{sass #{SASS_CONFIG.fetch(:common_options)} --watch #{SASS_CONFIG.fetch(:input)}:#{SASS_CONFIG.fetch(:output)}}
  end

  CLOBBER.include '.sass-cache'
end

namespace :uglifyjs do
  desc 'Ensure UglifyJS is installed'
  task :verify do
    File.executable?(UGLIFYJS_CONFIG.fetch(:bin)) || (raise "UglifyJS executable not found: #{UGLIFYJS_CONFIG.fetch(:bin)}\nTry `npm install`.")
  end

  CLOBBER.include 'node_modules'
end

namespace :jekyll do
  desc 'Compile and serve the site locally for development'
  task dev: :clean do
    sh DEV_ENV, %{jekyll serve --watch --trace}
  end

  desc 'Compile and serve the site locally for production'
  task prod: %i{uglifyjs:verify clean} do
    sh %{jekyll serve --watch --trace}
  end
end

desc 'Compile the site for production'
task site: %i{uglifyjs:verify clean sass:compile} do
  sh %{jekyll build}
end

desc 'Compile the site and deploy it to production'
task :deploy do
  sh %{git checkout master}
  sh %{git checkout -B tmp-gh-pages}
  Rake::Task['site'].invoke
  sh %{git add -f _site}
  sh %{git commit -m 'Generated site'}
  sh %{git subtree split --prefix _site -b gh-pages}
  sh %{git push -f origin gh-pages}
  sh %{git branch -D gh-pages}
  sh %{git checkout master}
end

CLEAN.include '_site'

RuboCop::RakeTask.new

task default: :site
