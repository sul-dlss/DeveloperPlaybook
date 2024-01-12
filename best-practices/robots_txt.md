# robots.txt files

There are a couple of different ways to set up a `robots.txt` file to instruct web robots how to crawl pages on our websites. This documents each way to do that.

Taken together, these methods should work for any application where we want a `robots.txt`.

## Setting up a robots.txt via github/capistrano

If the application is deployed via github/capistrano you can include a file named `robots.txt` in the application repo's public directory. For example, see [argo](https://github.com/sul-dlss/argo/blob/main/public/robots.txt).

## Setting up a robots.txt via puppet

If the previous method won't work, or if you prefer, you can instead find puppet's hieradata for the application, and include something like the following block of code:
```
profile::webserver::vhosts:
  vhost_name:
    robots_txt:
      present: true
      lines:
        - 'User-agent: *'
        - 'Crawl-delay: 10'
```
That is, under whatever your `vhost_name:` is, include a `robots_txt:` hash, with at least `present: true` defined. This will create an empty robots.txt, and rules can be added via the `lines:` hash. For example, see [was-pywb](https://github.com/sul-dlss/puppet/blob/production/hieradata/node/was-pywb-prod.stanford.edu.eyaml).