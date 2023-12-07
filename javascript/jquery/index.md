# jQuery

## Using jQuery

Your jQuery script runs on the user's browser after your WordPress webpage is received. A basic jQuery statement has two parts: a selector that determines which HTML elements the code applies to, and an action or event, which determines what the code does or what it reacts to. The basic event statement looks like this:

```
jQuery.(selector).event(function);
```

When an event, such as a mouse click, occurs in an HTML element selected by the selector, the function that is defined inside the final set of parentheses is executed.

All the following code examples are based on this HTML page content. Assume it appears on your plugin's admin settings screen, defined by the file `myplugin_settings.php`. It is a simple table with radio buttons next to each title.

```
<form id="radioform">
  <table>
    <tbody>
    <tr>
      <td><input class="pref" checked="checked" name="book" type="radio" value="Sycamore Row" />Sycamore Row</td>
      <td>John Grisham</td>
    </tr>
    <tr>
      <td><input class="pref" name="book" type="radio" value="Dark Witch" />Dark Witch</td>
      <td>Nora Roberts</td>
    </tr>
    </tbody>
  </table>
</form>
```

The output could look something like this on your settings page.

![Sample Table](https://i3.wp.com/make.wordpress.org/docs/files/2013/11/pdh-ajax-example.png)

In the [article on AJAX](https://developer.wordpress.org/plugins/javascript/ajax/), we will build an AJAX exchange that saves the user selection in usermeta and adds the number of posts tagged with the selected title. Not a very practical application, but it illustrates all the important steps. jQuery code can either reside in an external file or be output to the page inside a `<script>` block. We will focus on the external file variation because passing values from PHP requires special attention. The same code can be output to the page if that seems more expedient to you.

### Selector and Event

The selector is the same form as CSS selectors: `.class` or `#id`. There's many [more forms](https://api.jquery.com/category/selectors/ "jQuery Reference"), but these are the two you will frequently use. In our example, we will use class `.pref`. There's also a slew of possible [events](https://api.jquery.com/category/events/ "jQuery Reference"), one you will likely use a lot is _‘click'_. In our example we will use _‘change'_ to capture a radio button selection. Be aware that jQuery events are often named somewhat differently than those with JavaScript. So far, after we add in an empty anonymous function, our example statement looks like this:

```
$.(".pref").change(function(){
  /*do stuff*/
});
```

This code will "do stuff" when any element of the "pref" class changes.

[info]This code snippet, and all examples on this page, are for illustrating the use of AJAX. The code is not suitable for production environments because related operations such as [sanitization](https://developer.wordpress.org/plugins/security/securing-input/), [security](https://developer.wordpress.org/apis/security/nonces/), [error handling](https://www.sitepoint.com/error-handling-in-php/), and [internationalization](https://developer.wordpress.org/plugins/internationalization/) have been intentionally omitted. Be sure to always address these important operations in your production code.[/info]