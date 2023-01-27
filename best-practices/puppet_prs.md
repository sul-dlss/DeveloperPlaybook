# Puppet PRs

Occasionally, a developer may need a system level configuration change made to a machine. Perhaps the most common example is when a dev needs ssh access to a machine to be able to kick off a capistrano task. In this case, we encourage devs to make a pull request to the Puppet repository where such configuration is mannaged.

To issue a PR to the DLSS puppet repository, first make sure you have a local clone of the repo. Inside, check out a branch off of `production` on which to do your work. By convention the name of the branch should include your sunetid, so that operations knows who is issuing the PR. This is a convention used by ops to give us some visibility into whose work puppet is applying to a given machine:

```
git checkout -b sunetidFeature
```

The configuration to set up ssh access for a user on a server typically lives in the `hieradata/node` directory, where the file name is the fqdn of the server, followed by the `eyaml` extension. For example,

```
requests-dev.stanford.edu.eyaml
```

if you needed access to the requests development machine.  Within that file, you will find a hiera key for `k5users`, whose value is an array of sunetids. You will need to add your own sunetid to that hash like so:

```
role::params::app_user:
  requests:
    k5users:
      - mysunetid
      - yoursunetid
```

Next, add, commit, and push the change in your branch to GitHub for review, making sure to set your local feature branch to track a remote feature branch:

```
git add .
git commit
git push origin sunetidFeature
```

 Next, create a PR in GitHub for your changes. Once the PR has been submitted, the ops team will be automatically notified to review it. Once the PR has been reviewed and merged into the production branch by the ops team, the puppetmaster will apply the configuration on its next run, usually within an hour, or a friendly operations team member will apply the change to the server by running the puppet agent manually.

### Testing your puppet changes

If you need to have the ability on the development machine to do a test run of the proposed puppet changes, you need to have access to the machine as the root user. Add `ksu: true` to the `app_user` configuration as shown (note: this may already be applied):

```
role::params::app_user:
  requests:
    k5users:
      - mysunetid
      - yoursunetid
    ksu:
      true
```

Be aware that this will give root access to all of the `k5users`, so be certain that you actually need this level of access and that all of the k5users can have this level of access. Once the PR is submitted with this change the ops team will review and merge it as described above, and after the puppetmaster applies the configuration on the next run, the k5users will have root access to do test puppet runs.

The next time you create a new puppet branch and commit/push to the puppet Github repository, you will be able to do a test run of the proposed puppet configuration to see what changes would actually be applied. To do this use the `ksu` command on the server you are testing the puppet change on:

```
$ ksu
Authenticated mysunetid@stanford.edu
Account root: authorization for mysunetid@stanford.edu successful
Changing uid to root (0)
```

**NOTE**: You may need to source RVM into your root shell session and set the default Ruby as the system Ruby via:

```
$ source /etc/profile.d/rvm.sh && rvm use system
```

Now you have the ability to do a test puppet run. This will run the puppet agent against the branch that you pushed to puppet, but will not actually apply any of the changes. You will see a diff of changes that will be made when the PR is finally merged and puppetmaster picks up and applies the change on your server:

```
$ puppet agent --test --noop --environment=sunetidFeature
```
Note: "environment" is the name of your puppet branch, and is not related to a Rails environment.

:exclamation: WARNING :exclamation:
Please make sure that you use the `--noop` flag :exclamation: or else the puppet will try to apply the configuration from whatever environment is specified; in the example above
it would be from the `sunetidFeature` branch of the puppet repository.

You can do this to test your configuration changes in real-time, but be aware that puppet will then update the puppet config file (`/etc/puppetlabs/puppet/puppet.conf`) with the environment you just specified on the command line, and after that if somebody runs `puppet agent --test` (without the environment flag) it will assume the environment previously specified, so if you want to test your puppet runs in real-time, please make sure that you subsequently reset the puppet config to use the production environment with:

```
$ puppet agent --test --environment=production
```

...or else the next person who comes along will be confused as to why their puppet changes are not being propagated, or there may be errors because the environment's branch that you used is no longer available!

If you get a ruby-related error with the above, try:

```
$ source /etc/profile.d/rvm.sh && rvm use system
```
