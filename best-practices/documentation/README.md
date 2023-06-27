# Documentation

## Ruby
Ruby projects should use [YARD](http://yardoc.org/) for documentation purposes.

[Getting Started with YARD](http://www.rubydoc.info/gems/yard/file/docs/GettingStarted.md)

One benefit is YARD runs a live documentation server for the community, hosting docs for all RubyGems and Github projects. The project is at [RubyDoc.info](http://www.rubydoc.info/) (formerly rdoc.info)

A [useful Yardoc cheatsheet](https://gist.github.com/chetan/1827484)

## HTTP API

If your project exposes an HTTP API, you should consider documenting the API using the [OpenAPI Specification](https://swagger.io/docs/specification/about/) (formerly known as Swagger Specification). The OpenAPI Specification allows for rich description of HTTP APIs&mdash;paths, parameters, responses, headers, status codes, etc.&mdash;in machine-readable formats.

### Specification Formats

OpenAPI Specifications may be expressed as either YAML or JSON. Thus far, we have preferred YAML over JSON because it allows commenting and, unlike JSON, does not care about e.g., trailing commas.

### API Documentation Examples

* [SDR ("Cocina") Data Models](https://sul-dlss.github.io/cocina-models/)
* [SDR Deposit API](https://sul-dlss.github.io/sdr-api/)
* [SDR Identifier API](https://sul-dlss.github.io/suri-rails/)
* [SDR Indexing API](https://sul-dlss.github.io/dor_indexing_app/)
* [SDR Preservation API](https://sul-dlss.github.io/preservation_catalog/)
* [SDR Repository ("DOR Services") API](https://sul-dlss.github.io/dor-services-app/)
* [SDR Technical Metadata API](https://sul-dlss.github.io/technical-metadata-service/)
* [SDR Workflow API](https://sul-dlss.github.io/workflow-server-rails/) (**coming soon**!)

### How To Add OpenAPI To A Project

Here are the steps we have been following:

1. Create `openapi.yml`: feel free to copy from an existing project, paste, and adjust ([example](https://github.com/sul-dlss/technical-metadata-service/blob/main/openapi.yml))
1. Validate `openapi.yml` in the CI build ([CircleCI example](https://github.com/sul-dlss/technical-metadata-service/blob/6c8151b1ca713061c227e8030f03c6531eee1093/.circleci/config.yml#L103-L114))
1. Add OpenAPI validation badge to the README ([example](https://github.com/sul-dlss/technical-metadata-service/blob/6c8151b1ca713061c227e8030f03c6531eee1093/README.md#L5))
1. Add Committee gem, which allows automagic validation of requests and responses based on your API specification, as a dependency ([example](https://github.com/sul-dlss/technical-metadata-service/blob/6c8151b1ca713061c227e8030f03c6531eee1093/Gemfile#L9))
1. Enable request validation by Committee, then fix all the stuff that breaks as a result of shifting the burden of request validation from your controllers to the Committee middleware ([example](https://github.com/sul-dlss/technical-metadata-service/blob/6c8151b1ca713061c227e8030f03c6531eee1093/config/application.rb#L47-L51))
    * NOTE: If request validation causes more breakage than you are prepared to fix, comment out the request validation code and write up a new issue to do this later
1. Enable response validation by Committee, then fix all the stuff that breaks as a result of shifting the burden of response validation from your controllers to the Committee middleware ([example](https://github.com/sul-dlss/technical-metadata-service/blob/6c8151b1ca713061c227e8030f03c6531eee1093/config/application.rb#L52))
    * NOTE: If response validation causes more breakage than you are prepared to fix, comment out the response validation code and write up a new issue to do this later
1. Create `docs/` folder in the repo and copy the `docs/index.html` artifact from another repo, *e.g.*, technical-metadata-service, which will parse your OpenAPI specification and generate pretty HTML documentation to be served by Jekyll (via GitHub Pages) in the next step ([example](https://github.com/sul-dlss/technical-metadata-service/tree/6c8151b1ca713061c227e8030f03c6531eee1093/docs))
1. Enable GitHub Pages with the `docs/` folder as the source (and don't use the `Theme Chooser` because `docs/index.html` already has its own style) in the Settings of the GitHub repository
1. Verify that the automagic OpenAPI documentation works: see examples above
1. Add the GitHub Pages-based OpenAPI URL for your repository (*e.g.*, `https://sul-dlss.github.io/suri-rails/`) as the "Website" of the GitHub repository
