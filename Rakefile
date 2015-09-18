require 'rake'
require 'httparty'

# Read the base version from VERSION file.
def version
  File.readlines('./VERSION').first.strip
end

# The name of the container
# This is based off of the directory name
def container_name
  File.basename(Dir.getwd)
end

# The username the container is pushed to on DockerHub
def username
  'parabuzzle'
end

# Get the latest version for the given base version provided by #version
def hub_version
  base           = version
  taginfo        = JSON.parse(HTTParty.get("https://hub.docker.com/v2/repositories/#{username}/#{container_name}/tags/").body)['results']
  return {base: base, build: nil} if taginfo.nil?
  tags = []
  taginfo.each do |tag|
    tags << tag['name']
  end
  current_base   = tags.grep(/#{base}/)
  return {base: base, build: nil} if current_base.empty?
  build = current_base.sort { |x,y|
      a = x.split('.')[base.split('.').count].to_i
      b = y.split('.')[base.split('.').count].to_i
      a <=> b
    }.last.split('.').last.to_i
  return {base: base, build: build}
end

# return current hub version for the current base
def latest_hub_version
  latest = hub_version
  "#{latest[:base]}.#{latest[:build]}"
end

# return the next version for the current base
def next_version
  latest = hub_version
  base   = version
  build  = latest[:build] || -1
  build  += 1
  "#{base}.#{build}"
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
