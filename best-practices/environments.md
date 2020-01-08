# Environments

This document clarifies the terminology we use to talk about deployment & testing environments. 

Note that there are at least two separate needs from non-production environments:
* testing software: as we are developing software, we need a way to appropriately test and approve code & configuration before we deploy it to production.
* testing data: we have a variety of situations where bulk loading of data requires that the data itself be vetted before it is loaded or changed in production.

Sometimes these two different needs can share the same environment, but when software changes would disrupt the need for data vetting, this can become problematic.

This document is focused on environments for testing **software**.

<dl>
  <dt>Production (or "prod") </dt>
  <dd>The production environment is the one targeted for users and production data. These environments tend to be the most robustly provisioned, and also to hold data that we care about, <em>e.g.</em>, that gets preserved to SDR and backed up regularly.</dd>
  <dt>Staging (or "stage") </dt>
  <dd>The staging environment mimics production closely, and serves as a last step in testing a feature or set of features against production-like data after the work has undergone initial (user acceptance/quality assurance) testing and it has been merged. Projects **SHOULD** have staging environments.</dd>
  <dt>User Acceptance Testing ("UAT") & Quality Assurance ("QA")</dt>
  <dd>UAT and QA environments are used to test a feature or set of features early on, as part of development work and may also be used by service team members, data managers, designers, etc. to do their work outside of development work cycles. These environments may or may not have production-like data. We tend to use UAT environments for sites needing testing beyond the department, and QA for internal testing. Projects **MAY** have UAT and/or QA environments.</dd>
</dl>
