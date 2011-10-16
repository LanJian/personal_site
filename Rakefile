@github_pages_repo = "~/projects/lanjian.github.com/"

desc "Parse haml layouts"
task :haml do
  print "Parsing Haml layouts..."
  system(%{
    cd _layouts/haml && 
    for f in *.haml; do 
      [ -e $f ] && 
      haml $f ../${f%.haml}.html;
    done
  })
  puts "done."
end

desc "Launch preview environment"
task :default => [:haml, :clean] do
  system "jekyll --auto --server"
end

desc "Build site"
task :build => [:haml, :clean] do |task, args|
  system "jekyll"
end

task :clean do
  system "rm -rf _site"
end

desc "Copy generated site to github Pages repo"
task :commit do
  system "cp -rf _site/* #{@github_pages_repo}"
  puts "copied site to #{@github_pages_repo}"
end

desc "Watch for layout haml changes"
task :watch do
  require 'fssm'
  puts "Watching _layouts folder..."
  directory = "_layouts/"
  FSSM.monitor(directory, '**/*.haml') do
    update do |base, relative|
      input = "#{base}/#{relative}"
      output = "#{base}/#{File.basename(relative).gsub!('.haml', '.html')}"
      command = "haml #{input} #{output}"
      %x{#{command}}
      puts "Regenerated #{input} to #{output}"
    end
  end
end


namespace :compass do  

  desc 'Delete temporary compass files'
  task :clean do
    system "rm -fR css/*"
  end

  desc 'Run the compass watch script'
  task :watch do
    system "compass watch"
  end

  desc 'Compile sass scripts'
  task :compile => [:clean] do
    system "compass compile"
  end
  
end
