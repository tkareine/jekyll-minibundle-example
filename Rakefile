require 'rake/clean'

UGLIFYJS_CONFIG = {
  bin: 'node_modules/.bin/uglifyjs'
}

SASS_CONFIG = {
  input: '_assets/styles/site.scss',
  output: '_tmp/site.css',
}

DEV_ENV = {
  'JEKYLL_MINIBUNDLE_MODE' => 'development'
}

directory '_tmp'

CLOBBER.include '_tmp'

namespace :sass do
  desc 'Compile assets with Sass'
  task :compile => :_tmp do
    sh %{sass --scss --sourcemap=none --style=compressed #{SASS_CONFIG.fetch(:input)} #{SASS_CONFIG.fetch(:output)}}
  end

  desc 'Compile and watch assets with Sass, recompiling when necessary'
  task :watch => :_tmp do
    sh %{sass --scss --sourcemap=none --watch #{SASS_CONFIG.fetch(:input)}:#{SASS_CONFIG.fetch(:output)}}
  end

  CLOBBER.include '.sass-cache'
end

namespace :uglifyjs do
  desc 'Ensure UglifyJS is installed'
  task :verify do
    File.executable?(UGLIFYJS_CONFIG.fetch(:bin)) or fail "UglifyJS executable not found: #{UGLIFYJS_CONFIG.fetch(:bin)}\nTry `npm install`."
  end

  CLOBBER.include 'node_modules'
end

namespace :jekyll do
  desc 'Compile and serve the site locally for development'
  task :dev => :clean do
    sh DEV_ENV, %{jekyll serve --watch --trace}
  end

  desc 'Compile and serve the site locally for production'
  task :prod => [:'uglifyjs:verify', :clean] do
    sh %{jekyll serve --watch --trace}
  end
end

desc 'Compile the site for production'
task :site => [:'uglifyjs:verify', :clean, :'sass:compile'] do
  sh %{jekyll build}
end

CLEAN.include '_site'

task :default => :site
