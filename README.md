# Snippets for Visual Studio Code : Advanced Custom Fields

## Snippets

All tab triggers follow the following naming convention; `field:{field type}:{type/option}`. All fields also have appropriate tabstops setup, however the first will always be the field name.

### Basic Fields

`field` / `field:header` / `field:text` / `field:link` / `field:option` **(HTML/PHP)**

Get a field by name.  
- **field** – general variant (no HTML tags).  
- **field:text** – wraps the content in `<p>`.  
- **field:header** – wraps the field in a chosen header tag (`<h1>`, `<h2>`, etc.).  
- **field:link** – outputs a link to the field's returned URL.  
- **field:option** – same as `field` but uses ACF options (`'option'`).


```php
<?php if ( get_field('field_name') ) : ?>
  <?php the_field('field_name'); ?>
<?php endif; ?>
```

**`field:date` (HTML/PHP)**

Get and format a date field

```php
<?php if ( get_field('field_name') ) : $date = DateTime::createFromFormat('Ymd', get_field('field_name')); ?>
  <?php echo $date->format('d-m-Y'); ?>
<?php endif; ?>
```

**`field:if` / `field:ifelse` (HTML/PHP)**

Field conditional. Also used for true/false fields.

```php
<?php if ( get_field('field_name') ) : ?>
<?php endif; ?>
```

**`field:sub` (HTML/PHP)**

Get a field by name, within repeater/flexible.

```php
<?php if ( get_sub_field('field_name') ) : ?>
  <?php the_sub_field('field_name'); ?>
<?php endif; ?>
```

### Image Field

**`field:image` (HTML/PHP)**

Image field with a return value of "Image URL"

```php
<?php if ( get_field('field_name') ) : ?>
    <img src="<?php the_field('field_name'); ?>">
<?php endif; ?>
```

**`field:image:id` (HTML/PHP)**

Image field with a return value of "Image ID"

```php
<?php
if ( get_field('field_name') ) {
  $attachment_id = get_field('field_name');
  $size = 'full'; // (thumbnail, medium, large, full or custom)
  echo wp_get_attachment_image( $attachment_id, $size );
}
?>
```

**`field:image:array` (HTML/PHP)**

Image field with a return value of "Image Array"

```php
<?php if ( get_field('field_name') ) : $image = get_field('field_name'); ?>
  <img src="<?php echo esc_url($image['url']); ?>" alt="<?php echo esc_attr($image['alt']); ?>" />
<?php endif; ?>
```

**`field:image:array:example` (HTML/PHP)**

Image field with a return value of "Image Array example"

```php
<?php
$image = get_field('image');
if( $image ):

    // Image variables.
    $url = $image['url'];
    $title = $image['title'];
    $alt = $image['alt'];
    $caption = $image['caption'];

    // Thumbnail size attributes.
    $size = 'thumbnail';
    $thumb = $image['sizes'][ $size ];
    $width = $image['sizes'][ $size . '-width' ];
    $height = $image['sizes'][ $size . '-height' ];

    // Begin caption wrap.
    if( $caption ): ?>
        <div class="wp-caption">
    <?php endif; ?>

    <a href="<?php echo esc_url($url); ?>" title="<?php echo esc_attr($title); ?>">
        <img src="<?php echo esc_url($thumb); ?>" alt="<?php echo esc_attr($alt); ?>" />
    </a>

    <?php
    // End caption wrap.
    if( $caption ): ?>
        <p class="wp-caption-text"><?php echo esc_html($caption); ?></p>
        </div>
    <?php endif; ?>
<?php endif; ?>
```

### File Field

**`field:file` (HTML/PHP)**

File field with a return value of "File URL"

```php
<?php if ( get_field('field_name') ) : ?>
  <a href="<?php the_field('field_name'); ?>" >Download File</a>
<?php endif; ?>
```

**`field:file:id` (HTML/PHP)**

File field with a return value of "File ID"

```php
<?php
  if ( get_field('field_name') ) :
    $attachment_id = get_field('field_name');
    $url = wp_get_attachment_url( $attachment_id );
    $title = get_the_title( $attachment_id );
?>
  <a href="<?php echo $url; ?>" ><?php echo $title; ?></a>
<?php endif; ?>
```

**`field:file:object` (HTML/PHP)**

File field with a return value of "File Object"

```php
<?php if ( get_field('field_name') ) : $file = get_field('field_name'); ?>
  <a href="<?php echo $file['url']; ?>"><?php echo $file['title']; ?></a>
<?php endif; ?>
```

### Flexible Content Field

**`field:flex` (HTML/PHP)**

Flexible Content basic field returns 1 row deep:

```php
<?php if ( have_rows( 'field_name' ) ) : ?>
    <?php while ( have_rows('field_name' ) ) : the_row();
        if ( get_row_layout() == 'layout_field' ) : ?>
            <div class="class">
                <?php the_sub_field( 'field_name' ); ?>
            </div>
        <?php endif; ?>
    <?php endwhile; ?>
<?php endif; ?>
```

**`field:flex:nested` (HTML/PHP)**

Flexible Content nested field returns the below:

```php
<?php if( have_rows('field_name') ): ?>
    <?php while ( have_rows('field_name') ) : the_row(); ?>
        <?php if( get_row_layout() == 'layout_field' ): ?>
            <?php if( have_rows('layout_rows') ): ?>
                <ul>
                    <?php while ( have_rows('layout_rows') ) : the_row(); $image = get_sub_field('sub_field'); ?>
                        <li><img src="<?php echo $image['url']; ?>" alt="<?php echo $image['alt']; ?>" /></li>
                    <?php endwhile; ?>
                </ul>
            <?php endif; ?>
        <?php endif; ?>
    <?php endwhile; ?>
<?php endif; ?>
```

### Relationship Field

**`field:relationship` (HTML/PHP)**

Get a relationship field and loop over all returned posts.

```php
<?php $posts = get_field('field_name'); ?>
<?php if ( $posts ): ?>
  <ul>
    <?php foreach ( $posts as $post ) : setup_postdata( $post ); ?>
      <li>
        <a href="<?php the_permalink(); ?>"><?php the_title(); ?></a>
      </li>
    <?php endforeach; wp_reset_postdata(); ?>
  </ul>
<?php endif; ?>
```

### Gravity Form Field

**`field:form` (HTML/PHP)**

Display a gravity form. The parameters for `gravity_form()` are outlined in the [Gravity Forms documentation](http://www.gravityhelp.com/documentation/page/Embedding_A_Form#Function_Call).

```php
<?php if ( get_field('field_name') ) {
  $form = get_field('field_name');
  gravity_form_enqueue_scripts($form->id, true);
  gravity_form($form->id, display_title, display_description, false, field_values, enable_ajax, 1);
} ?>
```

### Repeater Field

**`field:repeater` (HTML/PHP)**

Get and loop over a repeater field

```php
<?php if ( have_rows('field_name') ) : ?>

  <?php while( have_rows('field_name') ) : the_row(); ?>

    <?php the_sub_field('sub_field_name'); ?>

  <?php endfor; ?>

<?php endif; ?>
```

**`field:repeater:grid` (HTML/PHP)**

Loop over a repeater filed and seperate results into rows. The second tabstop is the row length.

```php
<?php if ( get_field('field_name') ) : ?>

  <div class="items">
    <?php foreach ( array_chunk(get_field('field_name'), 2) as $row): ?>

      <div class="row">
        <?php foreach ($row as $item): ?>

          <div class="item">
            <?php echo $item['field_name']; ?>$5
          </div>

        <?php endforeach; ?>
      </div>

    <?php endforeach; ?>
  </div>

<?php endif; ?>
```

**`field:repeater:slider` (HTML/PHP)**

Loop over a repeater slider.

```php
<?php if( have_rows('slides') ): ?>
<div class="slides">
  <?php while( have_rows('slides') ): the_row(); 
        $image = get_sub_field('image');
        $size = "full"; // (thumbnail, medium, large, full or custom size)
        ?>
  <div class="slide">
    <?php echo wp_get_attachment_image( $image, $size ); ?>
  </div>
  <?php endwhile; ?>
</div>
<?php endif; ?>
```


**`field:repeater:foreach` (HTML/PHP)**

ACF repeater field with foreach loop

```php
<?php $rows = get_field('repeater_field_name');
if( $rows ) {
    echo '<div class="slides">';
    foreach( $rows as $row ) {
        $image = $row['image_field'];
        echo '<div class="slide">';
            echo wp_get_attachment_image( $image, 'full' );
            echo wp_kses_post( wpautop( $row['caption_field'] ) );
        echo '</div>';
    }
    echo '</div>';
} ?>
```

**`field:repeater:nested` (HTML/PHP)**

ACF nested repeater field

```php
<?php
/**
 * Field Structure:
 *
 * - parent_repeater (Repeater)
 *   - parent_title (Text)
 *   - child_repeater (Repeater)
 *     - child_title (Text)
 */
if( have_rows('parent_repeater') ):
    while( have_rows('parent_repeater') ) : the_row();

        // Get parent value.
        $parent_title = get_sub_field('parent_title');

        // Loop over sub repeater rows.
        if( have_rows('child_repeater') ):
            while( have_rows('child_repeater') ) : the_row();

                // Get sub value.
                $child_title = get_sub_field('child_title');

            endwhile;
        endif;
    endwhile;
endif;
?>
```

**`field:repeater:first` (HTML/PHP)**

ACF repeater field - get first row

```php
<?php
$rows = get_field('repeater_field_name');
if( $rows ) {
    $first_row = $rows[0];
    $first_row_title = $first_row['title'];
    // Do something...
}  ?>
```

### Gallery

**`field:gallery` (HTML/PHP)**

ACF repeater field - get first row

```php
<?php 
$images = get_field('gallery');
$size = 'full'; // (thumbnail, medium, large, full or custom size)
if( $images ): ?>
    <div class="slides">
        <?php foreach( $images as $image_id ): ?>
            <div class="slide">
                <?php echo wp_get_attachment_image( $image_id, $size ); ?>
            </div>
        <?php endforeach; ?>
    </div>
<?php endif; ?>
```

### Queries

**`field:query` (HTML/PHP)**

Query a post type on a field value and loop over posts

```php
<?php

$args = array(
  'numberposts' => 10,
  'post_type' => 'post',
  'meta_key' => 'field_name',
  'meta_value' => 'field_value'
);

$query = new WP_Query( $args );

?>

<?php if( $query->have_posts() ) : ?>
  <ul>
  <?php while ( $query->have_posts() ) : $query->the_post(); ?>
    <li>
      <a href="<?php the_permalink(); ?>"><?php the_title(); ?></a>
    </li>
  <?php endwhile; ?>
  </ul>
<?php endif; ?>

<?php wp_reset_query(); ?>
```

### Misc

`ddfield` **(HTML/PHP)**

`var_dump` the field contents wrapped in `<pre>` tags.

```php
<pre>
    <?php var_dump(get_field('field_name')); die(); ?>
</pre>
```

## Contributors

<!-- Contributors START
Anthony_Hubble hubsta http://github.com/hubsta doc
Michael_Mano michaelmano https://github.com/michaelmano doc tools
Neil Ne-Ne https://github.com/Ne-Ne bugfix
Vadym_Mishchenko miffa1991 https://github.com/miffa1991 doc
Contributors END -->

| [![c-1-image]][c-1-link] | [![c-2-image]][c-2-link] | [![c-3-image]][c-3-link] | [![c-4-image]][c-4-link] |
| :----------------------: | :----------------------: | :----------------------: | :----------------------: |
|      Anthony Hubble      |       Michael Mano       |       Neil (Ne Ne)       |     Vadym Mishchenko     |

[c-1-image]: https://avatars.githubusercontent.com/hubsta?s=100
[c-1-link]: http://github.com/hubsta
[c-2-image]: https://avatars.githubusercontent.com/michaelmano?s=100
[c-2-link]: http://github.com/michaelmano
[c-3-image]: https://avatars.githubusercontent.com/Ne-Ne?s=100
[c-3-link]: http://github.com/Ne-Ne
[c-4-image]: https://avatars.githubusercontent.com/miffa1991?s=100
[c-4-link]: https://github.com/miffa1991

