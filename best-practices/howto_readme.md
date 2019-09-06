# README.md

All projects should have a `README.md` file. This file should try to answer the following questions:

- What it is
- External requirements
- How to install it
- How to use it
- How to deploy it (if appropriate)
- How to run the tests
- Include relevant badges:
    - [Travis](http://docs.travis-ci.com/user/status-images/)
    - [Gem Version](http://badge.fury.io/for/rb)
    - Coverage ([codeclimate](https://codeclimate.com/))
        - [Getting Started](https://docs.codeclimate.com/docs)


## Example
    [![Build Status](https://travis-ci.org/sul-dlss/name-of-the-project.svg?branch=master)](https://travis-ci.org/sul-dlss/name-of-the-project) 
    [![Code Climate](https://codeclimate.com/github/sul-dlss/name-of-the-project/badges/gpa.svg)](https://codeclimate.com/github/sul-dlss/name-of-the-project)
    [![Code Climate Test Coverage](https://codeclimate.com/github/sul-dlss/name-of-the-project/badges/badges/coverage.svg)](https://codeclimate.com/github/sul-dlss/name-of-the-project/badges/coverage) 
    [![Dependency Status](https://gemnasium.com/sul-dlss/name-of-the-project.svg)](https://gemnasium.com/sul-dlss/name-of-the-project) [![Gem Version](https://badge.fury.io/rb/name-of-the-project.svg)](http://badge.fury.io/rb/name-of-the-project)

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
