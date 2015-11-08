require 'w3c_validators'
require 'pathname'
require 'colorize'

desc "Validates the HTML generated by this project"
task :validate do

  `middleman build --clean`

  validator = html: W3CValidators::MarkupValidator.new

  error_table = Dir[File.join(File.dirname(__FILE__), "build", "**", "*.html")].map do |path|
    [ path, validator.validate_file(path).errors ]
  end

  error_count = error_table.map { |path, errors| errors }.map(&:length).reduce(&:+)

  if error_count > 0
    puts "There were #{ error_count } #{ pluralize("error", error_count) } in your " \
     "files. :(\n".colorize(:red)

    error_table.each { |path, errors| print_errors(path, errors) }

    exit 1
  else
    puts "All of the files were valid! :)".colorize(:green)
  end
end

def print_errors(path, errors)
  return unless errors.length > 0

  puts "#{ errors.length } #{ pluralize("error", errors.length) } found in " \
    "#{ path }:\n".colorize(:red)

  error_messages = errors.map do |error|
    "Line #{ error.line.to_i - 4 }: #{ error.message.gsub(/\s+/, " ").strip }"
  end

  puts error_messages.join("\n\n").concat("\n")
end

def pluralize(word, count)
  "#{ word }#{ count > 1 ? "s" : "" }"
end