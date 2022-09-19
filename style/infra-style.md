# Infrastructure Team Style Guide Principles

## Why Do We Care

When developers grok each others code faster, we can do our work faster.  We read code more often than we write it.

Code readers can be all of these:  the language guru, the developer new to the language, the inexperienced developer, the expert developer, the sysadmin, the product owner ...

It can be our _own_ code, written earlier, that doesn't make sense.

From https://www.techtarget.com/searchsoftwarequality/feature/Follow-Googles-lead-with-programming-style-guides:

When code is consistent and readable across the organization, Shaindel Schwartz advises, engineers see these benefits:
- ability to focus on what's getting done, rather than the minutiae of how it's coded;
- quick understanding of any codebase through familiar interfaces and formats;
- simpler scaling and implementation of automation; and
- easier change, such as product ownership transfers, new personnel onboarding and code handoffs.


## Our Goals:
- improve maintainability
- reduce cognitive load when reading code; we grok it faster if choices are consistent across codebases
- avoid mistakes, e.g. typos
- continually improve code
- follow best practices in broader community
- avoid disagreements about style

- do as much as possible with automated tools

## Tools

Ruby:
  - [rubocop](https://rubocop.org/) [cop documentation](https://docs.rubocop.org/rubocop) [github](https://github.com/rubocop/rubocop)
  - [rubocop-rspec cop documentation](https://docs.rubocop.org/rubocop-rspec/cops.html) [github](https://github.com/rubocop/rubocop-rspec)
  - [rubocop-rails cop documentation](https://docs.rubocop.org/rubocop-rails/cops.html) [github](https://github.com/rubocop/rubocop-rails)
  - (not often used by us)[rubocop-performance cop documentation](https://docs.rubocop.org/rubocop-performance/cops_performance.html) [github](https://github.com/rubocop/rubocop-performance)

We want these tools to be exercised when a pull request is created, at a minimum.


## Editing existing codebases

Most of our work is editing existing code bases, whether they are 3 months old or 15 years old.

### RULE:  Follow the existing rules.  Strive for no new exceptions.

### Should it be a goal to reduce .rubocop_todo.yml to empty?

(TBD)


## Starting a New Code Base

Start with default style guides + exceptions below.


## When Style exceptions are acceptable

For long classes/methods/blocks, consider SOLID:

    Single responsibility
    Open-Closed (open for extension, closed for modification)
    Liskov substitution
    Interface segregation (clients should not be forced to depend of interfaces they don't use)
    Dependency inversion (depend on abstractions, not concretions)

However, refactoring can reduce readability:
- the reader now has to jump around more in the code without a clear gain in readability
- method/class names matter!

### When can we turn a rule off?

(TBD)


## What to do when keeping an exception

(TBD)

- Prefer an inline exception to one in .rubocop.yml
   - On same line if it's a single line to be excepted and one rule
   - Above disable, below disable if multiple rules or multiple lines excepted
- Consider a comment indicating why ("all this logic belongs together")


## How / When apply new linter rules?

For rubocop, this means adding new rules to the config file.

Guidance:
- when warnings are annoying
- when it's more than a handful
- when working in the code base for whatever reason

### What if there are failures due to new rule?

(TBD)


# Unenforceable Styles

## On Comments:
- if you read the code out loud and it says what the comment says -- the comment is superfluous


## On Names:

Good names make code a lot easier to read.  Choose illustrative names for methods, for variables, for classes.  This can be hard.

(the below TBD)

- Prefer the positive case to the negative. `foo_valid?` rather than `foo_invalid?`
- Avoid abbreviations. `foo_with_bar` rather than `foo_w_bar`
- Prefer shorter over longer if intent is revealed clearly. `wingnuts_colors` rather than `colors_in_wingnuts_coat`

If you're stuck, say out loud or write down what it's for/what it does as if you're explaining it to a colleague.  Pick out appropriate words to use.  You can ask for help in the PR:  "Do [the new method names / this method name / this variable name] work for readability?  Feel free to suggest improvements."

https://www.elpassion.com/blog/naming-101-programmers-guide-on-how-to-name-things
https://medium.com/swlh/naming-conventions-101-for-developers-8997bb96fd60
