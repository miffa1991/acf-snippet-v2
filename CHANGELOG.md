# Change Log

## 1.2.0

### Added
- **`field:url`** snippet for handling ACF URL fields.  
  Example: Outputs a clickable `<a>` tag with the URL from an ACF field.
- **`field:page_link`** snippet for handling ACF Page Link fields.  
  Example: Outputs a clickable `<a>` tag with the page link from an ACF field.
- **`field:page_link:multiple`** snippet for handling multiple ACF Page Link fields.  
  Example: Outputs a list of links when multiple pages are selected.

## 1.1.0

### Added
- **`field:link:array`** snippet for handling ACF link fields returned as arrays.  
  Example: Creates a button with `url`, `title`, and optional `target` attribute handling.

### Changed
- **`field:link`** snippet improved to output link text as `Continue Reading` using `esc_html()` for better security and flexibility.

## 1.0.0

- Initial release
