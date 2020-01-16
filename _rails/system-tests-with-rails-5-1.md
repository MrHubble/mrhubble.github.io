---
id: 96
title: System tests with Rails 5.1
date: 2018-05-22T02:02:13+00:00
author: finally
layout: rails
guid: https://www.mrcodebot.com/?p=96
permalink: /system-tests-with-rails-5-1/
categories:
  - Rails
---
Up until this point we were using the minitest-rails-capybara to create feature tests in our app. I experienced a request uri too large issue (<https://stackoverflow.com/questions/15961323/rails-datatables-ajax-json-414-request-uri-too-large>) when we started using server side datatables.

The fix for this was to use Puma as our webserver for Capybara rather than the default Webrick. This makes sense to me as we&#8217;re using Puma in production so it would be beneficial to have our test environment as similar to our production environment as possible.

Capybara had a bug where the Puma server wasn&#8217;t killed (<https://github.com/teamcapybara/capybara/pull/1992>) which was fixed with Capybara 3.0.0

Unfortunately the minitest-rails-capybara seems to no longer be actively maintained and it can be challenging to use Capybara 3 with this gem: <https://github.com/blowmage/minitest-rails-capybara/issues/45>

Also unfortunately Rails 5.1 won&#8217;t allow the use of Capybara 3:

> If you&#8217;re using Rails 5.1 you should be able to use gem &#8216;capybara&#8217;, &#8216;~> 2.18&#8217; in your gem file since Rails 5.1 won&#8217;t allow Capybara 3+ due to this change (which IMHO was bad and has been corrected in 5.2) if you&#8217;re using system tests. With Rails 5.2 you should be good to use gem &#8216;capybara&#8217;, &#8216;~> 3.0&#8217; (note that specifying the <4.0 is unnecessary due to the the way ~> works

<https://github.com/rails/rails/pull/30959#discussion_r181931451>

After upgrading to Capybara 2.18 and adding the gem `chromedriver-helper` I no experienced an `Element is not clickable at point` error:

    # Easy installation and use of chromedriver to run system tests with Chrome
    gem 'chromedriver-helper', '~> 1.2'


Other info:

> The file capybara/minitest was added to Capybara in version 2.13.0, which is the minimum version Rails requires for its system tests since Rails 5.1.0. Upgrade to the latest version of Capybara (2.14.4) and there should be no need for the minitest-capybara or minitest-rails gems. You will need to also add the &#8216;selenium-webdriver&#8217; gem to your test group.
>
> Additionally the assert_response :success line is&#8217;t valid in Capybara tests because the HTTP response code from the browser Capybara is using isn&#8217;t generally available.

<https://stackoverflow.com/a/45202150/1299792>