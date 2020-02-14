# Contents

1.  HTML

    1.  Use tables

2.  CSS

    1.  Inline styles

    2.  Linking CSS files

    3.  CSS inside head tag

    4.  Client-specific CSS styles

    5.  Reset CSS styles

    6.  Conditional CSS for Microsoft Outlook

3.  Responsive Emails

    1.  Media features

    2.  Using important flag

    3.  Text size

    4.  Responsive Column Layouts

    5.  No floating and positioning

    6.  Making captioned images responsive

    7.  Fluid images

    8.  Buttons

    9.  Usable link groups

4.  Reference

    1.  Email template design checklist
    
    2.  HTML table element

Following are a few set of rules related to responsiveness and CSS which can help build an email template without much struggle:

## HTML

### Use tables

When it comes to email HTML, note that all best practices from web development goes out the window. To make the look consistent, we should use `<table>` elements instead of `<div>` elements.

Some of the table attributes used extensively during HTML email template design are as follows:

1.  `align`: This indicates how the table must be aligned inside the containing document. It can have three values: `left`, `right` or `center`.

2.  `border`: This represents the size of the frame surrounding the table. The value is an integer.

3.  `cellpadding`: This defines the space between the content of a cell and its border, displayed or not. This can be a percentage value or pixels.

4.  `cellspacing`: This defines the size of the space between two cells in a percentage value or pixels.

5.  `width`: This defines the width of the table in percentage value or pixels.

## CSS

### Inline styles

CSS must be inlined within your HTML document.

### Linking CSS files

The major webmail clients don't support `<link>` element.

### CSS inside head tag

#### Don't leave CSS styles in the email's `<head>`

**Gmail** strips the entire element from the HTML when an email is viewed either in its web or mobile clients, leaving an unstyled mess.

### Client-specific CSS styles

1.  When an email is pulled into [Outlook.com](http://Outlook.com) / Hotmail, any style rules present in the email are appended with `.ExternalClass`.

**SOLUTION:** Normalization as shown below:
```
.ExternalClass{
    width: 100%;
}

.ExternalClass,
.ExternalClass p,
.ExternalClass span,
.ExternalClass font,
.ExternalClass td,
.ExternalClass div{
    line-height: 100%;
}
```

2.  [Outlook.com](http://Outlook.com) / Hotmail sets its own color on heading elements lower in level than an `<h1>` element.

**SOLUTION:** We need to override them.

3.  Left and right table element spacing issue on Outlook.

**SOLUTION:**

```
table{
    mso-table-lspace: 0pt;
    mso-table-rspace: 0pt;
}
```

4.  Issue with resizing fluid images on **IE** browsers:

**SOLUTION:**

```
img{
    -ms-interpolation-mode: bicubic;
}
```

5.  `WebKit` looks for any text that happens to be sized smaller than 13px and increases it to that number, which can sometimes cause design issues in places intended for small text. In order to prevent this on **OSX / iOS**, we can do the following:

```
body{
    -webkit-text-size-adjust: 100%;
}
```

6.  When iOS detects a phone number, address, or calendar date, it turns those items as links with the default color of `#0000FF`.

To avoid turning a phone number into a link, do the following:

<table class="wysiwyg-macro" data-macro-name="code" data-macro-id="b629f410-61b3-4c3e-941d-a6d79ac90f8c" data-macro-schema-version="1" style="background-image: url(https://springboard-edu.atlassian.net/wiki/plugins/servlet/confluence/placeholder/macro-heading?definition=e2NvZGV9&amp;locale=en_GB&amp;version=2); background-repeat: no-repeat;" data-macro-body-type="PLAIN_TEXT">

```
<meta name="format-detection" content="telephone=no">
```

**NOTE:** We should, however, provide our own way of making a call from an email as follows:

```
<a href="tel:1-800-555-5555">1-808-555-5555</a>
```

To avoid turning dates and addresses into links, do as shown in the following example:

```
Visit EmailCo. at <a href="#" style="color:#000000; text-decoration:none;">123 Atlantic Ave. &bull; Atlanta, GA 30318 USA</a>
```

### Reset CSS styles

Reset CSS styles are helpful in normalizing how the code gets rendered across multiple email clients. Below are a few commonly used ones:

1.  `body` element

```
body {
    margin: 0;
    padding: 0;
}
```

2.  `img` element

```
img {
    border: 0 none;
    height: auto;
    line-height: 100%;
    outline: none;
    text-decoration: none;
}

a img {
    border: 0 none;
}
```

3.  `table` and `td` elements

To get rid of extra spacing:

```
table, td {
    border-collapse: collapse;
}
```

When using the `border-radius` property on any element, use the following to get rid of extra spaces:

```
table, td {
    border-collapse: separate;
}
```

4.  Reset for the top-most parent table

Since **GMail** strips the `<body>` element, in order to make the top-most parent table act like the body, we can apply the following CSS properties to it:

<table class="wysiwyg-macro" data-macro-name="code" data-macro-id="574ce2f5-6b84-4541-9154-22db4e22bca9" data-macro-schema-version="1" style="background-image: url(https://springboard-edu.atlassian.net/wiki/plugins/servlet/confluence/placeholder/macro-heading?definition=e2NvZGV9&amp;locale=en_GB&amp;version=2); background-repeat: no-repeat;" data-macro-body-type="PLAIN_TEXT">

```
height:100% !important;
margin:0;
padding:0;
width:100% !important;
```

### Conditional CSS for Microsoft Outlook

Outlook doesn't support CSS very well and to make sure the email template looks particularly well on Outlook, we need to follow a different syntax as shown below:

```
<!--[if gte mso 9]>
    <style type="text/css">
    /* Your Outlook-specific CSS goes here. */
    </style>
<![endif]-->
```

**NOTE:** Any Outlook specific CSS should be at the end of the CSS styles.

In the above code snippet, we are targeting Outlook versions greater than or equal to 9\. `mso` stands for Microsoft Outlook, `gte` stands for greater than or equal to. Similarly, `lte` stands for less than or equal to, `lt` stands for less than and `gt` stands for greater than.

## Responsive Emails

### Media features

For responsiveness, we should only be concerned with using two specific media features:

1.  `max-width` - measured against the available space of the browser

2.  `max-device-width` - measured against the size of the device screen

Example:

```
@media only screen and (max-width:480px) {
  //...
}
```

In the above example,

`only` restricts the display of the media query styles to the specified media type.

`screen` is the **media** type. There can be two **media** types - `screen` and `print`.

### Using important flag

In HTML email templates, it's necessary to inline CSS styles. So, for the media queries to work properly, every media query style rule we write needs to be marked with a `!important` declaration because _inline styles have the highest specificity in the cascade_.

```
<style type="text/css">
    .container {
        color: #505050;
        font-size: 15px;
    }

    @media only screen and (max-width:480px){
        .container {
            color: #CCCCCC !important;
            font-size: 20px !important;
        }
    }
</style>
```

### Text size

While 14px font size is fine for a desktop, the user will struggle to read anything of that size on a small screen.

The recommended values for `font-size` used when designing email templates to solve this problem are:

| Device | Size |
|--------|------|
| Desktop | 14px |
| Mobile | 16px |

### Responsive Column Layouts

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

**NOTE:** See the use of `display: block` in the above code snippet.

### No floating and positioning

Avoid CSS `float` and `position` properties. These two properties are not supported across some email clients.

### Making captioned images responsive

##### Top caption image

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

##### Bottom caption image

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

##### Right caption image

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

##### Left caption image

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

### Fluid images

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

### Buttons

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

### Usable link groups

A group of several links clustered together can generally be found near the footer of the page. While it is fine to have the links inline on desktop; for small screens, the top-to-bottom approach works best. Here's how to do it:

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

## Reference

### Email template design checklist

[https://templates.mailchimp.com/resources/email-client-css-support/](https://templates.mailchimp.com/resources/email-client-css-support/)

### HTML table element

[https://developer.mozilla.org/en-US/docs/Web/HTML/Element/table](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/table)
