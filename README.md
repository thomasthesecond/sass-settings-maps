# Sass Settings Maps

Use Sass maps with functions and dot notation. For example, this method allows
you to write something like this:

```scss
width: setting('component.header.width');
```

This method relies on the following functions:

- `str-explode` by [Hugo Giraudel](https://github.com/HugoGiraudel/SassyStrings)
- `map-deep-get` by [at-import](https://github.com/at-import/sassy-maps)

## Usage

The settings map allows you to conveniently define all of your variables in a
single map.

You should have a map like so:

```scss
$settings: (
  border: 1px solid black,
  zIndex: (
    nav: 2,
    default: 1
  )
);
```

and then you would simply use the `setting` function to call your defined
settings:

```scss
.foo {
  border-top: setting('border');
  z-index: setting('zIndex.default');
}
```

which will output:

```css
.foo {
  border-top: 1px solid black;
  z-index: 1;
}
```

You can also pass a second optional paramter to the `setting` function to
return a setting from a different map. This is ideal for component-specific
settings. By default, the `setting` function looks for a map named
`$settings`, but say you have partial named `_masthead.scss`, you might do
something like so:

```scss
// _settings.scss

$settings: (
  maxWidth: 1200px
);

// _masthead.scss

$masthead: (
  breakpoint: em(800px),
  height: (
    min: 200px,
    max: 90vh
  )
);

.masthead {
  height: setting('height.min', $masthead); // component-specific setting
  max-width: setting('maxWidth'); // 'global' setting

  @media (min-width: setting('breakpoint', $masthead)) {
    height: setting('height.max', $masthead);
  }
}
```

and when compiled will read:

```css
.masthead {
  height: 200px;
  max-width: 1200px;

  @media (min-width: 50em) {
    height: 90vh;
  }
}
```
