exec(*(["bundle", "exec", $PROGRAM_NAME] + ARGV)) if ENV['BUNDLE_GEMFILE'].nil?

task :default => :test

begin
  Bundler.setup(:default, :development)
rescue Bundler::BundlerError => e
  $stderr.puts e.message
  $stderr.puts "Run `bundle install` to install missing gems"
  exit e.status_code
end

spec = Bundler.load_gemspec("orm_adapter-activestash.gemspec")
require "rubygems/package_task"

Gem::PackageTask.new(spec) { |pkg| }

namespace :gem do
  desc "Push all freshly-built gems to RubyGems"
  task :push do
    Rake::Task.tasks.select { |t| t.name =~ %r{^pkg/#{spec.name}-.*\.gem} && t.already_invoked }.each do |pkgtask|
      sh "gem", "push", pkgtask.name
    end
  end
end

task :release do
  sh "git release"
end

require 'yard'

YARD::Rake::YardocTask.new :doc do |yardoc|
  yardoc.files = %w{lib/**/*.rb - README.md}
end
