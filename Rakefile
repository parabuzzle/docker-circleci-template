require 'rake'
require 'httparty'

def version
  File.readlines('./VERSION').first.strip
end

def container_name
  File.basename(Dir.getwd)
end

def username
  'parabuzzle'
end

def latest_hub_version
  taginfo        = JSON.parse(HTTParty.get("https://hub.docker.com/v2/repositories/#{username}/#{container_name}/tags/").body)['results']
  tags = []
  taginfo.each do |tag|
    tags << tag['name']
  end
  (tags - ['latest']).sort.last
end

def next_version
  base           = version
  taginfo        = JSON.parse(HTTParty.get("https://hub.docker.com/v2/repositories/#{username}/#{container_name}/tags/").body)['results']
  return "#{base}.0" if taginfo.nil?
  tags = []
  taginfo.each do |tag|
    tags << tag['name']
  end
  current_base   = tags.grep(/#{base}/)
  return "#{base}.0" if current_base.empty?
  build = current_base.sort { |x,y|
      a = x.split('.')[base.split('.').count].to_i
      b = y.split('.')[base.split('.').count].to_i
      a <=> b
    }.last.split('.').last.to_i + 1
  return "#{base}.#{build}"
end

task :install_deps do
  sh 'gem install bundler'
  sh 'bundle install'
end

desc "tags latest as next_version"
task :tag do
  sh "docker tag -f #{username}/#{container_name}:latest #{username}/#{container_name}:#{next_version}"
end

desc "pushes the next_version and latest to docker hub"
task :push => :tag do
  sh "docker push #{username}/#{container_name}:#{next_version}"
  sh "docker push #{username}/#{container_name}:latest"
end

desc "builds as latest"
task :build => :install_deps do
  sh "docker build -t #{username}/#{container_name}:latest ."
end

task :default => [:build, :push]
