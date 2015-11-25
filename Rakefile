require "bundler/gem_tasks"
require "rspec/core/rake_task"
require "rake/testtask"

Rake::TestTask.new do |t|
  t.libs << "test"
  t.test_files = FileList['test/**/*_test.rb']
end

Rake::TestTask.new('test:unit') do |t|
  t.libs << "test"
  t.test_files = FileList['test/*_test.rb']
end

Rake::TestTask.new('test:functional') do |t|
  t.libs << "test"
  t.test_files = FileList['test/functional/**/*_test.rb']
end

Rake::TestTask.new('test:integration') do |t|
  t.libs << "test"
  t.test_files = FileList['test/integration/**/*_test.rb']
end

RSpec::Core::RakeTask.new(:spec) do |task|
  task.pattern = "./spec/**/*_spec.rb"
  task.rspec_opts = ['--color']
end

CACERT_PATH = 'lib/adyen/api/cacert.pem'

desc 'Update CA root certificates for the simple SOAP client'
task :update_cacert do
  tmp = '/tmp/cacert.pem.new'
  sh "curl -o #{tmp} http://curl.haxx.se/ca/cacert.pem"
  mv CACERT_PATH, '/tmp/cacert.pem.old'
  cp tmp, CACERT_PATH
end

# # Update the cacert.pem file before each release.
# task :build => :update_cacert do
#   sh "git diff-index --quiet HEAD #{CACERT_PATH} || (git add #{CACERT_PATH} && git commit -m '[API] Update CA root certificates file.')"
# end

begin
  require 'rubygems'
  require 'yard'
  require File.expand_path('../yard_extensions', __FILE__)
  YARD::Rake::YardocTask.new do |y|
    y.options << '--no-private' << '--title' << "The 'Adyen payment service' library for Ruby"
  end
rescue LoadError
end

task :default => [:test, :spec]
