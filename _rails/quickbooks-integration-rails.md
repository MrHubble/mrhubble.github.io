---
id: 20
title: Quickbooks integration with Rails
date: 2018-02-02T00:14:36+00:00
author: finally
layout: rails
guid: https://www.mrcodebot.com/?p=20
permalink: /quickbooks-integration-rails/
dsq_thread_id:
  - "6507847517"
categories:
  - Rails
---
Quickbooks integration with Rails can prove to be quite challenging. It seems the Quickbooks API has undergone significant changes over the years and as a new Quickbooks developer you&#8217;ll be using Oauth2 (as opposed to Oauth1).

Previous versions of the API used XML, whereas the current version uses JSON.

The most popular Ruby gem is <a href="https://github.com/ruckus/quickbooks-ruby" target="_blank" rel="noopener">quickbooks-ruby</a>, however:

> Intuit started the v3 API supporting both XML and JSON. However, new v3 API services such as Tax Service will only support JSON. This gem has roots in the v2 API, which was XML only, and hence was constructed supporting XML only.
>
> That said, the Tax Service is supported and other new v3-API-JSON-only services will be supported. Ideally, we would like to fully support JSON for all entities and services for the 1.0.0 release. Please jump in and contribute to help that aim.

<a href="https://github.com/ruckus/quickbooks-ruby#json-support" target="_blank"
rel="noopener">quickbooks-ruby#json-support</a>

```rb
def this_is_a_test
  some_func
end
```

The main problem with this gem is that oauth2 is currently not supported, however, it should be merged soon: <a href="https://github.com/ruckus/quickbooks-ruby/issues/389" target="_blank" rel="noopener">quickbooks-ruby/issues/389</a>

I think the oauth2 branch of quickbooks-ruby will use <a href="https://rubygems.org/gems/oauth2/" target="_blank" rel="noopener">the oauth2 gem</a>. Whereas the author of qbo_api prefers <a href="https://rubygems.org/gems/rack-oauth2:" target="_blank" rel="noopener">rack-oauth2</a>

> I really won&#8217;t bother trying to re-invent the wheel by rolling your own OAuth2 code. That said, the OAuth2 2-legged process is simpler than the 3-legged OAuth 1a process and if you do want to roll your own OAuth2 code take a look at Intuit&#8217;s Python example for a good starting point.
>
> As for me, I&#8217;ll choose to leverage the Rack-OAuth2 gem as I like its ability to directly set endpoints.

[access-the-quickbooks-online-api-with-oauth2](http://minimul.com/access-the-quickbooks-online-api-with-oauth2.html)

You&#8217;re going to need to create a Quickbooks developer account, follow a guide such as: [quick-start-quickbooks-online-rest-api-oauth-2-0](https://developer.intuit.com/hub/blog/2017/08/03/quick-start-quickbooks-online-rest-api-oauth-2-0)

# Putting it all together:

So I&#8217;m going to take parts from:

  * https://developer.intuit.com/docs/00\_quickbooks\_online/2\_build/10\_authentication\_and\_authorization/10\_oauth\_2.0
  * https://github.com/minimul/qbo_api/blob/master/example/app.rb
  * https://github.com/ruckus/quickbooks-ruby/blob/389-oauth2/README.md
  * https://github.com/IntuitDeveloper/OAuth2\_RubyOnRails/blob/master/OAuth2/app/controllers/token\_controller.rb

## 1&#46; Initiating the authorization request

Initiate the OAuth 2 process by redirecting the user to Intuit&#8217;s OAuth 2.0 server. The response is sent to the redirect_uri that you specified in the request.

Sample code:

<pre><code class="ruby">#quickbooks_controller.rb
class QuickbooksController &lt; ApplicationController
    def index
    end

    def authenticate
        session[:state] = SecureRandom.uuid
        client = Quickbooks.new.oauth2_client
        grant_url = client.auth_code.authorize_url(response_type: "code", state: session[:state], scope: "com.intuit.quickbooks.accounting")
        redirect_to grant_url
    end

</code></pre>

## 2&#46; Exchange code for refresh and access tokens

After the app receives the authorization code, it exchanges the authorization code for refresh and access tokens.

## 3&#46; Refreshing the access token

> Access tokens are valid for 3600 seconds (one hour), after which time you need to get a fresh one using the latest refresh\_token returned to you from the previous request. When you request a fresh access token, always use the refresh token returned in the most recent token\_endpoint response. Your previous refresh tokens expire 24 hours after you receive a new one. If an access\_token is not requested during the current refresh\_token 100 day lifetime, the refresh\_token expires and access to the QuickBooks Online company terminates. As long as it hasn&#8217;t expired, the refresh\_token can be reset as often as needed within a one year access window from the time the original access\_token was generated. Access to the QuickBooks Online company terminates when the current refresh\_token expires or at the end of the one year access window from the time the original access_token was generated, whichever is earlier. When access terminates, your app must ask the user to reauthorize the connection.

[above taken from various parts of the Quickbooks OAuth 2.0 guide](https://developer.intuit.com/docs/00_quickbooks_online/2_build/10_authentication_and_authorization/10_oauth_2.0)

## 4&#46; Encryption

Encrypt and store the refresh token and realmId in persistent memory. Encrypt the refresh token with a symmetric algorithm (3DES or AES). AES is preferred. Store your AES key in your app, in a separate configuration file.

## 5&#46; Moving to production

  * Install ngrok.
  * Run `ngrok http 3000 -region=au` (use your region) and add the URI as a redirect_uri in Quickbooks dev tools for the Production environment.