# Heroku buildpack for git-lfs

This is a [Heroku buildpack][buildpacks] which installs [Git
LFS][git-lfs] and downloads your Git LFS assets during deployment
(which Heroku does not do by default).

## Usage

To configure Git LFS for your Heroku app called `<myapp>`, run:

    $ heroku buildpacks:add                                   \
        https://github.com/raxod502/heroku-buildpack-git-lfs  \
        -a <myapp>

Set the following environment variable for your app:

* `BUILDPACK_GIT_LFS_REPO` to the clone URL of the repository
  from which to download Git LFS assets. This should include any
  username, password, or personal access token which is necessary to
  clone noninteractively. See [here][noninteractive-clone] for
  details on the syntax. It must be something like `git@github.com:remix/remix`
* `BUILDPACK_GIT_LFS_SSH_PRIVATE_KEY`: your private key encoded in base64 with `base64 -w 0`. You
  can use `heroku config:set --app preprod-bureauxlocaux "BUILDPACK_GIT_LFS_SSH_PRIVATE_KEY=$(cat
  ~/.ssh/heroku_deploy_lfs | base64 -w 0)"` to set it.
* `BUILDPACK_GIT_LFS_INCLUDE_PATHS`: (OPTIONAL) a comma-separated list of (relative) paths to
  include in the fetch. Passed via the `--include` flag to `git lfs pull`
* `BUILDPACK_GIT_LFS_EXCLUDE_PATHS`: (OPTIONAL) a comma-separated list of (relative) paths to
  exclude in the fetch. Passed via the `--exclude` flag to `git lfs pull`


After the next time you deploy your app, Git LFS assets will be
downloaded and checked out automatically, and Git LFS will be
available on `PATH` for your app.

Inspiration was taken from [the original repo](https://github.com/raxod502/heroku-buildpack-git-lfs)
and [a buildpack to use SSH keys][git-ssh-key-buildpack]. This repository allows you to rely on
deploy SSH keys to pull LFS files instead of requiring tokens. Also, added support to optionally
specify relative paths of files to include/exclude when fetching lfs objects.

[buildpacks]: https://devcenter.heroku.com/articles/buildpacks
[git-lfs]: https://git-lfs.github.com/
[heroku-buildpack-apt]: https://github.com/heroku/heroku-buildpack-apt
[noninteractive-clone]: https://stackoverflow.com/a/50193010/3538165
[git-ssh-key-buildpack]: https://github.com/poetic-labs/git-ssh-key-buildpack
