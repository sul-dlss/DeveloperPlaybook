# README.md

All projects should have a `README.md` file. This file should try to answer the following questions:

- What it is
- External requirements
- How to install it
- How to use it
- How to deploy it (if appropriate)
- How to run the tests
- Include relevant badges:
    - Build status
      - [GitHub](https://github.com/features/actions); or
      - [CircleCI](https://circleci.com/docs/2.0/status-badges/)
    - Current version (if a published package)
      - E.g., if Ruby: [Gem Version](http://badge.fury.io/for/rb)
    - Test coverage
      - [CodeCov](https://app.codecov.io); or
      - [CodeClimate](https://codeclimate.com/)
    - API validation (if project declares an HTTP API using the [OpenAPI Specification](http://spec.openapis.org/oas/v3.0.2))
      - [OpenAPI validator](http://validator.swagger.io/validator)
    - Docker image (if project publishes one or more docker images)
      -  [Microbadger](https://microbadger.com/#badges)

## Example
    [![Build Status](https://travis-ci.org/sul-dlss/name-of-the-project.svg?branch=main)](https://travis-ci.org/sul-dlss/name-of-the-project)
    [![Code Climate](https://codeclimate.com/github/sul-dlss/name-of-the-project/badges/gpa.svg)](https://codeclimate.com/github/sul-dlss/name-of-the-project)
    [![Code Climate Test Coverage](https://codeclimate.com/github/sul-dlss/name-of-the-project/badges/badges/coverage.svg)](https://codeclimate.com/github/sul-dlss/name-of-the-project/badges/coverage)
    [![Dependency Status](https://gemnasium.com/sul-dlss/name-of-the-project.svg)](https://gemnasium.com/sul-dlss/name-of-the-project) [![Gem Version](https://badge.fury.io/rb/name-of-the-project.svg)](http://badge.fury.io/rb/name-of-the-project)
    [![OpenAPI Validator](http://validator.swagger.io/validator?url=https://raw.githubusercontent.com/sul-dlss/name-of-the-project/main/openapi.yml)](http://validator.swagger.io/validator/debug?url=https://raw.githubusercontent.com/sul-dlss/name-of-the-project/main/openapi.yml)
    [![Docker image](https://images.microbadger.com/badges/image/suldlss/name-of-the-project.svg)](https://microbadger.com/images/suldlss/name-of-the-project "Get your own image badge on microbadger.com")

    # Name of the Project

    This project provides an implementation of the [XKCD Random Number](https://xkcd.com/221/) generator. This random number generator is particularly useful when predictable random numbers can improve performance.

    ## Requirements

    - Ruby 3.0
    - libxml 5

    ## Installation

    Add this line to your Rails application's Gemfile

    ```ruby
      gem 'xkcd-random'
    ```

    Then execute:

    ```console
    $ bundle install
    ```

    In your code, record the random number:

    ```
    XKCD::Random.number = 4 # chosen by a fair die roll
    ```

    ## Running the tests

    Run tests:

    ```console
    $ rake
    ```

## Other Examples

- [spotlight](https://github.com/sul-dlss/spotlight)
- [triannon](https://github.com/sul-dlss/triannon)
- [revs](https://github.com/sul-dlss/revs)
- [exhibits-request](https://github.com/sul-dlss/exhibits-request)
