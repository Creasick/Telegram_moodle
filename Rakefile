require 'rake/testtask'
require_relative 'config/crear_tablas_bd'
require 'sequel'



namespace :tasks do
  namespace :db do

    namespace :test do

      desc "Creamos base de datos test"
      task :crear  do
        db=Sequel.connect(ENV['URL_DATABASE'])
        begin
          db.run "CREATE DATABASE bd_prueba"
        rescue Sequel::Error
          db.run "DROP DATABASE bd_prueba"
          db.run "CREATE DATABASE bd_prueba"
        end
        db.disconnect
        db=Sequel.connect(ENV['URL_DATABASE']+'/bd_prueba')
        crear_tablas(db)
        db.disconnect
      end

      Rake::TestTask.new :tests_bd do |t|
        t.test_files = FileList['test/test_*.rb']
        t.verbose = false
        t.warning = false
      end

      desc "Borramos la base de datos"
      task :destruir  do
        db=Sequel.connect(ENV['URL_DATABASE'])
        db.run "DROP DATABASE bd_prueba"
        db.disconnect
      end

    end

  end
end



desc "Ejecutamos los test sobre la base de datos"
task :test => ['tasks:db:test:crear', 'tasks:db:test:tests_bd', 'tasks:db:test:destruir' ]