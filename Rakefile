require "highline/import"

namespace :wp do

  namespace :dump do

    desc "Rewrite the host in a Wordpress database dump"
    task :rewrite do
      file = ask("Path to sql dump:") do |q| 
        validate(q, "File does not exist.") do |f|
          File.exists?(f)
        end
      end
      
      search = ask("String to search for:") do |q|
        validate(q, "No string given.") do |s|
          !s.empty?
        end        
      end
      
      replace = ask("String to replace with:") do |q|
        validate(q, "No string given.") do |r|
          !r.empty?
        end        
      end      
      
      confirmed = ask("Do you really want to replace #{search} with #{replace} in #{file}? (y/n)") do |q|
        validate(q, "Aborted by user.") do |c|
          c == 'Y' || c == 'y'
        end
      end
      
      if confirmed
        say `perl -pi -e 's/#{regexp_escape(search)}/ "#{regexp_escape(replace)}"/g' #{file}`
        say "Finished processing #{file}"        
      end
    end

  end
  
  def regexp_escape(string)
    Regexp.escape(string).gsub('/', '\/')
  end
  
  def validate(question, message, &block)
    question.responses[:not_valid] = ""
    question.responses[:ask_on_error] = :question
    question.validate = proc do |subject|
      valid = block.call(subject)
      say message unless valid
      valid
    end
  end
  
end