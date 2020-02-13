# HTML-email-template-guidelines

## Responsive Emails

### RULE 1

For responsivenes, we should only be concerned with using two specific media features:

1. `max-width` - measured against the available space of the browser
2. `max-device-width` - measured against the size of the device screen

Example: 
```
@media only screen and (max-width:480px) {
  //...
}
```
In the above example,

`only` restricts the display of the media query styles to the specified media type.

`screen` is the **media** type. There can be two **media** types - `screen` and `print`.

### RULE 2

In HTML email templates, it's necessary to inline CSS styles. So, for the media queries to work properly, every media query style rule we write needs to be marked with a `!important` declaration because *inline styles have the highest specificity in the cascade*.

```
<style type="text/css">
    .container{
        color: #505050;
        font-size: 15px;
    }

    @media only screen and (max-width:480px){
        .container{
            color: #CCCCCC !important;
            font-size: 20px !important;
        }
    }
</style>
```

### RULE 3

#### Text size

While 14px font size is fine for a desktop, the user will struggle to read anything of that size on a small screen.

The recommended values for `font-size` used when designing email templates to solve this problem are:

|Device|Size|
|-|-|
|Desktop|14px|
|Mobile|16px|

### RULE 4

#### Responsive Column Layouts

On small screens, it's best to have linear top-to-bottom layout instead of horizontal layout.

This can be achieved using `@media` queries and targeting devices with `max-width` of `480px` as shown below:

```
<table border="0" cellpadding="0" cellspacing="0" width="600" id="templateColumns">
    <tr>
        <td align="center" valign="top" width="50%" class="templateColumnContainer">
            <!-- Content wrapped in another table -->
        </td>
        <td align="center" valign="top" width="50%" class="templateColumnContainer">
            <!-- Content wrapped in another table -->
        </td>
    </tr>
</table>
```

```
 @media only screen and (max-width: 480px){
    #templateColumns {
        width: 100% !important;
    }

    .templateColumnContainer {
        display: block !important;
        width: 100% !important;
    }
}
```

### RULE 5

### No floating and positioning

Avoid CSS `float` and `position` properties. These two properties are not supported across some email clients.

### RULE 6

### Making captioned images responsive

#### Top caption image

```
<table border="0" cellpadding="0" cellspacing="0" width="100%">
    <tr>
        <td align="center" valign="top">
            <table border="0" cellpadding="0" cellspacing="0" width="100%">
                <tr>
                    <td valign="top">
                        Text content
                    </td>
                </tr>
                <tr>
                    <td valign="top">
                        <img class="imgResponsive" src="..." width="520" />
                    </td>
                </tr>
            </table>
        </td>
    </tr>
</table>
```

#### Bottom caption image

```
<table border="0" cellpadding="0" cellspacing="0" width="100%">
    <tr>
        <td align="center" valign="top">
            <table border="0" cellpadding="0" cellspacing="0" width="100%">
                <tr>
                    <td valign="top">
                        <img class="imgResponsive" src="..." width="520" />
                    </td>
                </tr>
                <tr>
                    <td valign="top">
                        Text content
                    </td>
                </tr>
            </table>
        </td>
    </tr>
</table>
```

In order to make both the above responsive, we can do the following:

```
<style type="text/css">
    @media only screen and (max-width: 480px) {
        .imgResponsive {
            max-width: 520px !important;
            width: 100% !important;
        }
    }
</style>
```

#### Right caption image

```
<table border="0" cellpadding="0" cellspacing="0" width="100%">
    <tr>
        <td valign="top">
            <table align="left" border="0" cellpadding="0" cellspacing="0" width="200" class="imgContainer">
                <tr>
                    <td align="center" valign="top">
                        <img class="imgResponsive" src="..." width="200" />
                    </td>
                </tr>
            </table>
            <table align="right" border="0" cellpadding="0" cellspacing="0" width="300">
                <tr>
                    <td valign="top">
                        Right content
                    </td>
                </tr>
            </table>
        </td>
    </tr>
</table>
```

#### Left caption image

```
<table border="0" cellpadding="0" cellspacing="0" width="100%">
    <tr>
        <td valign="top">
            <table align="right" border="0" cellpadding="0" cellspacing="0" width="200" class="imgContainer">
                <tr>
                    <td align="center" valign="top">
                        <img class="imgResponsive" src="..." width="200" />
                    </td>
                </tr>
            </table>
            <table align="left" border="0" cellpadding="0" cellspacing="0" width="300">
                <tr>
                    <td valign="top">
                        Right content
                    </td>
                </tr>
            </table>
        </td>
    </tr>
</table>
```

**NOTE**: We don't change the layout and only change the `align` attribute. This is to make sure that on smaller screens, the responsive version is rendered (the image at the top followed by the text at the bottom).

In order to make the above two responsive, we can do the following:

```
<style type="text/css">
    @media only screen and (max-width: 480px) {
        .imgResponsive {
            max-width: 520px !important;
            width: 100% !important;
        }

        .imgContainer {
            display: block !important;
            width: 100% !important;
        }
    }
</style>
```

### RULE 7

#### Fluid images

We can make the image adapt to different screen sizes as follows:

```
<style type="text/css">
    @media only screen and (max-width: 480px) {
        .imgResponsive {
            height: auto !important;
            max-width: 600px !important;
            width: 100% !important;
        }
    }
</style>
```

### RULE 8

#### Buttons

For small screens, buttons should be full width and text size should be increased in comparison to the desktop text size for readability.

```
<table border="0" cellpadding="0" cellspacing="0" class="emailButton">
  <tr>
    <td align="center" valign="middle">
      <a href="..." target="_blank">Read more</a>
    </td>
  </tr>
</table>
```

```
@media only screen and (max-width: 480px){
    .emailButton {
      max-width: 600px !important;
      width: 100% !important;
    }

    .emailButton a {
      // ...
      display: block !important;
    }
  }
```

### RULE 9

#### Usable link groups

A group of several links clustered together can generally be found near the footer of the page. While it is fine to have the links inline on desktop, for small screens, the top-to-bottom approach works best. Here's how to do it:

```
<table border="0" cellpadding="0" cellspacing="0" width="100%" id="emailFooter">
    <tr>
        <td align="center" valign="top">
            <table border="0" cellpadding="5" cellspacing="0" id="link-container">
                <tr>
                    <td valign="top" class="link">
                        <a href="..." target="_blank">Link 1</a>
                    </td>
                    <td valign="top" class="link">
                        <a href="..." target="_blank">Link 2</a>
                    </td>
                    <td valign="top" class="link">
                        <a href="..." target="_blank">Link 3</a>
                    </td>
                    <td valign="top" class="link">
                        <a href="..." target="_blank">Link 4</a>
                    </td>
                </tr>
            </table>
        </td>
    </tr>
</table>
```

```
<style type="text/css">
    @media only screen and (max-width: 480px){
        #link-container {
            max-width:600px !important;
            width:100% !important;
        }

        .link {
            // ...
            display:block !important;
            width:100% !important;
        }

        .link a{
            // ...
            display:block !important;
        }
    }
</style>
```

## CSS

### RULE 1

CSS must be inlined within your HTML document.

### RULE 2

#### Links to CSS files are not supported

The major webmail clients don't support `<link>` element.

### RULE 3

#### Don't leave CSS styles in the email's `<head>`

**Gmail** strips the entire element from the HTML when an email is viewed either in its web or mobile clients, leaving an unstyled mess.
