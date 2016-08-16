---
layout: post
title:  "Inlining svg from file in ASP .NET"
date:   2016-05-28
---

**Rendering your svg's by inlining them can be very practical. But how to do it if all you have is a bunch of svg files, and you don't have time or the knowledge to manually extract the inline code for each of them? Here is a solution I used in an ASP .NET MVC project recently that turned out pretty well.**

SVG's are a great for displaying vector graphics, like icons, that look crisp across all devices. These days we can also enjoy [great browser support](http://caniuse.com/#feat=svg). But using only svg-files loaded through an `<img />` can be a bit limitting. You can't for example change the color of the icon. To achieve this we need to inline the svg-code. Another perk gained from inlining the svg is that you save yourself from the extra network request, with the downside of getting a rather bloated html. But if you have a system for loading the inline svg markup, at least the developers wont have to deal with the bloat.

The solution can be very simple. Here is an example I've used in ASP .NET MVC: We create a HtmlHelper extension method that simply gets the contents of the file requested, like so:

```cs
public static IHtmlString InlineSvg(this HtmlHelper helper, string fileName)
{
    if (string.IsNullOrEmpty(fileName))
    {
        throw new ArgumentException("Path can't be empty", nameof(fileName));
    }
    if (!path.EndsWith("svg"))
    {
        throw new ArgumentException("Path must be to an svg file", nameof(path));
    }

    var fullPath = "~/Content/img/icons/" + filePath;

    return helper.Raw(File.ReadAllText(HostingEnvironment.MapPath(fullPath)));
}
```

Then we can use it like this in our views:

```cs
@Html.InlineSvg("arrow-right.svg")
```

Here you can of course leave it to the caller to give the full path to the extension method, or even leave out the .svg extension from the file name parameter and add it in the extension method. It all boils down to personal preference.

### Supporting different colors
As previously stated, the big upside with inlining svg is that we can modify its fill colors from css. Here's an idea on how to do that, with a simple change to our InlineSvg method:


```cs
public static IHtmlString InlineSvg(this HtmlHelper helper, string path, IconColor color = IconColor.White)
{
    if (string.IsNullOrEmpty(path))
    {
        throw new ArgumentException("Path can't be empty", nameof(path));
    }
    if (!path.EndsWith("svg"))
    {
        throw new ArgumentException("Path must be to an svg file", nameof(path));
    }

    var svg = helper.Raw(File.ReadAllText(HostingEnvironment.MapPath(path)));
    var colorString = color.ToString().ToLowerInvariant();
    return helper.Raw($"<span class='icon-color--{colorString}'>{svg}</span>");
}
```

Here we have added an optional parameter with an enum containing the colors we wish to be available. The color passed in is used to set a css class on a wrapping span element. We can then create these in css classes, and simply set the fill property of the path element to the color we like:


```scss
@mixin icon-color($name, $color) {
.icon-color--#{$name} {
    path {
      fill: $color;
    }
  }
}

@include icon-color(blue, #00f);
@include icon-color(red, #f00);
@include icon-color(white, #fff);
````

We can now drop a bunch of svg icons in a folder, load them in with the HtmlHelper and set them to the color of our choice. Pretty handy.

**But isn't it slow reading the file contents each time the InlineSvg() method is executed? Shouldn't we at least cache the results of File.ReadAllText()?** 
<br />So we get the upside of not having to request the file through http, but that way at least the browser will usually cache the file for the second time the client requests it. Using the technique above, one might think that the extension method will go and read the file on disk every time the method is ran. Luckily though, the OS is pretty good at caching filesystem operations like this. I also ran some tests on the method, and it executed on avarage in about 0.0002ms. So in this case, implementnig caching here would be a classic case of pre mature optimization, make it more complex than it needs to be and introduce more challenges and issues.

### Optimize the svg's
Even if the method to get the file contents is fast, it is definitely a good idea to optimize the svg's to reduce the size of the file size of the html-document delivered from the server. 
There are a few options for this, for example [this website](http://petercollingridge.appspot.com/svg-optimiser) and a bunch of different plugins for your build tool of choice like [gulp-svgmin](https://github.com/ben-eb/gulp-svgmin) and [grunt-svgmin](https://github.com/sindresorhus/grunt-svgmin).

### If the svg comes from a CMS

### Summary


For more reading on svg and it's usage on the web, I recommend [this css-tricks article](https://css-tricks.com/using-svg/).
