#! /usr/bin/env ruby

require "rubygems"
require "bundler"

Bundler.require

task default: :tweet

desc "Gets tweets"
task :tweet do
  raise "$TWITTER_KEY not set in ENV, cannot start." if ENV['TWITTER_KEY'].nil?
  raise "$TWITTER_SECRET not set in ENV, cannot start." if ENV['TWITTER_SECRET'].nil?
  raise "$TWITTER_LIST not set in ENV, cannot start." if ENV['TWITTER_LIST'].nil?

  client = Twitter::REST::Client.new do |config|
    config.consumer_key = ENV["TWITTER_KEY"]
    config.consumer_secret = ENV["TWITTER_SECRET"]
  end

  username, list_id = ENV["TWITTER_LIST"].split("/")

  messages = client.list_timeline(username, list_id, {count: 1000}).delete_if do |t|
    Chronic.parse("last night") > t.created_at
  end

  template = Tilt.new('mail.erb')
  html = template.render(nil, {
    messages: messages,
    oembeds: client.oembeds(messages, {omit_script: true})
  })

  if ENV['POSTMARK_API_TOKEN']
    client = Postmark::ApiClient.new(ENV['POSTMARK_API_TOKEN'])
    client.deliver(
      from: "Tweet Today <tweets@distraction.today>",
      to: 'Nat Welch <nat@natwelch.com>',
      subject: "Tweet Today #{Time.now.strftime("%F")}",
      html_body: html,
      track_links: :html_only)
  else
    puts html
  end
end
