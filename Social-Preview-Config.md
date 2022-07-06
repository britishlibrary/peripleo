# Peripleo Social Preview Configuration

__Social previews__ are info boxes that appear when you 
share a link on social media platforms like as Facebook, Twitter, LinkedIn, etc.

![Peripleo social preview example](/social-preview-example.png)

You can control Peripleo's social preview through a set of `<meta>` tags in your `index.html`.

## Twitter Metadata

Twitter previews rely on the proprietary [TWitter Cards](https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/abouts-cards)
format. The key properties to fill are `twitter:card`, `twitter:site`, `twitter:title`, `twitter:description` and `twitter:image`. Simply
adjust the presets in the `index.html` template to your needs.

Make sure that `twitter:image` points to the __absolute URL__ of your preview image.

```html
<!-- Twitter card metadata -->
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:site" content="@britishlibrary" />
<meta name="twitter:title" content="Peripleo | Locating a National Collection" />
<meta name="twitter:description" content="A prototype viewer for geo-located collection data." />
<meta name="twitter:image" content="https://github.com/britishlibrary/peripleo-lanc/raw/main/public/peripleo-social-preview.png" />
```

## OpenGraph Metadata

Most other social platforms (Facebook, LinkedIn, etc.) support the [Open Graph](https://ogp.me/) vocabulary. The key 
properties to fill are `og:type`, `og:title`, `og:description`, `og:image`, `og:image:width`, `og:image:height` and `og:url`. 
Make sure that the 

Make sure that `og:image` points to the __absolute URL__ of your preview image; and that `og:url` points to the 
__absolute base URL__ of your Peripleo instance.

```html

```html
<!-- OG metadata -->
<meta property="og:type" content="website" />
<meta property="og:title" content="Peripleo | Locating a National Collection" />
<meta property="og:description" content="A prototype viewer for geo-located collection data." />
<meta property="og:image" content="https://github.com/britishlibrary/peripleo-lanc/raw/main/public/peripleo-social-preview.png" />
<meta property="og:image:width" content="1000" />
<meta property="og:image:height" content="500" />
<meta property="og:url" content="https://britishlibrary.github.io/peripleo-lanc/" />
```

## Preview Image

This template links to a [generic Peripleo preview image](https://raw.githubusercontent.com/britishlibrary/peripleo-lanc/main/public/peripleo-social-preview.png).
Replace this with a custom image of your own choice. Make sure the image dimensions are at least 1000x500 pixels.