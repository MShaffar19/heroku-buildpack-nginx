Heroku buildpack: nginx
=======================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpack) for nginx.
The nginx is build using the source form (http://nginx.org/download/nginx-<nginx_version>.tar.gz).
Built binary is cached to speedup future builds.

Usage
-----

Example usage:

    $ ls
    conf/  _nginx.cfg
    $ ls -R *
    conf:
    nginx.conf.erb

    $ cat _nginx.cfg
    export NGINX_VERSION=1.9.5

    $ heroku create --stack cedar --buildpack https://github.com/abhishekmunie/heroku-buildpack-nginx.git
    ...

    $ git push heroku master
    ...
    -----> Fetching custom buildpack... done
    -----> Nginx app detected
    -----> Vendoring nginx 1.3.15... done
    -----> Creating startup script... done
    -----> Creating Procfile... done
    -----> Discovering process types
    ...

The buildpack will detect your app as nginx if it has the file
`nginx.conf.erb` in the `conf` directory. You must define all `listen`
directives as `listen <%= ENV['PORT'] %>;` and also include `daemon off;` in
order for this buildpack to work correctly.

To configure the version of nginx used, modify the _nginx.cfg file in the root
directory.

To start the server run `bin/start_nginx`.
If no `Procfile` is present buildpack will create one with `web: bin/start_nginx`

As an alternative to the above instructions you may wish to investigate
[heroku-buildpack-multi](https://github.com/ddollar/heroku-buildpack-multi)
in order to support more complex use-cases such as compiling a static site
that is served by nginx or placing nginx in front of app server processes.

Logs
----

The sample nginx configuration is set to log to stdout.

Hacking
-------

To modify this buildpack, fork it on Github. Push up changes to your fork, then
create a test app with `--buildpack <your-github-url>` and push to it.
