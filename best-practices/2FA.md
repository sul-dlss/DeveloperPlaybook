# Enabling multi-factor authentication (a.k.a. MFA, Two-factor Auth, 2FA, etc)

## What is it?

https://en.wikipedia.org/wiki/Multi-factor_authentication

> Multi-factor authentication is an authentication method in which a computer user is granted access only after successfully presenting two or more pieces of evidence (or factors) to an authentication mechanism: knowledge (something the user and only the user knows), possession (something the user and only the user has), and inherence (something the user and only the user is).

MFA (multi-factor authentication) and 2FA (two factor authentication) are often used interchangably, since typically only two factors (a password and something else) are required on each login (even if many additional factors besides password are configured to be usable).

## Why enable it?

https://en.wikipedia.org/wiki/Multi-factor_authentication#Authentication_factors

> The use of multiple authentication factors to prove one's identity is based on the premise that an unauthorized actor is unlikely to be able to supply the factors required for access. If, in an authentication attempt, at least one of the components is missing or supplied incorrectly, the user's identity is not established with sufficient certainty and access to the asset (e.g., a building, or data) being protected by multi-factor authentication then remains blocked.

### Practical examples of what can happen when an account gets compromised (e.g. because a developer account gets compromised due to crackable password or shared password turning up in a data dump from another hack)

_Backdoor code found in 11 Ruby libraries_ -- https://www.zdnet.com/article/backdoor-code-found-in-11-ruby-libraries/

> Maintainers of the RubyGems package repository have yanked 18 malicious versions of 11 Ruby libraries that contained a backdoor mechanism and were caught inserting code that launched hidden cryptocurrency mining operations inside other people's Ruby projects. _[the widely used rest-client gem was among the hacked projects]_

_Compromised JavaScript Package Caught Stealing npm Credentials_

> A hacker has gained access to a developer's npm account and injected malicious code into a popular JavaScript library, code that was designed to steal the npm credentials of users who utilize the poisoned package inside their projects.  The JavaScript (npm) package that got compromised is called eslint-scope, a sub-module of the more famous ESLint, a JavaScript code analysis toolkit.

## Services to protect

DLSS developers should enable multi-factor authentication for Github, as well as any package or image management repositories to which they have push access, and any other accounts which might be valuable targets for compromise.  See documentation for each individual service to configure its specific MFA setup.

A non-exhaustive list of services for which MFA should be enabled
* Github
* Rubygems
* NPM
* Dockerhub
* AWS

Stanford accounts (e.g. Shib/webauth, VPN access) should already require multi-factor auth.

### Gotchas

**Be sure to store your recovery code(s) in a secure location (secure from both theft and damage).**

You may also want to do at least one of the following for each service on which 2FA is enabled:
* if the service allows it, register multiple different additional factors that wouldn't all get lost together (e.g. register an additional FIDO key stashed somewhere safe, link to an authenticator app, etc)
* connect a recovery account, if that's an option

For most or all services, if you lose access to your usual second factor (whether it's a Yubikey or an authenticator app on your phone), you will need to use recovery codes or a different second factor to prove your identity upon login.  If the recovery codes and all registered additional auth factors are lost, _you will likely be unable to login to the account_.
