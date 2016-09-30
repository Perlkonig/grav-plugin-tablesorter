# Tablesorter Plugin

The **Tablesorter** Plugin is for [Grav CMS](http://github.com/getgrav/grav). It applies the jQuery plugin [Tablesorter](https://mottie.github.io/tablesorter/docs/) to tables in a page (v2.27.7).

For a demo, [visit my blog](https://perlkonig.com/demos/tablesorter).

## Installation

Installing the Tablesorter plugin can be done in one of two ways. The GPM (Grav Package Manager) installation method enables you to quickly and easily install the plugin with a simple terminal command, while the manual method enables you to do so via a zip file.

### GPM Installation (Preferred)

The simplest way to install this plugin is via the [Grav Package Manager (GPM)](http://learn.getgrav.org/advanced/grav-gpm) through your system's terminal (also called the command line).  From the root of your Grav install type:

    bin/gpm install tablesorter

This will install the Tablesorter plugin into your `/user/plugins` directory within Grav. Its files can be found under `/your/site/grav/user/plugins/tablesorter`.

### Manual Installation

To install this plugin, just download the zip version of this repository and unzip it under `/your/site/grav/user/plugins`. Then, rename the folder to `tablesorter`. You can find these files on [GitHub](https://github.com/Perlkonig/grav-plugin-tablesorter) or via [GetGrav.org](http://getgrav.org/downloads/plugins#extras).

You should now have all the plugin files under

    /your/site/grav/user/plugins/tablesorter
  
> NOTE: This plugin is a modular component for Grav which requires [Grav](http://github.com/getgrav/grav) and the [Error](https://github.com/getgrav/grav-plugin-error) and [Problems](https://github.com/getgrav/grav-plugin-problems) to operate.

## Configuration

Below is the default configuration. To override, first copy `plugins/tablesorter/tablesorter.yaml` to `config/plugins/tablesorter.yaml` and only edit that file. These settings can also be overridden at the page level.

```
enabled: true
active: false
include_widgets: false
include_metadata: false
production: true 
custom_path: assets/tablesorter  #no leading or trailing slash
themes: blue
table_nums: 1
```

* The `enabled` field turns the plugin off and on globally. If set to `false`, no tables will ever be affected, regardless of your page settings.

* The `active` field is what lets you activiate the plugin on a page-by-page basis. The default is `false`. To activiate it for a specific page, put the following in the page header.

  ```
  tablesorter:
    active: true
  ```

* If `include_widgets` is `true`, then the core widgets JS will be included.

* If `include_metadata` is `true`, then the core metadata JS will be included.

* If `production` is true, then the minified versions of the files will be loaded.

* The `custom_path` field is only needed if you're going to customize things (see the "Customization" section below). It's a path relative to your active theme where you will place your custom CSS and JS files. Do *not* include a leading or a trailing slash.

* The `themes` field is where you specify all the CSS themes you wish to load. Separate multiple themes with commas. A list of available themes is found at [Tablesorter's Github page](https://github.com/Mottie/tablesorter/tree/v2.27.7/dist/css).

* The `table_nums` field tells the plugin which tables in your page you wish to apply the plugin to. `1` simply means the first table in your document. If you wish to affect multiple tables, separate the numbers with commas. Note that because of how Grav parses YAML, you must enclose multiple numbers with quotation marks (e.g., `'1,2'`). Each individual table number must be listed. There is no capability to use ranges or wildcards.

## Usage

This plugin is a limited implementation of the full plugin. There is no capacity, for example, to inject metadata into the tables themselves, and widget capabilities have not been tested. What you *can* do is inject configuration variables into the initial function call. You can do this globally (a single set of arguments for all affected tables) or set them on a table-by-table basis.

To do this, add the following to your page header:

```
tablesorter:
  active: true
  args:
    ...
```

The full list of available options can be had on [the Tablesorter documentation page](https://mottie.github.io/tablesorter/docs/index.html#Configuration). What the plugin does is JSON encode the `args` content and pass that on to the function.

For example, if you want to automatically sort the first column *of all the affected tables* in ascending order, you'd do the following:

```
tablesorter:
  active: true:
  args:
    sortList: [[0,0]]
```

If you had two different tables, and you wanted the first sorted ascending but the second descending, then you'd do the following:

```
tablesorter:
  active: true:
  table_nums: '1,2'
  args:
  	1:
      sortList: [[0,0]]
    2:
      sortList: [[0,1]]
```

Themes take a little finesse. As long as you're only using a single theme for all your tables, then you can rely on the default configuration or set the `themes` field in the page header. If you want to use multiple themes in a page, though, then you need to define those themes in the `args` field as well. Let's say you had two tables and you wanted to apply the `blue` theme to the first and the `green` theme to the second:

```
tablesorter:
  active: true:
  table_nums: '1,2'
  themes: green,blue 	#order is irrelevant
  args:
  	1:
      theme: blue
    2:
      theme: green
```

## Customization

Currently the only customization supported is themes. When finding theme files to inject, the plugin first checks the `custom_path`. If it doesn't find the file there, it will pull from the plugin's `dist` folder. All you have to do is ensure you follow the naming convention: `theme.{NAME}{.min?}.css`.

The most common customization will be copying an existing theme over and just tweaking it a little.

## Future Work

I'm not a PHP native, so I welcome any pull requests to clarify documentation, clean up the code, or to add new functionality. 

- [X] Make it possible to override or insert your own themes.
- [ ] Explore available widgets and document which work and which don't.
- [ ] Look at incorporating custom parsers.
- [ ] Admin integration (I don't use it, so please submit pull requests)
