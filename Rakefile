require "rubygems"
require 'rake'
require 'yaml'
require 'time'

SOURCE = "."
CONFIG = {
  :posts => File.join(SOURCE, "_posts"),
  :post_ext => "md"
}

# Usage: rake post title="A Title" [date="2012-02-09"] [tags=[tag1,tag2]] [category="category"]
desc "Begin a new post in #{CONFIG[:posts]}"
task :post do
  abort("rake aborted: '#{CONFIG[:posts]}' directory not found.") unless FileTest.directory?(CONFIG[:posts])
  title = ENV["title"] || "new-post"
  slug = title.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')
  begin
    date = (ENV['date'] ? Time.parse(ENV['date']) : Time.now).strftime('%Y-%m-%d')
  rescue
    puts "Error - date format must be YYYY-MM-DD, please check you typed it correctly!"
    exit(-1)
  end
  filename = File.join(CONFIG[:posts], "#{date}-#{slug}.#{CONFIG[:post_ext]}")
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end

  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{title.gsub(/-/,' ')}\""
    post.puts "description:"
    post.puts "date: \"#{Time.now.strftime('%Y-%m-%d %H:%M:%S')}\""
    post.puts "tags:"
    post.puts "categories:"
    post.puts "---"
    post.puts ""
  end
end # task :post

def ask(message, valid_options)
  if valid_options
    answer = get_stdin("#{message} #{valid_options.to_s.gsub(/"/, '').gsub(/, /,'/')} ") while !valid_options.include?(answer)
  else
    answer = get_stdin(message)
  end
  answer
end

def get_stdin(message)
  print message
  STDIN.gets.chomp
end