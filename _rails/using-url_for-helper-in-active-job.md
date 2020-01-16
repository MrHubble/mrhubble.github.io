---
id: 100
title: Using url_for helper in Active Job
date: 2018-07-10T02:39:28+00:00
author: finally
layout: rails
guid: https://www.mrcodebot.com/?p=100
permalink: /using-url_for-helper-in-active-job/
categories:
  - Rails
---
I wanted to use `url_for` in a job, so I added the following:

```ruby
# Routes aren't included by default into job classes so it's up to the developer to configure what they need
# When the module is included we'll add a class attribute if there's no existing default_url_options method.
# Where we do include them by default we have config options - one for Action Mailer and one for Action Controller.
# taken from: https://github.com/rails/rails/issues/29992#issuecomment-318819265
include Rails.application.routes.url_helpers
  self
  def default_url_options
    Rails.application.config.action_mailer.default_url_options
  end
end
```

So in the above code I tell the job to use the ActionMailer `default_url_options` which is:

`config.action_mailer.default_url_options = { :host => "http://www.mysite.io" }`

Now when my job runs it tries to open the report at `https://www.mysite.io/rails/active_storage/blobs/crazyguid/report.pdf`