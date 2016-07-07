require 'html-proofer'

desc 'Test building and consistency of the generated pages'
task :default do
  sh "bundle exec jekyll build --drafts"

  options = {
    check_html: true,
    empty_alt_ignore: true,
    file_ignore: [/(.*)windows10\-sdk\/(.*)/]
  }

  HTMLProofer.check_directory("./_site", options).run
end