require 'bundler'
Bundler::GemHelper.install_tasks

require 'rspec/core/rake_task'
RSpec::Core::RakeTask.new(:spec)
task :default => :spec

require 'appraisal'

#---------------------------------------------------------------------------------------------------
# Monkey patch Bundler gem_helper so we release to our gem server instead of rubygems.org
module Bundler
  class GemHelper
    def rubygem_push(path)
      if Gem::Version.new(Bundler::VERSION) >= Gem::Version.new("2.3.0")
        sh [
          "gem",
          "push",
          "--verbose",
          "--host",
          "https://artifactory.elstc.co/artifactory/api/gems/swiftype-gems",
          "--key",
          "elastic",
          path.to_s
        ]
      else
        sh("gem push --verbose --host https://artifactory.elstc.co/artifactory/api/gems/swiftype-gems --key elastic '#{path}'")
      end
      Bundler.ui.confirm "Pushed #{name} #{version} to artifactory"
    end
  end
end
