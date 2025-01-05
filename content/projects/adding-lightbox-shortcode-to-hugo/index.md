+++
date = '2025-01-05T00:55:51Z'
draft = false
title = 'Adding Lightbox Shortcode to Hugo'
+++


# Adding lightbox to my Hugo Theme

I use a lot of pictures in my notes So I wanted a more effencient way to add them to my project pages. Adfter reading a bit about Hugo, I found that using a shortcode might be the best way to do this.


## Here is how I did it.

### Copy the Files

1. Download the files from [lightbox2](https://lokeshdhakar.com/projects/lightbox2/)
2. Unzip and copy the js/lightbox-plus-jquery.min.js to assets/js
3. Copy css/lightbox.min.css to assets/css
4. copy the images/* files to assets/images/

### Adding the javascript and CSS

1. Step one was top add the CSS and Javascripts to my website. To do this, we needed to edit the theme. In my case I added the css to edit the themes/congo/layouts/partials/head.html
```
{{ $lightboxcss := resources.Get "css/lightbox.min.css" }}
<link href="{{ $lightboxcss.RelPermalink }}" rel="stylesheet" />
```
2. Then I added the javascript to the theme/congo/layouts/partials/footer.html
```
    {{ $lightboxjs := resources.Get "js/lightbox-plus-jquery.min.js" }}
    <script src="{{ $lightboxjs.RelPermalink }}"></script>
```


### Adding a new shortcode

1. Edit (add is needed) the layouts/shortcodes/lightbox.html file and add the following content:
```
{{- $group := .Get "group" -}}
{{- $lines := split .Inner "\n" -}}
<h3>{{ $group }}</h3>
<hr />
{{ range $lines }}
  {{- $line := strings.TrimSpace . -}}
  {{ if ne $line "" }}
    {{- $parts := split $line "|" -}}
    {{- $href := index $parts 0 }}
    {{- $href := strings.TrimSpace $href }}
    {{- $title := index $parts 1 }}
    {{- $title := strings.TrimSpace $title }}
    <a href="{{ $href }}" data-lightbox="{{ $group }}" data-title="{{ $title }}"><img src="{{ $href }}" width="100px"></a>
  {{ end }}
{{ end }}
<hr />
```

1. **`{{- $group := .Get "group" -}}`**  
   This retrieves the "group" parameter passed into the shortcode and stores it in the variable `$group`.

2. **`{{- $lines := split .Inner "\n" -}}`**  
   This takes the inner content of the shortcode (everything between the shortcode's opening and closing tags) and splits it into individual lines based on newlines (`\n`). The result is stored in the `$lines` variable.

3. **`<h3>{{ $group }}</h3>`**  
   This outputs the `$group` variable inside an `<h3>` tag, which will be rendered as a heading on the page.

4. **`<hr />`**  
   This adds a horizontal line below the group heading.

5. **`{{ range $lines }}`**  
   This iterates over each line in the `$lines` variable.

6. **`{{- $line := strings.TrimSpace . -}}`**  
   This trims any extra spaces from the beginning and end of each line.

7. **`{{ if ne $line "" }}`**  
   This checks if the line is not empty. If the line is not empty, the following block is executed.

8. **`{{- $parts := split $line "|" -}}`**  
   This splits each non-empty line into two parts by the `|` character. The result is stored in the `$parts` variable, which is a list with two elements.

9. **`{{- $href := index $parts 0 }}`**  
   This retrieves the first element of `$parts` (the part before the `|`), which is treated as a URL or image source (`$href`).

10. **`{{- $href := strings.TrimSpace $href }}`**  
    This trims any extra spaces from the `$href` variable.

11. **`{{- $title := index $parts 1 }}`**  
    This retrieves the second element of `$parts` (the part after the `|`), which is treated as the image title (`$title`).

12. **`{{- $title := strings.TrimSpace $title }}`**  
    This trims any extra spaces from the `$title` variable.

13. **`<a href="{{ $href }}" data-lightbox="{{ $group }}" data-title="{{ $title }}"><img src="{{ $href }}" width="100px"></a>`**  
    This generates an anchor (`<a>`) tag with the `href` pointing to the image URL. The `data-lightbox` attribute groups the images by `$group`, and the `data-title` attribute sets the title of the image. Inside the anchor tag, it places an `<img>` tag that points to the image source (`$href`) and sets the width to `100px`.

14. **`{{ end }}`**  
    This closes the `if` condition for checking if the line is not empty.

15. **`{{ end }}`**  
    This closes the `range` loop that iterates over the lines.

16. **`<hr />`**  
    This adds another horizontal line after the entire group of images.

### Using the new shortcode

1. Here is an example of using it on a page:
```
{< lightbox group="Reference Pictures" >}
    images/powersupply.jpg | Power Supply 
    images/Pinout.jpg | Pinout 
    images/s2_mini_v1.0.0_1_16x16.jpg | Esp32 S2-mini Front
    images/s2_mini_v1.0.0_2_16x16.jpg | Esp32 S2-mini Back
{< /lightbox  >}
```
{{< alert >}}
* Double up on those braces, I cound not get it to display without rendering to show a proper example.
{{< /alert >}}
