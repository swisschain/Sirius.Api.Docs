# Sirius API documentation

Web page: [swisschain.github.io/Sirius.Api.Docs](https://swisschain.github.io/Sirius.Api.Docs)

Structure

- [index.html.md](https://github.com/swisschain/Sirius.Api.Docs/blob/master/source/index.html.md) - root page
- [v2.html.md](https://github.com/swisschain/Sirius.Api.Docs/blob/master/source/v2.html.md) - root page
- [change-log.md](https://github.com/swisschain/Sirius.Api.Docs/blob/master/source/v1/includes/_change-log.md) - change log description

Local setup

- Install [Ruby with Devkit](https://github.com/oneclick/rubyinstaller2/releases/download/RubyInstaller-2.7.5-1/rubyinstaller-devkit-2.7.5-1-x64.exe)
- Install Node.js [v14.18.0](https://nodejs.org/download/release/v14.18.0/node-v14.18.0-x64.msi)
- gem install bundler
- bundle install
- bundle exec middleman build
- bundle exec middleman serve

NOTE: Don't remove Gemfile.lock

Whenever new API V2 endpoint is added, link to its "Signing data format" subsection should be added to the
[signing data](https://github.com/swisschain/Sirius.Api.Docs/blob/master/source/includes/v2/_signing-data.md)
 page.
