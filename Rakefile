require "open3"

deploy_dir = "public"

posts_dir       = "content"    # directory for blog files
new_post_ext    = "md"  # default new post file extension when using the new_post task
new_page_ext    = "md"  # default new page file extension when using the new_page task
server_port     = "3000"      # port for preview server eg. localhost:4000

#######################
#  Working with Zola  #
#######################

desc "Generate zola site"
task :generate do
  puts "## Generating Site with Zola"
  system "ZOLA_ENV='#{ENV["ZOLA_ENV"]}' zola build"
end

desc "preview the site in a web browser"
task :preview do
  puts "Starting to watch source with Jekyll and Compass. Starting Rack on port #{server_port}"
  interface = ENV["ZOLA_INTERFACE"] ? "--interface #{ENV["ZOLA_INTERFACE"]}" : nil
  server_port = "--port #{ENV["ZOLA_PORT"] || server_port}"
  base_url = ENV["ZOLA_URL"] ? "--base-url #{ENV["ZOLA_URL"]}" : nil

  process = (%w"zola serve" + [interface, server_port, base_url]).join(" ")

  zola_pid = Process.spawn(process)

  trap("INT") {
    [zola_pid,].each { |pid| Process.kill(9, pid) rescue Errno::ESRCH }
    exit 0
  }

  [zola_pid,].each { |pid| Process.wait(pid) }
end

# usage rake new_post[my-new-post] or rake new_post['my new post'] or rake new_post (defaults to "new-post")
desc "Begin a new post in #{posts_dir}"
task :new_post, :title do |t, args|
  if args.title
    title = args.title
  else
    title = get_stdin("Enter a title for your post: ")
  end
  raise "### You haven't set anything up yet. First run `rake install` to set up an Octopress theme." unless File.directory?(posts_dir)
  mkdir_p "#{posts_dir}/"
  filename = "#{posts_dir}/#{Time.now.strftime('%Y-%m-%d')}-#{title}.#{new_post_ext}"
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end
  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "+++"
    post.puts "title = \"#{title}\""
    post.puts "date = #{Time.now.strftime("%FT%T%:z")}"
    post.puts "path = \"/blog/#{Time.now.strftime('%Y/%m/%d')}/#{title}\""
    post.puts "+++"
  end
end

desc "Generate zola"
task :generate do |t, args|
  process = ["zola", "build"]
  run(*process)
end

desc "Deploy zola"
task :deploy do |t, args|
  process = ["git", "push"]
  run(*process)
end

def run(*process)
  Open3.popen3(*process) { |_i, o, e, _t|
    puts o.read
    STDERR.puts e.read
  }
end
