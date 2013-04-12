#!/usr/bin/env ruby

require 'yaml'
require 'rubygems'
require 'net/dns'
require 'optparse'

options = {}
OptionParser.new do |opts|
  opts.banner = "Usage: your_app [options]"
  opts.on('-f [ARG]', '--file [ARG]', "Provider YML file") do |v|
    options[:file] = v
  end
  opts.on('-t [ARG]', '--type [ARG]', "Query type (e.g. A, NS, MX, CNAME") do |v|
    options[:type] = v.downcase
  end
  opts.on('-q [ARG]', '--query [ARG]', "Query (e.g. example.com, host.example.com)") do |v|
    options[:query] = v
  end
  opts.on('-h', '--help', 'Display this help') do 
    puts opts
    exit
  end
end.parse!

unless options[:query] && options[:type] && options[:file]
  $stderr.puts "Error: you must specify --file, --query and --type options."
  exit
end

providers = YAML::load(File.open(options[:file]))

case options[:type]
when 'soa'
	type = Net::DNS::SOA
when 'ns'
	type = Net::DNS::NS
when 'a'
	type = Net::DNS::A
when 'cname'
	type = Net::DNS::CNAME
when 'mx'
	type = Net::DNS::MX
end


providers.each_key { |provider|

	puts "----------------------------------------"
	puts " #{provider}"
	puts "----------------------------------------"

	
	primary = providers[provider]['primary']
	secondary = providers[provider]['secondary']
	
	resolver = Net::DNS::Resolver.new(:nameservers => ["#{primary}","#{secondary}"])
	resolver.log_level = Net::DNS::UNKNOWN
	begin
		packet  = resolver.query(options[:query], type)
	rescue
		puts "No response from nameservers list."
	else

		header = packet.header
		answer = packet.answer

		puts "The packet is #{packet.data.size} bytes"
		puts "It contains #{header.anCount} answer entries"
	
		answer.each {|ans| puts ans}
	end
}


