# Code Review

## Tools

* Continuous integration tools automatically ensure the tests pass (e.g. travis, jenkins)
* Test coverage tools can inform QA/verifiability, reflecting the relative completeness of test/CI results (e.g. coveralls, code cov, code climate)
* Documentation coverage tools can inform documentation extent (e.g. inch)
* Doc tools can detect formal documentation errors (e.g. yard)
* Audit tools can inform security analysis, including across the dependency tree (e.g. bundle_audit, gemnasium)
* Static analysis can detect both flaws/deprecations and style-compliance (e.g. rubocop, code climate, reek)
* Style tools can normalize logically equivalent expressions into fewer conventional forms (e.g. linters, rubocop)

In ALL cases, the uses of these tools (configs, example invocations, etc.) should be explicitly documented as part of the project.  (e.g. README, badges …)
