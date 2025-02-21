# Change Log

## 1.3.0

### Added
- **`field:map`** snippet for handling ACF Google Map fields.
  Example: Outputs a map with custom marker and zoom level settings.
- **`field:taxonomy`** snippet for handling ACF taxonomy fields.
  Example: Displays a list of taxonomy terms with links to their archive pages.
- **`field:image:array`** improved with better conditional check and proper escaping.
- **`field:gallery:captions`** snippet for handling gallery images with captions.
  Example: Displays gallery images in a grid with optional captions using the <figure> and <figcaption> elements.
- **`field:cf7``** snippet for integrating ACF fields with Contact Form 7.
  Example: Retrieves form ID from ACF field and displays the form using do_shortcode() with proper escaping.

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
