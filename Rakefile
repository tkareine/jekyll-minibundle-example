# frozen_string_literal: true

require 'rake/clean'
require 'rubocop/rake_task'
require 'open3'

def sh_capture2(cmd)
  stdout_str, status = Open3.capture2(cmd)

  if status.exitstatus != 0
    raise "sh_capture2 failed (#{status.exitstatus}): #{cmd}"
  end

  stdout_str
end

UGLIFYJS_CONFIG = {
  bin: 'node_modules/.bin/uglifyjs'
}.freeze

SASS_CONFIG = {
  common_options: '--no-source-map',
  input:          '_assets/styles/site.scss',
  output:         '_tmp/site.css'
}.freeze

DEV_ENV = {
  'JEKYLL_MINIBUNDLE_MODE' => 'development'
}.freeze

directory '_tmp'

CLEAN.include '_tmp'

namespace :sass do
  desc 'Compile assets with Sass'
  task compile: :_tmp do
    sh "sass #{SASS_CONFIG.fetch(:common_options)} --style=compressed #{SASS_CONFIG.fetch(:input)} #{SASS_CONFIG.fetch(:output)}"
  end

  desc 'Compile and watch assets with Sass, recompiling when necessary'
  task watch: :_tmp do
    sh "sass #{SASS_CONFIG.fetch(:common_options)} --watch #{SASS_CONFIG.fetch(:input)}:#{SASS_CONFIG.fetch(:output)}"
  end

  CLEAN.include SASS_CONFIG.fetch(:output)
end

namespace :uglifyjs do
  desc 'Ensure UglifyJS is installed'
  task :verify do
    File.executable?(UGLIFYJS_CONFIG.fetch(:bin)) || (raise "UglifyJS executable not found: #{UGLIFYJS_CONFIG.fetch(:bin)}\nTry `npm ci`.")
  end

  CLOBBER.include 'node_modules'
end

namespace :jekyll do
  desc 'Compile the site (prod env)'
  task :compile do
    sh 'jekyll build'
  end

  namespace :watch do
    desc 'Compile, watch, and serve the site locally (dev env)'
    task :dev do
      sh DEV_ENV, 'jekyll serve --watch --trace'
    end

    desc 'Compile, watch, and serve the site locally (prod env)'
    task :prod do
      sh 'jekyll serve --watch --trace'
    end
  end

  CLOBBER.include '_site', '.jekyll-cache'
end

desc 'Compile the site (prod env)'
task site: %i[clean uglifyjs:verify sass:compile jekyll:compile]

desc 'Compile the site and deploy it to production'
task :deploy do
  Rake::Task['site'].invoke
  sh 'git add -f _site'
  git_tree_id = sh_capture2('git write-tree --prefix=_site/').lines(chomp: true).first
  git_commit_id = sh_capture2("git commit-tree -m 'Generated site' '#{git_tree_id}'").lines(chomp: true).first
  sh "git update-ref refs/heads/gh-pages '#{git_commit_id}'"
  sh 'git reset'
  sh 'git push -f origin gh-pages'
end

RuboCop::RakeTask.new

task default: :site
