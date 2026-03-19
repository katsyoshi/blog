require "fileutils"
require "jekyll"
require "rake/file_list"
require "shellwords"

include FileUtils

SSH_USER       = "katsu@katsyoshi.org"
SSH_PORT       = "2222"
DOCUMENT_ROOT  = "~/blog/"
RSYNC_DELETE   = false
RSYNC_ARGS     = ""
DEPLOY_DEFAULT = :push
DEPLOY_BRANCH  = "master"

PUBLIC_DIR   = "public"
SOURCE_DIR   = "source"
DEPLOY_DIR   = "_deploy"
POSTS_DIR    = "_posts"
NEW_POST_EXT = "markdown"
NEW_PAGE_EXT = "markdown"
SERVER_PORT  = "4000"

READY_ERROR = "### Missing #{SOURCE_DIR}. This repository expects checked-in Jekyll sources under `#{SOURCE_DIR}`."

if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM)
  puts "## Set the codepage to 65001 for Windows machines"
  `chcp 65001`
end

def ensure_source_dir!
  raise READY_ERROR unless File.directory?(SOURCE_DIR)
end

def slugify(value)
  slug = Jekyll::Utils.slugify(value.to_s.strip)
  slug.empty? ? "new-post" : slug
end

def prompt(message)
  print message
  STDIN.gets.to_s.chomp
end

def ask(message, valid_options = nil)
  if valid_options
    options = valid_options.join("/")
    answer = nil
    answer = prompt("#{message} #{options} ") until valid_options.include?(answer)
    answer
  else
    prompt(message)
  end
end

def confirm_overwrite!(path)
  return unless File.exist?(path)

  answer = ask("#{path} already exists. Do you want to overwrite?", %w[y n])
  abort("rake aborted!") if answer == "n"
end

def run_command!(*command, env: {}, chdir: nil)
  success =
    if chdir
      Dir.chdir(chdir) { system(env, *command) }
    else
      system(env, *command)
    end

  raise "Command failed: #{command.join(' ')}" unless success
end

def run_foreground_command!(*command, env: {})
  pid = Process.spawn(env, *command)

  trap("INT") do
    Process.kill("TERM", pid) rescue Errno::ESRCH
    Process.wait(pid) rescue Errno::ECHILD
    exit 130
  end

  _, status = Process.wait2(pid)
  raise "Command failed: #{command.join(' ')}" unless status.success?
ensure
  trap("INT", "DEFAULT")
end

def copy_dot_files(source, dest)
  FileList["#{source}/**/.*"]
    .exclude("**/.", "**/..", "**/.DS_Store", "**/._*")
    .each do |file|
      next if File.directory?(file)

      target = file.sub(/\A#{Regexp.escape(source)}/, dest)
      mkdir_p(File.dirname(target))
      cp_r(file, target)
    end
end

def write_front_matter(path, attributes)
  mkdir_p(File.dirname(path))
  File.open(path, "w") do |file|
    file.puts("---")
    attributes.each do |key, value|
      file.puts("#{key}: #{value}")
    end
    file.puts("---")
  end
end

def post_path(title)
  File.join(
    SOURCE_DIR,
    POSTS_DIR,
    "#{Time.now.strftime('%Y-%m-%d')}-#{slugify(title)}.#{NEW_POST_EXT}"
  )
end

def page_destination(raw_filename)
  input = raw_filename.to_s.strip
  return nil if input.empty?

  parts = input.sub(%r{\A/+}, "").split("/")
  basename = parts.pop
  return nil if basename.nil? || basename.empty?

  if basename.include?(".")
    extension = File.extname(basename).delete_prefix(".")
    title = basename.sub(/#{Regexp.escape(File.extname(basename))}\z/, "")
  else
    title = basename
    extension = nil
  end

  return nil if title.nil? || title.empty?

  page_dir = [SOURCE_DIR, *parts.map { |part| slugify(part) }]

  if extension.nil? || extension.empty?
    page_dir << slugify(title)
    filename = "index"
    extension = NEW_PAGE_EXT
  else
    filename = slugify(title)
  end

  [File.join(*page_dir, "#{filename}.#{extension}"), title]
end

def rsync_command
  command = ["rsync", "-avze", "ssh -p #{SSH_PORT}"]
  command.concat(["--exclude-from", File.expand_path("./rsync-exclude")]) if File.exist?("./rsync-exclude")
  command.concat(Shellwords.split(RSYNC_ARGS)) unless RSYNC_ARGS.empty?
  command << "--delete" if RSYNC_DELETE
  command << "#{PUBLIC_DIR}/"
  command << "#{SSH_USER}:#{DOCUMENT_ROOT}"
  command
end

desc "Generate Jekyll site"
task :generate do
  ensure_source_dir!

  puts "## Generating site with Jekyll"
  env = {}
  env["JEKYLL_ENV"] = ENV["JEKYLL_ENV"] if ENV["JEKYLL_ENV"]
  run_command!("jekyll", "build", "-d", PUBLIC_DIR, env: env)
end

desc "Preview the site locally"
task :preview do
  ensure_source_dir!

  puts "## Starting Jekyll server on port #{SERVER_PORT}"
  run_foreground_command!(
    "jekyll",
    "serve",
    "-d",
    PUBLIC_DIR,
    "-P",
    SERVER_PORT,
    env: { "OCTOPRESS_ENV" => "preview" }
  )
end

desc "Begin a new post in #{SOURCE_DIR}/#{POSTS_DIR}"
task :new_post, :title do |_task, args|
  ensure_source_dir!

  title = args.title || prompt("Enter a title for your post: ")
  filename = post_path(title)
  confirm_overwrite!(filename)

  puts "Creating new post: #{filename}"
  write_front_matter(
    filename,
    "layout" => "post",
    "title" => "\"#{title.gsub(/&/, '&amp;')}\"",
    "date" => Time.now.strftime("%Y-%m-%d %H:%M:%S %z"),
    "comments" => "true",
    "categories" => ""
  )
end

desc "Create a new page in #{SOURCE_DIR}/(filename)/index.#{NEW_PAGE_EXT}"
task :new_page, :filename do |_task, args|
  ensure_source_dir!

  args.with_defaults(filename: "new-page")
  file, title = page_destination(args.filename)

  unless file
    puts "Syntax error: #{args.filename} contains unsupported characters"
    next
  end

  confirm_overwrite!(file)
  puts "Creating new page: #{file}"
  write_front_matter(
    file,
    "layout" => "page",
    "title" => "\"#{title}\"",
    "date" => Time.now.strftime("%Y-%m-%d %H:%M"),
    "comments" => "true",
    "sharing" => "true",
    "footer" => "true"
  )
end

desc "Clean out caches and generated stylesheet artifacts"
task :clean do
  rm_rf([
    ".pygments-cache/**",
    ".gist-cache/**",
    ".sass-cache/**",
    "source/stylesheets/screen.css"
  ])
end

desc "Default deploy task"
task :deploy do
  unless ENV["JEKYLL_ENV"] == "production"
    puts "REALLY DEPLOY? check your JEKYLL_ENV"
    puts "exec:"
    puts "JEKYLL_ENV=production bundle exec rake deploy"
    exit(0)
  end

  copy_dot_files(SOURCE_DIR, PUBLIC_DIR)
  Rake::Task[DEPLOY_DEFAULT].invoke
end

desc "Generate website and deploy"
task gen_deploy: %i[generate deploy]

desc "Copy dot files for deployment"
task :copydot, :source, :dest do |_task, args|
  raise ArgumentError, "source and dest are required" unless args.source && args.dest

  copy_dot_files(args.source, args.dest)
end

desc "Deploy website via rsync"
task :rsync do
  puts "## Deploying website via Rsync"
  run_command!(*rsync_command)
end

desc "Deploy public directory to GitHub Pages"
task :push do
  puts "## Deploying branch to GitHub Pages"
  puts "## Pulling any updates from GitHub Pages"
  run_command!("git", "pull", chdir: DEPLOY_DIR)

  Dir["#{DEPLOY_DIR}/*"].each { |file| rm_rf(file) }
  copy_dot_files(PUBLIC_DIR, DEPLOY_DIR)

  puts "\n## Copying #{PUBLIC_DIR} to #{DEPLOY_DIR}"
  cp_r("#{PUBLIC_DIR}/.", DEPLOY_DIR)

  run_command!("git", "add", "-A", chdir: DEPLOY_DIR)

  changes_staged = !Dir.chdir(DEPLOY_DIR) { system("git", "diff", "--cached", "--quiet") }
  unless changes_staged
    puts "\n## No changes to deploy"
    next
  end

  message = "Site updated at #{Time.now.utc}"
  puts "\n## Committing: #{message}"
  run_command!("git", "commit", "-m", message, chdir: DEPLOY_DIR)

  puts "\n## Pushing generated #{DEPLOY_DIR} website"
  run_command!("git", "push", "origin", DEPLOY_BRANCH, chdir: DEPLOY_DIR)
  puts "\n## GitHub Pages deploy complete"
end

desc "List tasks"
task :list do
  puts "Tasks: #{(Rake::Task.tasks - [Rake::Task[:list]]).join(', ')}"
  puts "(type rake -T for more detail)\n\n"
end
