# Cap::Aws::Ec2

Tired of setting hardcoded ips or dns names in your capistrano deploy script?
You just found the solution!

cap-aws-ec2 will fetch instances from you EC2 region and define them by
project, environment and roles.

This gem should be used in conjunction with [capistrano multistage](https://github.com/capistrano/capistrano/wiki/2.x-Multistage-Extension)

Note: This gem is only compatibale with Capistrano 2.x

## Installation

Add this line to your application's Gemfile:

    gem 'cap-aws-ec2', require: false

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install cap-aws-ec2

## Usage

1. Define tags for your servers on EC2:

  | Name Tag      | Project Tag   | Environment Tag | Roles Tag       | Public IP |
  | ------------- |:-------------:|:---------------:|:---------------:| ---------:| 
  | ftbpro-web1   | ftbpro-web    | production      | app,web         | 1.1.1.1   |
  | ftbpro-web2   | ftbpro-web    | production      | app,web         | 2.2.2.2   |
  | ftbpro-resque | ftbpro-web    | production      | app,resque      | 3.3.3.3   | 

2. In your `deploy.rb` script:
  ```ruby
  require 'cap-aws-ec2'
  set :aws_key_id, 'XXXXXXXXXXXXXX'
  set :secret_access_key, 'XXXXXXXXXXXXXXXXXXXX'
  set :aws_region, 'us-west-2' 
  set :ec2_project, 'my-project-tag-value'
  set(:ec2_env) { stage }
  ```

3. In your corresponding stage file (`config/deploy/production.rb`):
  ```ruby
  define_servers
  ```

This will result in the following statements:

```ruby
server 1.1.1.1, :app, :web
server 2.2.2.2, :app, :web
server 3.3.3.3, :app, :resque
```

## Contributing

1. Fork it ( http://github.com/<my-github-username>/cap-aws-ec2/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
