---
layout: post
title:  "ArgumentError:Error on Rails 3.0 and Selenium Webdriver v4.9.1"
date:   2023-05-12 18:00:16 +0700
categories: rails
---
I have a quest, upgrading ruby 2.7 to ruby 3.0 on the Rails app. We use rspec with Capybara and Selenium to do e2e testing. I don't expect the upgrade will be smooth, it should be any adjustment.

Ruby 3 introduce a new way to use [keyword arguments](https://www.ruby-lang.org/en/news/2019/12/12/separation-of-positional-and-keyword-arguments-in-ruby-3-0/). As expected, I need to adjust that feature.

Back to upgrading ruby 2.7 to ruby 3.0, when I run page testing with capybara and selenium, I got error:

```sh
68) Manage Virtual Gifts can create, edit virtual gift and assign to stream using content targetings
      Got 0 failures and 2 other errors:

      68.1) Failure/Error: page.current_window.resize_to(1440, 900)

            ArgumentError:
              wrong number of arguments (given 2, expected 0..1)
            # /usr/local/bundle/gems/selenium-webdriver-4.9.1/lib/selenium/webdriver/common/logger.rb:51:in `initialize'
            # /usr/local/bundle/gems/capybara-3.39.0/lib/capybara/selenium/logger_suppressor.rb:8:in `initialize'
            # /usr/local/bundle/gems/selenium-webdriver-4.9.1/lib/selenium/
            ...
            # --- Caused by: ---
            # Errno::EADDRNOTAVAIL:
            #   Cannot assign requested address - bind(2) for "::1" port 9515
            #   /usr/local/bundle/gems/selenium-webdriver-4.9.1/lib/selenium/webdriver/common/port_prober.rb:35:in `initialize'
```

and the very bottom it also show:

```sh
68.2) Failure/Error:
                    def initialize(progname = 'Selenium', default_level: nil, ignored: nil, allowed: nil)
                      default_level ||= $DEBUG || ENV.key?('DEBUG') ? :debug : :warn
              
                      @logger = create_logger(progname, level: default_level)
                      @ignored = Array(ignored)
                      @allowed = Array(allowed)
                      @first_warning = false

            ArgumentError:
              wrong number of arguments (given 2, expected 0..1)
```

So I look at the doc and found the similar issue, [ArgumentError: wrong number of arguments](https://github-com.translate.goog/teamcapybara/capybara/issues/2666?_x_tr_sl=auto&_x_tr_tl=en&_x_tr_hl=en&_x_tr_pto=wapp).

So I downgrade the Selenium Webdriver to v4.9.0 for now until the patch released.

I ran again the test. Not it passed

```sh
rspec ./spec/features/admin/app_download_banner/add_app_download_banner_spec.rb
=== rspec simplecov started: ===
WARN: Unresolved or ambiguous specs during Gem::Specification.reset:
      minitest (>= 5.1)
      Available/installed versions of this gem:
      - 5.18.0
      - 5.14.2
      rake (>= 12.2)
      Available/installed versions of this gem:
      - 13.0.6
      - 13.0.3
WARN: Clearing out unresolved specs. Try 'gem cleanup <gem>'
Please report a bug if this causes problems.
logname: no login name
Capybara starting Puma...
* Version 4.3.12 , codename: Mysterious Traveller
* Min threads: 0, max threads: 4
* Listening on tcp://0.0.0.0:9888
.

Finished in 9.51 seconds (files took 2.51 seconds to load)
1 example, 0 failures
```

