# Environments

This document clarifies the terminology we use to talk about deployment environments.

<dl>
  <dt>Production (or "prod") </dt>
  <dd>The production environment is the one used by most end-users. These environments tend to be the most robustly provisioned, and also to hold data that we care about, *i.e.*, that gets preserved to SDR and backed up regularly.</dd>
  <dt>Staging (or "stage") </dt>
  <dd>The staging environment mimics production closely, and serves as a last step in testing a feature or set of features against production-like data after the work has undergone initial (user acceptance/quality assurance) testing and it has been merged.</dd>
  <dt>User Acceptance Testing ("UAT") & Quality Assurance ("QA")</dt>
  <dd>UAT and QA environments are used to test a feature or set of features early on, as part of development work. These environments may or may not have production-like data. We tend to use UAT environments for sites needing testing beyond the department, and QA for internal testing.</dd>
</dl>
