# Twitter Bootstrap

When building front-end interfaces, we should use [Twitter Bootstrap](http://getbootstrap.com/) to provide a base layout, common set of user interface components, and easily sharable SUL branding.

Projects should override bootstrap variables (following SCSS conventions) to customize the Bootstrap theme, e.g.:

```scss
# variables.scss
$cardinal-red: #8c1515;

$btn-primary-color: $white;
$btn-primary-bg: $cardinal-red;
$btn-primary-border: darken($btn-primary-bg, 9.8%);

@import 'bootstrap-variables';
```

## Additional Recommendations for Rails projects

Rails projects should use the [`bootstrap-sass`](https://github.com/twbs/bootstrap-sass) and [`bootstrap_form`](https://github.com/bootstrap-ruby/rails-bootstrap-forms) gems, which integrate well with the Rails asset pipeline and Rails form helpers.

## Examples

- [Searchworks](https://github.com/sul-dlss/searchworks)
- [Earthworks](https://github.com/sul-dlss/earthworks/)
- [Requests](https://github.com/sul-dlss/sul-requests)
- [Exhibits](https://github.com/sul-dlss/sul-exhibits-template)
