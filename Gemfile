# frozen_string_literal: true

source "https://rubygems.org"

gemspec

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
gem "tzinfo-data", platforms: [:mingw, :mswin, :x64_mingw, :jruby]

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.2.0" if Gem.win_platform?
