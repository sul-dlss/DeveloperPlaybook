# Updating NodeJS

NodeJS can be updated on our deployment servers by updating the version in a Puppet hieradata file:

```diff
nodejs:
-   install_options: '--disablerepo=centos-scl'
- npm:
-   install_options: '--disablerepo=centos-scl'
+   repo: 'nodejs9'
```

After updating (from 0.x -> 9.9), run manually on the server (as ksu): 

```sh
yum remove npm
yum upgrade nodejs
```

This is needed as NodeJS started bundling NPM at some point and it wasn't a separate package. [Source needed here but can't find it]
