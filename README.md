
<!-- README.md is generated from README.Rmd. Please edit that file -->

# thematic

<!-- badges: start -->

[![R build
status](https://github.com/rstudio/thematic/workflows/R-CMD-check/badge.svg)](https://github.com/rstudio/thematic)
[![CRAN
status](https://www.r-pkg.org/badges/version/thematic)](https://CRAN.R-project.org/package=thematic)
[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
<!-- badges: end -->

Unified and automatic theming of **ggplot2**, **lattice**, and **base**
R graphics.

## Installation

**thematic** is not yet available on [CRAN](https://CRAN.R-project.org),
but you can install it now with:

``` r
remotes::install_github("rstudio/thematic")
library(thematic)
```

In addition, for the auto-theming integration with **shiny**, you
currently need:

``` r
remotes::install_github("rstudio/shiny")
```

And, for auto-theming in `rmarkdown::html_document()`:

``` r
remotes::install_github("rstudio/rmarkdown#1706")
```

If you’d prefer to not install these experimental versions, and just
want to play around with some **thematic** examples, take a test drive
on **RStudio Cloud**:

<div>

<a href="https://rstudio.cloud/project/1208127" target="_blank">
<img src="man/figures/thematic-test-drive.svg" alt="RStudio Cloud Example" height="80px" style="display: block; margin: 0 auto;">
</a>

</div>

## Getting started

**thematic** sets various graphing defaults based on a few different
color and font settings. The main settings are background, foreground,
`accent`, and `font`. These particular setting here intentionally match
the styling of **thematic**’s
[website](https://rstudio.github.io/thematic/).

``` r
library(thematic)
thematic_on(
  bg = "#222222", fg = "white", accent = "#0CE3AC", 
  font = font_spec("Oxanium", scale = 1.25)
)
```

Now, any future plot(s) inherit these settings (at least until
`thematic_off()` is called):

``` r
library(ggplot2)
ggplot(mtcars, aes(wt, mpg)) +
  geom_point() +
  geom_smooth()
```

<img src="man/figures/README-unnamed-chunk-3-1.png" width="70%" style="display: block; margin: auto;" />

In addition, **thematic** sets new defaults for qualitative color-scales
(the default is based color-blind safe `okabe_ito()` color palette, but
this may be customized via the `qualitative` argument to
`thematic_on()`):

``` r
ggplot(mtcars, aes(wt, mpg)) +
  geom_point(aes(color = factor(cyl))) +
  geom_smooth(color = "white") 
```

<img src="man/figures/README-unnamed-chunk-4-1.png" width="70%" style="display: block; margin: auto;" />

It also sets new defaults for `sequential` color scales based on the
`accent`, `bg`, and `fg` (this default is also configurable via the
`sequential` argument):

``` r
ggplot(mtcars, aes(wt, mpg)) +
  geom_point(aes(color = cyl)) +
  geom_smooth(color = "white") 
```

<img src="man/figures/README-unnamed-chunk-5-1.png" width="70%" style="display: block; margin: auto;" />

## Auto theming

**thematic** also has the ability to automatically detect the relevant
colors and fonts in some scenarios, based on the current plotting
context. The `bg`, `fg`, and `accent` arguments all default to `'auto'`
because they generally work where available, but you currently must
opt-in to font detection:

``` r
thematic_on(font = "auto")
```

Auto theming works best with a [**shiny**
runtime](https://rstudio.github.io/thematic/articles/shiny.html), but it
can also work under certain conditions inside
[**rmarkdown**](https://rstudio.github.io/thematic/articles/rmarkdown.html)
and [RStudio](https://rstudio.github.io/thematic/articles/rstudio.html).
For a quick demonstration, here’s a **shiny** app styled with a dark
background, light foreground, and a Google Font (e.g. Pacifico),
**thematic** can automatically mimic those styles in the R plots.

<img src="https://i.imgur.com/eWXYtis.gif" width="70%" style="display: block; margin: auto;" />

## Font support

If a Google Font (e.g.,
[Oxanium](https://fonts.google.com/specimen/Oxanium),
[Pacifico](https://fonts.google.com/specimen/Pacifico), etc) which isn’t
already known to R is requested, **thematic** attempts to download,
cache, and register that font for use with **showtext** and **ragg**
before plotting. That means, if you have **showtext** installed, Google
Fonts work ‘out-of-the-box’ in **shiny** and **rmarkdown**. Moreover,
**shiny**, **rmarkdown**, and RStudio can all be configured to use
**ragg** as well, which can also render custom fonts. For more details,
see the Google Font rendering sections of the **shiny**, **rmarkdown**,
and RStudio articles.

Of course, you can also specify fonts that are already known to R
without necessarily have **showtext** or **ragg**. If you’d like to use
a font that isn’t already known to R (and isn’t a Google Font), point
`sysfonts::font_add()` (for **showtext**) or
`systemfonts::register_font()` (for **ragg**) to the font files.

## Learn more

To learn more about how to set up and use **thematic** in various
environments see these articles:

  - [Shiny](https://rstudio.github.io/thematic/articles/shiny.html)
  - [R
    Markdown](https://rstudio.github.io/thematic/articles/rmarkdown.html)
  - [RStudio](https://rstudio.github.io/thematic/articles/rstudio.html)

Also see the
[customize](https://rstudio.github.io/thematic/articles/customize.html)
article to learn more about how to control thematic’s defaults.

<!--

With **thematic** now enabled, static R plots generated via a **shiny** app automatically adopt the styling of their HTML container(s). For example, the **shiny** app below uses **shinythemes** and the [Google Fonts API](https://developers.google.com/fonts/docs/getting_started) for styling the HTML; and thanks to **thematic**, those styles are automatically reflected in the R plots. Note that, in the case that the Pacifico font isn't available to R, **thematic** will detect a lack of support, download font files via the Google Font API (unless told otherwise), cache them (see `font_cache_set()`), and register them for use with both **showtext** and **ragg** (one of these two packages must be installed to actually render the auto-installed fonts).

...

By the way, **thematic** works regardless of how the **shiny** app is actually styled (in fact, [**bootstraplib**](https://rstudio.github.io/bootstraplib/articles/recipes.html), not **shinythemes**, will soon be our recommended way to style **shiny** apps). The only requirement is that, for font rendering, you must either use a [Google Font](https://fonts.google.com/) (and have **showtext** or **ragg** installed) or make sure R already has the ability to render the font (which can be done manually via **extrafont**, **showtext**, or **ragg**).

## Auto vs specified themes

Auto theming is guaranteed to work inside of a **shiny** runtime, but in other situations<sup>1</sup>, you may want to specify the colors and/or fonts prior to plot generation.


```r
thematic_on(
  bg = "#FFFFF8", fg = "#111111", accent = "#DD1144",
  font = font_spec("Tangerine", scale = 2)
)
```


```r
library(ggplot2)
ggplot(diamonds[sample(nrow(diamonds), 1000), ], aes(carat, price)) +
  geom_point(alpha = 0.2) +
  geom_smooth() +
  facet_wrap(~cut) + ggtitle("Diamond price by carat and cut with tufte styling")
```

<img src="man/figures/README-unnamed-chunk-8-1.png" width="70%" style="display: block; margin: auto;" />

## Rendering of custom fonts

Rendering of 'custom' Google Fonts (i.e., Google Fonts not generally available to R) requires either the **showtext** or the **ragg** package to be installed. For Google Font rendering to work 'out-of-the-box' with both **shiny** and **rmarkdown**, make sure **showtext** is installed. If you want a plot with custom fonts outside of **shiny** and **rmarkdown**, consider using `thematic_save_plot()` as RStudio's graphics device currently doesn't support custom fonts at all (if you'd like to preview the file that `thematic_save_plot()` generates in RStudio, you can use `file.show()`).

If you want custom font(s) that aren't hosted by Google Fonts, you'll currently need to download and register them with R yourself. After downloading the font files, you can use `sysfonts::font_add()` (for **showtext**) and/or `systemfonts::register_font()` (for **ragg**) to register them with R. Another, more expensive, but more permanent (i.e., you only have to do it once, instead of everytime you start a new R session) solution to make custom fonts available to R is via `extrafont::import_font()` and `extrafont::loadfonts()`.

If you have **showtext** installed and custom Google Font rendering still fails in **rmarkdown**, you can likely fix it by doing one of the following in a "setup" chunk (i.e., a **knitr** code chunk that appears at the top of the document which sets up [chunk options](https://yihui.org/knitr/options/) defaults and other global state):
  1. Include `knitr::opts_chunk$set("fig.showtext" = TRUE)`, which enables **showtext** in all *future* code chunks.
  2. Load **thematic** (e.g., `library(thematic)`), which will call `knitr::opts_chunk$set("fig.showtext" = TRUE)` for you.

## Learn more

To learn more about **thematic**, see the [demos article](https://rstudio.github.io/thematic/articles/demos.html).

<hr/>

1. Auto theming works in **rmarkdown** with `runtime: shiny` and also in `html_document_base()` based output formats with a non-`NULL` `theme` (i.e., powered by **bootstraplib**). With time, hopefully most other **rmarkdown** output formats will use `auto_defaults()` (internally) so that `'auto'` values can work the way you'd expect them to (at least by default). Note also that `accent='auto'` and `font='auto'` currently doesn't work with custom RStudio themes.

-->
