require 'yaml'
require 'hominid'

desc "Task to pull customer email templates from MailChimp and cache them in the app"
  task :mailchimp  do
    # Maintain a map of ActionMailer methods to MailChimp
    # template names in a YAML file. YAML looks like:
    # <mailer class name>:
    #   <mailer method name>: <name of MailChimp template used by method>
    # Example:
    # user_mailer:
    #   signed_up: "Welcome To Mightyvites"
    #   ...
    map=YAML.load_file("mailchimp.yaml")
    puts map
    api=Hominid::API.new(map["MAILCHIMP_API_KEY"])
    template_name = map["template_name"]
 
     # loop through the list of known MailChimp template names
     api.templates['user'].each do |template|
 
        if template_name
          html_cache=File.join(".", "#{template_name}.html.erb")
      end
 
 
      # pull the latest HTML templates from MailChimp
      html=api.template_info(template['id'].to_i)['source']
 
      # create a text version of the HTML for multi-part mail
      text=api.generate_text('html', html)
 
      # inline all CSS to ensure greatest email client compatibility
      html=api.inline_css(html)
 
      # overwrite mailer views
      File.open(html_cache, 'w+') {|f| f << html }
      text_cache=html_cache.gsub('.html.erb', '.text.erb')
      File.open(text_cache, 'w+') {|f| f << text }
    end
  end
