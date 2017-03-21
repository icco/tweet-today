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

  messages = client.list_timeline("icco", "short-list", {count: 500}).delete_if do |t|
    Chronic.parse("last night") > t.created_at
  end

  template = Tilt.new('mail.erb')
  html = template.render(nil, {messages: messages})

  if ENV['POSTMARK_API_TOKEN']
    client = Postmark::ApiClient.new(ENV['POSTMARK_API_TOKEN'])
    client.deliver(
      from: "Tweet Today <tweets@distraction.today>",
      to: 'Nat Welch <nat@natwelch.com>',
      subject: "Tweet Today #{Time.now.strftime("%F")}",
      html_body: html,
      track_links: :html)
  else
    puts html
  end
end
