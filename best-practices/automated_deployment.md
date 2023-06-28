# Automated deployment

The access team uses a process where most projects are configured to have Jenkins deploy them automatically. In the stage environment, the deploy occurs upon a push to the main git branch. On production, a new version tag in git, will trigger the deploy.

In order to install this you first must have a `Jenkinsfile` in your repository like https://github.com/sul-dlss/vt-arclight/blob/main/Jenkinsfile

We configure the SSH key authorization so that Jenkins can deploy on our servers in puppet. Under the user section ensure you have this:
```yaml
profile::app::user:
  <username>:
    auth_keys:
      jenkinsqa@sul-ci-prod:
        type: 'ssh-ed25519'
        key:
          '<YOUR KEY HERE>'
```

Browse the index at https://dlss-ci-prod.stanford.edu/job/SUL-DLSS/ to find your project and the jobs that have run for it.