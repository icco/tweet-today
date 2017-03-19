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

  emailbody = "# Tweets for #{Time.now.strftime("%F")}:\n\n"

  client.list_timeline("icco", "short-list", {count: 500}).each do |t|
    break if Chronic.parse("last night") > t.created_at
    emailbody += "> \"#{t.full_text}\"\n\n - @#{t.user.screen_name} / #{t.created_at}\n - #{t.uri}\n\n"
  end

  markdown = Redcarpet::Markdown.new(Redcarpet::Render::HTML, autolink: true, tables: true)
  html = markdown.render(emailbody)

  if ENV['POSTMARK_API_TOKEN']
    client = Postmark::ApiClient.new(ENV['POSTMARK_API_TOKEN'])
    client.deliver(
      from: "Tweet Today <tweets@distraction.today>",
      to: 'Nat Welch <nat@natwelch.com>',
      subject: "Tweet Today #{Time.now.strftime("%F")}",
      html_body: html,
      text_body: emailbody,
      track_links: :html_and_text)
  else
    puts emailbody

    puts html
  end
end
