#! /usr/bin/env ruby

require "rubygems"
require "bundler"

Bundler.require

task default: :tweet

desc "Gets tweets"
task :tweet do
  client = Twitter::REST::Client.new do |config|
    config.consumer_key = "9WillnTISas5zRRHDnEAXhtNz"
    config.consumer_secret = "OmjEz2xHwuX80sBqzU592ggSS1GAzssqRdZJlIWnrgQ8Ab1DT7"
  end

  client.list_timeline("icco", "short-list", {count: 500}).each do |t|
    break if Chronic.parse("last night") > t.created_at
    puts "\"#{t.full_text}\"\n  -- @#{t.user.screen_name} / #{t.created_at}\n  -- #{t.uri}\n\n"
  end
end
