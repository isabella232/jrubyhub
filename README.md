## Welcome to JRubyHub

JRubyHub is a Rails 3 application that showcases JRuby and Java
technologies mixed with Rails.

We're planning to run a live instance of JRubyHub on the
http://jruby.org site eventually. Currently we're in heavy development
mode.

## Development Notes

- We're using a custom fork of Spork for fast re-running of Cucumber
  features and RSpec examples in a single JRuby process. We had to
  tweak some things to get this working clean:
  - Nick's Spork fork features a single process looper (originally by
    Roger Pack) that also has to reset the Cucumber and RSpec shared
    state each time so it doesn't report previous runs' results.
  - Set `config.cache_classes = false` in
    `config/environments/test.rb` so that code gets reloaded in the
    test environment. Cucumber warns about this breaking its
    use_transactional_fixtures method, but we're not using
    transactional fixtures here.
  - Run `::ActiveSupport::Dependencies.clear` in the Spork.each_run
    block to reload code.
- To run the Spork servers, type `jruby script/autodev`. This starts
  up two Spork servers, one each for Cucumber and RSpec, as well as a
  Watchr script (`script/project.watchr`) that pings Cucumber and/or
  RSpec every time the appropriate files are touched.

## Production notes

JRubyHub uses Neo4j for data storage. To specify the disk path where
data should be kept, set the Java system property `jrubyhub.home`. The
path defaults to the system-wide temporary directory found in
'java.io.tmpdir' + '/jrubyhub' if not specified.

To use Warbler to create a .war file of JRubyHub, you'll need to
perform the following steps in order to work an issue with Warbler and
Bundler interaction:

    $ bundle install --without test
    $ warble

If you want to go back to develop-and-test mode, you'll need to
`bundle install` again.
