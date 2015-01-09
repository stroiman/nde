namespace :paket do
  paket_exe = "paket.exe"

  file paket_exe do
    system "mono paket.bootstrapper.exe"
  end

  desc "Downloads paket.exe"
  task :bootstrap => paket_exe
end

# Creates the concrete task for building F# code
class FscBuilder
  attr_accessor :output_file

  def initialize task_name
    @task_name = task_name
    @output_file = task_name
    yield self if block_given?
  end
  
  def source_files
    @source_files ||= []
  end

  def create_task
    if (output_file != @task_name)
      # Define this task before the file task, so any
      # task description will be associated with this task
      Rake::Task::define_task @task_name => @output_file
    end
    Rake::FileTask::define_task @output_file => source_files do |t|
      output = "--out:#{@output_file}"
      target = "--target:exe"
      sources = source_files.join(" ")
      system "fsharpc #{output} #{target} #{sources}"
    end
  end
end

def fsc *args, &block 
  builder = FscBuilder.new *args, &block
  builder.create_task
end

task :clean do
end

desc "Main executable"
fsc :build do |t|
  t.output_file = "nde.exe"
  t.source_files << "main.fs"
end

#task :build => ["nde.exe"] 

task :default => [:clean, :build]
