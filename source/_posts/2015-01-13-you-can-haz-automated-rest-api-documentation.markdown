---
layout: post
title: "You Can Haz Automated REST API Documentation"
date: 2015-01-13 09:29
comments: true
categories: [SpyREST]
---

Let's face it, the manual process of documenting REST APIs sucks big time. The process goes like this:

1. Craft a cURL request for an API Endpoint.
2. Copy the response.
3. Narrate the API, paste the cURL and response on an editor.
4. Edit the response, describe necessary request and response headers, query parameters etc.
5. Repeat steps 1-4 for each of your tens of Endpoints.
6. Publish the documentation on the web.
7. Repeat these steps for each release.

This is a laborious process. However, it doesn't have to be this way. Meet [SpyREST](http://SpyREST.com) to see how it produces purely __organic__, __automated__, __replayable__, __published__, __version aware__ and __customizable__ API Documentation for you.

This is how it works:

1. You script the API calls using any scripting you prefer, including bash/cURL, automated test frameworks, etc. For example, using a test framework:

```
describe 'gists' do
  it "List a user's gists since a time:" do
    response = Github.get '/users/smsohan/gists?since=20140101T00:00:00Z', @common_options
    assert_equal 200, response.code
    refute_nil response.body
  end
end
```

2. Use the SpyREST http proxy `http://SpyREST.com:9081` to perform this call on a real API.
3. You get the same response, and SpyREST produces a fully baked API documentation as you see [here](http://spyrest.com/api_actions/details?api_action=GET+%2Fusers%2Fsmsohan%2Fgists&api_host=api.github.com&api_resource=gists&api_version=v3):

Try here if you want to see for yourself:

```
curl -k -x "http://spyrest.com:9081" -H "Accept: application/vnd.github.v3+json" "https://api.github.com/users/YOUR_GITHUB_USER_NAME/gists"
```

See the documentation at

[http://spyrest.com/api_resources/gists?api_host=api.github.com&api_version=v3](http://spyrest.com/api_resources/gists?api_host=api.github.com&api_version=v3)

Drop me a line if you'd like to use this to publish your API documentations.
