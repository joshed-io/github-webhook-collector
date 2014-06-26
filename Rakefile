$stdout.sync = true

require 'octokit'
require 'keen'

raise "Please configure Keen!" unless Keen.project_id && Keen.write_key

WEBHOOK_URL =
  "https://api.keen.io/3.0/projects/#{Keen.project_id}/events/github-webhook-collector?api_key=#{Keen.write_key}"

puts "Webhook URL is: #{WEBHOOK_URL}"

Octokit.configure do |c|
  c.login = ENV['GITHUB_USERNAME']
  c.password = ENV['GITHUB_PASSWORD']
end

OCTOCLIENT = Octokit::Client.new

def all_repos
  # add your account's repos
  repos = OCTOCLIENT.repos

  # add the repos of all the orgs you're in
  OCTOCLIENT.orgs.each do |org|
    org.rels[:repos].get.data.each do |repo|
      repos.push(repo)
    end
  end

  repos
end

namespace :hooks do
  desc 'List webhooks'
  task :list do
    all_repos.each do |repo|
      if repo.permissions.admin
        OCTOCLIENT.hooks(repo[:full_name]).each do |hook|
          url = hook.config.url
          puts "#{repo[:full_name]}: #{url}" if url
        end
      end
    end
  end

  desc 'Add webhooks'
  task :create do
    all_repos.each do |repo|
      if repo.permissions.admin
        begin
          OCTOCLIENT.create_hook(repo.full_name, 'web', {
            :url => WEBHOOK_URL,
            :content_type => 'json' },
            { :active => true, :events => '*' })
          puts "Webhook created for #{repo[:full_name]}"
        rescue => e
          puts e.message
        end
      end
    end
  end

  desc 'Remove webhooks'
  task :remove do
    all_repos.each do |repo|
      if repo.permissions.admin
        OCTOCLIENT.hooks(repo[:full_name]).each do |hook|
          if hook.config.url =~ /api\.keen\.io/
            OCTOCLIENT.remove_hook(repo[:full_name], hook.id)
            puts "#{repo[:full_name]}: #{hook.config.url}"
          end
        end
      end
    end
  end
end
