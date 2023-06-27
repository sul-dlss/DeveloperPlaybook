# Infrastructure Team Accessibility Guide

The Infrastructure Team's current approach (as of mid-2023) to accessibility work is 1) to increase access by taking on prioritized accessibility issues in whatever our current work cycle is, and 2) to adopt a "first, do no harm" strategy when changing applications with user-facing web views. Here's how this strategy works.

Whenever working on a user-facing web view, a developer should run an accessibility tool (see [`Tools`](#tools) below) on it before you start your work, noting the results. Then after finishing the work, a developer should run it again to ensure no new accessibility violations have been introduced. For now, the team is focusing effort on violations at the WCAG A and AA [conformance levels](https://www.w3.org/TR/UNDERSTANDING-WCAG20/conformance.html#uc-conformance-requirements-head), not the AAA level. If a developer wishes to include one or more additional commits that also remove existing violations, the team will happily support this.

## Tools

* [Siteimprove Accessibility Checker (browser extension)](https://www.siteimprove.com/integrations/browser-extensions)
