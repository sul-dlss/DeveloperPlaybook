# Ruby Style Enforcement

It is recommended that you use a ruby style enforcement tool such as [RuboCop](/bbatsov/rubocop). This can be integrated in your CI build and optionally use a service like HoundCI to comment on Pull Requests indicating violations.

There is a RuboCop configuration file available in this directory that can be used as a base for a new project.  There are various modifications in that configuration that we have collaboratively determined are acceptable in DLSS.


## Adding to an existing project

One quick way to get started using RuboCop without having an overwhelming number of violations to deal with is to use an [Automatically Generated Configuration](https://github.com/bbatsov/rubocop#automatically-generated-configuration) and work on resolving the violations in the generated .rubocop_todo.yml file.  This ensures no new violations will be introduced and existing violations can be addressed when prudent.

## Contributing

If you would like to propose an addendum to the base configuration please submit a Pull Request with the proposed changes.
