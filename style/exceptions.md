# Style

## Process for Declaring Exceptions

Please make exceptions in either `.rubocop.yml` or `.rubocop_todo.yml` — depending on whether your team considers that exception an acceptable one or one to fix — instead of making exceptions inline (using `rubocop:disable` syntax immediately before the violation). This way, there are two well-known files to check to get a full picture of how fully a codebase conforms to the style guide.

Please also make exceptions as small as possible:

* If you can declare an exclusion to a single file, do so; else
* If you can declare exclusions to multiple files, do so; else
* If you can declare an exclusion to a directory or directory hierarchy, do so; else
* If you can declare an exclusion to multiple directories or directory hierarchies, do so.
* These are all preferable to blanket disabling the rule.
