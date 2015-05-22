# Licensing

As a matter of practice, DLSS publishes software into publicly accessible code repositories. This facilitates the review, exchange, reuse and possible code contributions from other sites--a key part of our development strategy and methodology. As best practice, we endeavor to put a clear license on this code so others know what they may and may not do with it. DLSS staff should release it under an open source license.

- When contributing to an existing codebase that has an approved OSS license, we should contribute the code back under the this same license.
- When developing new code, then it should use an Apache 2 license as the default.
- The copyright holder for most work is "The Board of Trustees of the Leland Stanford Junior University"

## Example

This text should be placed in `./LICENSE` within your project (this is true for Ruby / Rails, but other conventional locations may exist for other types of projects):

```
Copyright 2015 The Board of Trustees of the Leland Stanford Junior University

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

## Special note for Ruby gems

In addition to adding the `LICENSE` file, you should also specify the license in your `gemspec`:

```ruby
Gem::Specification.new do |s|
  s.name        = 'example'
  s.version     = '0.1.0'
  s.license     = 'Apache-2.0'
end
```
> [license=](http://guides.rubygems.org/specification-reference/#license=)
