# spirals

Explore and generate different Spirals with SVG

[![](https://raw.githubusercontent.com/NetCoreApps/TemplatePages/master/src/wwwroot/assets/img/screenshots/spirals.png)](http://spirals.web-app.io)

## Install

Run as a Desktop App (Windows only):

    $ dotnet tool install -g app

    $ app install spirals
    $ cd spirals && app

Run as a .NET Core Web App (Windows, macOS, Linux):

    $ dotnet tool install -g web

    $ web install spirals
    $ cd spirals && web

> Requires [.NET Core 2.1](https://www.microsoft.com/net/download/dotnet-core/2.1).

## The Making of Spirals

[![Spirals .NET Core Desktop App](http://img.youtube.com/vi/Cf-vstYXrmY/0.jpg)](http://www.youtube.com/watch?feature=player_embedded&v=Cf-vstYXrmY)

> Video: https://youtu.be/Cf-vstYXrmY

Spirals is a good example showing how easy it is to create .NET Core Desktop Web Apps utilizing HTML5's familiar and simple development model to
leverage advanced Web Technologies like SVG in a fun, interactive and live development experience. 

To start install the dotnet `app` global tool:

    $ dotnet tool install -g app

Then create a folder for our app called `spirals` and initialize and empty Web App with `app init`:

    $ md spirals
    $ cd spirals && app init

This generates a minimal Web App but you could also start from any of the more complete 
[Web App Templates](http://templates.servicestack.net/docs/web-apps#getting-started).

Now let's open the folder up for editing in our preferred text editor, VS Code:

    $ code .

![](https://raw.githubusercontent.com/ServiceStack/docs/master/docs/images/app/app-init-code.png)

To start developing our App we just have to run `app` on the command-line which in VS Code we can open with `Terminal > New Terminal` or the **Ctrl+Shift+`** shortcut key. This will open our minimal App:

![](https://raw.githubusercontent.com/ServiceStack/docs/master/docs/images/app/app-init-run.png)

The [_layout.html](https://gist.github.com/gistlyn/5c9ee9031e53cd8f85bd0e14881ddaa8#file-_layout-html) shown above is currently where all 
the action is which we'll quickly walk through:

```html
<i hidden>{{ '/js/hot-fileloader.js' | ifDebugIncludeScript }}</i>
```

This gives us a Live Development experience in `debug` mode where it injects a script that will detect file changes **on Save** and automatically reload the page at the current scroll offset.

```html
{{ 'menu' | partial({ 
        '/':           'Home',
        '/metadata':   '/metadata',
    }) 
}}
```

This evaluates the included [_menu-partial.html](https://gist.github.com/gistlyn/5c9ee9031e53cd8f85bd0e14881ddaa8#file-_menu-partial-html) with the links
to different routes we want in our Menu on top of the page.

```html
<div id="body" class="container">
    <h2>{{title}}</h2>
    
    {{ page }}
</div>
```

The body of our App is used to render the title and contents of each page.

```html
{{ scripts | raw }}
```

If pages include any `scripts` they'll be rendered in the bottom of the page.

> The `raw` filter prevents the output from being HTML encoded.

The other 2 files included is [app.settings](https://gist.github.com/gistlyn/5c9ee9031e53cd8f85bd0e14881ddaa8#file-app-settings) containing the 
name of our App and `debug true` setting to run our App in Debug mode:

    debug true
    name My App

The template only has one page [index.html](https://gist.github.com/gistlyn/5c9ee9031e53cd8f85bd0e14881ddaa8#file-index-html) containing the `title` 
of the page in a page argument which the `_layout.html` has access to without evaluating the page, anything after is the page contents:

```html
<!-- 
title: Home Page
-->

This is the home page.
```

We can now **save** changes to any of the pages and see our changes reflected instantly in the running App. But we also have access to an even better 
live-development experience than **preview as you save** with **preview as you type** :)

### Live Previews

To take advantage of this we can exploit one of the features available in all ServiceStack Apps by clicking on `/metadata` Menu Item to view the 
[Metadata page](/metadata-page) containing links to our Apps Services, links to Metadata Services and any registered plugins:

![](https://raw.githubusercontent.com/ServiceStack/docs/master/docs/images/app/app-init-metadata.png)

Then click on [Debug Inspector](/debugging#debug-inspector) to open a real-time REPL, which is normally used to get rich insights from a running App:

![](https://raw.githubusercontent.com/ServiceStack/docs/master/docs/images/app/app-init-debug-inspector.png)

But can also be used for executing ad hoc Template Scripts. Here we can drop in any mix of HTML and templates to view results in real-time. 

In this case we want to generate SVG spirals by drawing a `circle` at each point along a 
[Archimedean spiral function](https://stackoverflow.com/a/6824451/85785) which was initially used as a base and with the help of the live REPL was 
quickly able to apply some constants to draw the **tall & narrow** spirals we want:

```hbs
<svg height="640" width="240">
{{#each range(180) }}
  {{ 120 + 100 * cos((1) * it * 0.02827) | assignTo: x }}
  {{ 320 + 300 * sin((1) * it * 0.02827) | assignTo: y }}
  <circle cx="{{x}}" cy="{{y}}" r="10" fill="rgb(0,100,0)" stroke="black" stroke-width="1"/>
{{/each}} 
</svg>
```

We can further explore different spirals by modifying `x` and `y` cos/sin constants:

![](https://raw.githubusercontent.com/ServiceStack/docs/master/docs/images/app/spirals/single.gif)

Out of the spirals we've seen lets pick one of the interesting ones and add it to our `index.html`, let's also enhance them by modifying the `fill` and
`radius` properties with different weightings and compare them side-by-side:

```hbs
<svg height="640" width="240">
{{#each range(180) }}
  {{ 120 + 100 * cos((5) * it * 0.02827) | assignTo: x }}
  {{ 320 + 300 * sin((1) * it * 0.02827) | assignTo: y }}
  <circle cx="{{x}}" cy="{{y}}" r="10" fill="rgb(0,100,0)" stroke="black" stroke-width="1"/>
{{/each}} 
</svg>

<svg height="640" width="240">
{{#each range(180) }}
  {{ 120 + 100 * cos((5) * it * 0.02827) | assignTo: x }}
  {{ 320 + 300 * sin((1) * it * 0.02827) | assignTo: y }}
  <circle cx="{{x}}" cy="{{y}}" r="10" fill="rgb(0,{{it*1.4}},0)" stroke="black" stroke-width="1"/>
{{/each}} 
</svg>

<svg height="640" width="240">
{{#each range(180) }}
  {{ 120 + 100 * cos((5) * it * 0.02827) | assignTo: x }}
  {{ 320 + 300 * sin((1) * it * 0.02827) | assignTo: y }}
  <circle cx="{{x}}" cy="{{y}}" r="{{it*0.1}}" fill="rgb(0,{{it*1.4}},0)" stroke="black" stroke-width="1"/>
{{/each}} 
</svg>
```

> You can use `ALT+LEFT` + `ALT+RIGHT` shortcut keys to navigate back and forward to the home page.

Great, hitting `save` again will show us the effects of each change side-by-size:

[![](https://raw.githubusercontent.com/ServiceStack/docs/master/docs/images/app/spirals/single-fill-radius.png)](http://spirals.web-app.io)

### Multiplying

Now that we have the effect that we want, let's go back to the debug inspector and see what a number of different spirals look side-by-side
by wrapping our last svg snippet in another each block:

```hbs
<table>{{#each i in range(0, 4) }}
<svg height="640" width="240">
{{#each range(180) }}
  {{ 120 + 100 * cos((1)   * it * 0.02827) | assignTo: x }}
  {{ 320 + 300 * sin((1+i) * it * 0.02827) | assignTo: y }}
  <circle cx="{{x}}" cy="{{y}}" r="{{it*0.1}}" fill="rgb(0,{{it*1.4}},0)" stroke="black" stroke-width="1"/>
{{/each}} 
</svg>
{{/each}}
```

We can prefix our snippet with `<table>` as a temp workaround to force them to display side-by-side in Debug Inspector. In order to 
for spirals to distort we'll only change 1 of the axis, as they're tall & narrow lets explore along the y-axis:

![](https://raw.githubusercontent.com/ServiceStack/docs/master/docs/images/app/spirals/multi-01.png)

We're all setup to begin our pattern explorer expedition where we can navigate across the `range()` index both incrementally and logarithmically across
to quickly discover new aesthetically pleasing patterns :)

![](https://raw.githubusercontent.com/ServiceStack/docs/master/docs/images/app/spirals/multi.gif)

Let's go back to our App and embody our multi spiral viewer in a new `multi.html` page containing:

```hbs
{{#each i in range(0, 4) }}
<svg height="640" width="240">
{{#each range(180) }}
  {{ 120 + 100 * cos((5)   * it * 0.02827) | assignTo: x }}
  {{ 320 + 300 * sin((1+i) * it * 0.02827) | assignTo: y }}
  <circle cx="{{x}}" cy="{{y}}" r="{{it*0.1}}" fill="rgb(0,{{it*1.4}},0)" stroke="black" stroke-width="1"/>
{{/each}} 
</svg>
{{/each}}
```

Then make it navigable by adding a link to our new page in the `_layout.html` menu:

```hbs
{{ 'menu' | partial({
     '/':           'Home',
     '/multi':      'Multi',
     '/metadata':   '/metadata',
   })
}}
```

Where upon save, our creation will reveal itself in the App's menu:

[![](https://raw.githubusercontent.com/ServiceStack/docs/master/docs/images/app/spirals/multi.png)](http://spirals.web-app.io/multi)

### Animating

With the help of SVG's [`<animate>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/animate) we can easily bring our spirals to life
by animating different properties on the generated SVG circles.

As we have to wait for the animation to complete before trying out different effects, we won't benefit from Debug Inspector's live REPL
so let's jump straight in and create a new `animated.html` and add a link to it in the menu:

```hbs
{{ 'menu' | partial({
     '/':           'Home',
     '/multi':      'Multi',
     '/animated':   'Animated',
     '/metadata':   '/metadata',
   })
}}
```

Then populate it with a copy of `multi.html` and sprinkle in some `<animate>` elements to cycle through different `<circle>` property values. 
We're entering the "creative process" of our App where we can try out different values, hit **Save** and watch the effects of our tuning eventually 
arriving at a combination we like:

```hbs
{{#each i in range(0, 4) }}
<svg height="640" width="240">
{{#each range(180) }}
  {{ 120 + 100 * cos((5)   * it * 0.02827) | assignTo: x }}
  {{ 320 + 300 * sin((1+i) * it * 0.02827) | assignTo: y }}
  <circle cx="{{x}}" cy="{{y}}" r="{{it*0.1}}" fill="rgb(0,{{it*1.4}},0)" stroke="black" stroke-width="1">
    <animate attributeName="fill" values="green;yellow;red;green" dur="{{it%10}}s" repeatCount="indefinite" />
    <animate attributeName="opacity" values="1;.5;1" dur="5s" repeatCount="indefinite" />
    <animate attributeName="cx" values="{{x}};{{x*1.02}};{{x*0.98}};{{x}}" dur="3s" repeatCount="indefinite" />
    <animate attributeName="cy" values="{{y}};{{y*1.02}};{{y*0.98}};{{y}}" dur="3s" repeatCount="indefinite" />
  </circle>
{{/each}} 
</svg>
{{/each}}
```

Although hard to capture in a screenshot, we can sit back and watch our living, breathing Spirals :)

[![](https://raw.githubusercontent.com/ServiceStack/docs/master/docs/images/app/spirals/animated.png)](http://spirals.web-app.io/animated?from=0)

> Checkout [spirals.web-app.io](http://spirals.web-app.io/animated?from=0) for the animated version.

### Navigating

Lets expand our App beyond these static Spirals by enabling some navigation, this is easily done by adding the snippet below on the top of the home page:

```hbs
{{ qs.from ?? 1 | toInt | assignTo: from }}
<div style="text-align:right;margin:-54px 0 30px 0">
  {{#if from > 1}} <a href="?from={{ max(from-1,0) }}" title="{{max(from-1,0)}}">previous</a> |{{/if}}
  {{from}} | <a href="?from={{ from+1 }}" title="{{max(from-1,0)}}">next</a>
</div>
```

Whilst the `multi.html` and `animated.html` pages can skip by 4:

```hbs
{{ qs.from ?? 1 | toInt | assignTo: from }}
<div style="text-align:right;margin:-54px 0 30px 0">
  {{#if from > 1}} <a href="?from={{ max(from-4,0) }}" title="{{max(from-1,0)}}">previous</a> |{{/if}}
  {{from}} | <a href="?from={{ from+4 }}" title="{{max(from-1,0)}}">next</a>
</div>
```

Then changing the `index.html` SVG fragment to use the `from` value on the y-axis:

```hbs
<svg height="640" width="240">
{{#each range(180) }}
  {{ 120 + 100 * cos((5)    * it * 0.02827) | assignTo: x }}
  {{ 320 + 300 * sin((from) * it * 0.02827) | assignTo: y }}
  <circle cx="{{x}}" cy="{{y}}" r="10" fill="rgb(0,100,0)" stroke="black" stroke-width="1"/>
{{/each}} 
</svg>
```

Whilst the `multi.html` and `animated.html` pages can use it in its `range(from, 4)` function:

```hbs
{{#each i in range(from, 4) }}
<svg height="640" width="240">
{{#each range(180) }}
  {{ 120 + 100 * cos((5) * it * 0.02827) | assignTo: x }}
  {{ 320 + 300 * sin((1+i)  * it * 0.02827) | assignTo: y }}
  <circle cx="{{x}}" cy="{{y}}" r="{{it*0.1}}" fill="rgb(0,{{it*1.4}},0)" stroke="black" stroke-width="1"/>
{{/each}} 
</svg>
{{/each}}
```

With navigation activated we can quickly scroll through and explore different spirals. To save ourselves the effort of finding them again lets
catalog our favorite picks and add them to a bookmarked list at the bottom of the page. Here are some interesting ones I've found for the home page:

```hbs
<div>
  Jump to favorites: 
  {{#each [1,5,101,221,222,224,298,441,443,558,663,665,666,783,888] }}
    {{#if index > 0}} | {{/if}} {{#if from == it }} {{it}} {{else}} <a href="?from={{it}}">{{it}</a> {{/if}}
  {{/each}}
</div>
```

and my top picks for the `multi.html` and `animated.html` pages:

```hbs
<div>
  Jump to favorites: 
  {{#each [1,217,225,229,441,449,661,669,673,885,1338,3326,3338,4330,8662,9330,11998] }}
    {{#if index > 0}} | {{/if}} {{#if from == it }} {{it}} {{else}} <a href="?from={{it}}">{{it}</a> {{/if}}
  {{/each}}
</div>
```

> If you've found more interesting ones, [let me know](https://github.com/mythz/spirals/issues)!

Now it's just a matter of signing off our digital piece by giving it a name in your `app.settings`:

    name Spirals

Which replaces the name in the `menu` and used in any shortcuts that are created, and with those finishing touches our App's journey
into the rich colorful world of SVG is complete:

[![](https://raw.githubusercontent.com/ServiceStack/docs/master/docs/images/app/spirals/spiral-nav.png)](http://spirals.web-app.io/animated)

### Recap

If you've reached this far you've created a beautiful Desktop App without waiting for a single build or App restart, in a live and 
interactive environment which let you rapidly prototype and get real-time feedback of any experimental changes, to produce a 
rich graphical app with fast and fluid animations taking advantage of the impressive engineering invested in Chrome, implemented
declaratively by just generating SVG markup. 

As Bret Victor shows us in his seminal [Inventing on Principle](https://vimeo.com/36579366) presentation, being able to immediately visualize 
changes gives us a deep connection and intuition with our creations that's especially important when creating visual software like UIs where 
the rapid iterations lets us try out and perfect different ideas in the moment, as soon as we can think of them.

It's a good example of the type of App that would rarely be considered to be created with a native GUI toolkit and 2D drawing API, the amount
of effort and friction to achieve the same result would take the enjoyment out of the experience and the end result would have much less utility
where it wouldn't be able to be re-used on different OS's or other websites. 

## Publishing your App

To share our digital masterpiece with the world we just need to publish it in a GitHub repo, which I've already done for my Spirals app at: 
[https://github.com/mythz/spirals](github.com/mythz/spirals).

Anyone will then be able to install your App by first downloading the `app` tool themselves ([.NET Core 2.1 Required](https://www.microsoft.com/net/download/dotnet-core/2.1)):

    $ dotnet tool install -g app

Then running `install` with the name of the **repo** and your GitHub **User** or **Organization** name in the `--source` argument:

    $ app install spirals --source mythz

Which installs instantly thanks to the `7kb` .zip download that can then be opened by double-clicking on the generated **Spirals** Desktop Shortcut:

![](https://raw.githubusercontent.com/ServiceStack/docs/master/docs/images/app/app-install-spirals.png)

### Publishing your App with binaries

The unique characteristics of Web Apps affords us different ways of publishing your App, e.g. to save users from needing to install the 
`app` tool you can run `publish` in your App's directory:

    $ app publish

Which will copy your App to the `publish/app` folder and the `app` tool binaries in the `publish/cef` folder:

![](https://raw.githubusercontent.com/ServiceStack/docs/master/docs/images/app/app-publish.png)

A Desktop shortcut is also generated for convenience although this is dependent on where the App is installed on the end users computer. 
If you know it will be in a fixed location you can update the **Target** and **Start in** properties to reference the `cef\app.dll` and
`/app` folder:

![](https://raw.githubusercontent.com/ServiceStack/docs/master/docs/images/app/app-shortcut-properties.png)

This includes all `app` binaries needed to run Web Apps which compresses to **89 MB** in a `.zip` or **61 MB** in 7-zip `.7z` archive.

### Publishing a self-contained Windows 64 executable

But that's not all, we can even save end users who want to run your app the inconvenience of installing .NET Core :) by creating a self-contained 
executable with:

    $ app publish-exe

![](https://raw.githubusercontent.com/ServiceStack/docs/master/docs/images/app/app-publish-exe.png)

This downloads the [WebWin](https://github.com/ServiceStack/WebWin) self-contained .NET Core 2.1 binaries and copies then to `publish/win` folder with 
the app copied to `publish/app`.

This publishing option includes a self-contained .NET Core with all `app` binaries which compresses to **121 MB** in a `.zip` or **83 MB** in 7-zip `.7z` archive.

### Publish to the world

To maximize reach and accessibility of your App leave a comment on the [App Gallery](https://gist.github.com/gistlyn/f555677c98fb235dccadcf6d87b9d098)
where after we link to it on [NetCoreWebApps](https://github.com/NetCoreWebApps) it will available to all users when they look for available apps in:

    $ app list

Which can then be installed with:

    $ app install spirals

### Learnable Living Apps

A beautiful feature of Web Apps is that they can be enhanced and customized in real-time whilst the App is running. No pre-installation of dev tools
or knowledge of .NET and C# is required, anyone can simply use any text editor to peek inside `.html` files to see how it works and see any of their
customizations visible in real-time. 

Apps can be customized using both the JavaScript-like [Templates language](http://templates.servicestack.net) syntax for any "server" HTML generation
which can be further customized by any JavaScript on the client.

We'd loved to see this aspect of Web Apps utilized as we believe it creates an enjoyable environment that's an easy introduction for curious people 
who are new and want to learn programming. We believe it can serve as a great tool to have in a classroom where students can install a "half-finished"
App that students can have fun experimenting with and complete.

## Learn More

See [templates.servicestack.net/docs/web-apps](http://templates.servicestack.net/docs/web-apps) to learn about ServiceStack .NET Core 2.1 Web Apps.
