Heroku buildpack: Environment variables
=======================================
This buildpack allows manipulating environment variables.
It does this by running the `source` bash-command on the file `bin/envvars` in
your apps repository.

If your repository does not have the `bin/envvars`-file, the buildpack will
fail.

By default the following environment variables will be availble to `bin/envvars`:
* `$BUILD_DIR`
   The repository directory. When the build ends (after all buildpacks have
   been exectued) it ends up in `/app/`, but as the script is invoked during
   the build process, it is in a temporary directory.
* `$CACHE_DIR`
  The package (for npm, pip etc.) cache directory that is persisted between
  builds.
* `$ENV_DIR`
  _to be described_
* Your normal environment variables set via `heroku config:set` are _not_
  available. They are not made available by Heroku during the build process,
  they are only made available once the app starts.
* Any environment variables set by other buildpacks (and passed on via the `exports`-file)
  are available

<!-- BP_DIR=$(cd $(dirname $0); cd ..; pwd) -->

Usage
-----

Example usage:

    $ cat bin/envvars
    # Print current environment
    env
    # Set `CPATH` environment variable to include `xmlsec1`
    export CPATH=$BUILD_DIR/.apt/usr/include/xmlsec1:$CPATH

    $ heroku buildpacks:set --app example-app https://github.com/heroku/heroku-buildpack-multi.git
    $ heroku buildpacks:add --app example-app --index 1 https://github.com/heroku/heroku-buildpack-apt.git
    $ heroku buildpacks:add --app example-app --index 2 https://github.com/malthejorgensen/heroku-buildpack-envvars.git
    $ heroku buildpacks:add --app example-app --index 3 https://github.com/malthejorgensen/heroku-buildpack-python.git

    $ git push heroku master
    TODO: Correct output below
    ...
    -----> Heroku receiving push
    -----> Fetching custom buildpack
    -----> HelloFramework app detected
    -----> Found a hello.txt
