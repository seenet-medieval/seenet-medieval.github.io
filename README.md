# SEENET Main Site

Main website for the Society for Early English and Norse Electronic Texts (SEENET). The site offers information about SEENET and its publications, and is currently hosted at [SEENET.org](http://seenet.org).

## Updating and Maintaining the Site

The website is built on [Jekyll](https://jekyllrb.com/), a static website generator. Because of tight integration between Jekyll and [GitHub Pages](https://pages.github.com/), the website can be updated on GitHub and changes will appear automatically on the website without having to take any action beyond committing the changes. (Sometimes it takes a few minutes for changes to appear on the site.)

### Updating Content and Adding Pages

Site pages are stored in the root directory, ending with the `.html` extension. `index.html` is the main landing page. Each `.html` file contains an HTML snippet with only the content of the page and not the head, navigation, etc.; those are provided through a **Layout**, as explained below. Update the content of existing pages by editing these HTML files.

Each HTML file begins with a section of [Front Matter](https://jekyllrb.com/docs/frontmatter/), set off by a series of three minus signs before and after:

```yaml
---
title: [Page title, displayed in title bar]
heading: [Heading displayed at top of page; if omitted, the title is used instead]
layout: default [or the name of a different layout, if it has been created]
navigation_weight: [A number indicating in what order the page should appear in the navigation bar]
---
```

**heading** is optional (if it is absent, the title will appear at the top of the page, but **heading** can be provided to allow pages to have different headings and titles); the other elements are mandatory for pages to be rendered properly. **navigation_weight** is required in order for a page to appear in the navigation bar. The order is determined numerically; if you change the weight of one page, it will be necessary to update other pages to make sure the proper order is maintained. Lower numbers are first; the page's **title** is what appears in the bar.

To add a new page, simply create a new file in the same format as the existing files. Note that the Front Matter section is *required* for Jekyll to render the page. If you are creating an HTML page, the page content you supply should be what would go inside the `<main>` element; the default template will build the page, add the **title** or **header** specified in the Front Matter inside an `<h1>` element directly within `<main>`, and then place the rest of the content after that heading in `<main>`. You may also choose to write a new page in [Markdown](https://daringfireball.net/projects/markdown/) instead of HTML; simply create the file with the `.md` extension (Jekyll will transform it into an HTML file when building the page) and provide the required Front Matter.

### Adding News Items

The **News** page contains a complete record of news items posted to the site, and the home page always features the most recent news item. News items are created in the form of blog posts, which means that they reside in the `/_posts/` directory. Each post is an independent file.

Posts should be given a descriptive filename beginning with the post date in the format `yyyy-mm-dd-` followed by a slug that describes the post and can serve as a permanent link to that post (though we do not currently provide direct links to individual posts through the site). Please note that the date you use in the post filename will be used by Jekyll as the date of the post. Each post may be stored in Markdown or as an HTML snippet, and should be given the appropriate corresponding extension, `.md` or `.html`.

Within site templates, the HTML `<h1>` element is used to contain the post title. If internal headings are needed within a post, use `<h2>` as the highest heading level.

As with main site pages, each post also begins with a **Front Matter** section. Currently, the following fields are used:

```yaml
---
layout: post
title: "[The title of the post (in quotation marks)]"
---
```

Any features needed in future, such as short titles, authors, post times, snippets, etc. will have to be added in future both to front matter and to templates. Draft posts are also not currently supported, though they are easy to implement.

All that is necessary to make a new post is to create the file. It will immediately be added to the **News** page (in the order determined by its date), and if it has the most recent date it will be placed at the top of the home page.

### Modifying the Page Generation

Jekyll builds the content snippets into full pages using Layouts, which are provided in the `/_layouts/` directory. At present, we use a single layout, `default.html`, for all pages in order to ensure a uniform experience across the site. Layouts are created using a combination of HTML code and code in the [Liquid templating language](https://shopify.github.io/liquid/).

The Liquid code provides flexibility in generating pages by allowing us to use conditional logic (for instance, heading the page with **heading** if present, but with **title** if there is no **heading** in the Front Matter), iterate over content (for instance, building the navigation bar by looking at each page and sorting them by **navigation_weight**), and accessing data like what we provide in the Front Matter. There are many guides available online for building Jekyll layouts and working with Liquid tags.

Creating a new layout is as simple as creating a new HTML file in the `/_layouts/` directory. To use your new layout, simply reference it by name in the Front Matter of any page you want to use it. The name of the layout is what comes before the `.html` extension; for example, `/_layouts/default.html` is invoked on site pages with the line `layout: default`.

### Modifying the CSS Styling

The site CSS is available in the `/css/` directory. Jekyll supports [Sass](http://sass-lang.com/guide), a common CSS extension language. Files ending in `.sass` or `.scss` are automatically converted to `.css` files when the site is generated. For instance, `/css/seenet.scss` is written in SCSS (a dialect of Sass designed to be more CSS-like) and uses several Sass features like variables and nested templates. When someone visits the site, they receive a regular CSS file built from our SCSS file.

### Modifying Images

Images are stored in the `/images/` directory and can be uploaded or replaced there. `/images/icons/` contains resources to provide high-quality favicons to multiple platforms, generated with [RealFaviconGenerator.net](https://realfavicongenerator.net/).

## Developing Locally

The site can easily be developed and tested on a local computer. You simply need to clone the repository and run Jekyll to build your site locally.

### Prerequisites:
* Ruby ([installation instructions](https://www.ruby-lang.org/en/documentation/installation/))
  * Note for MacOS users: While Ruby comes installed with MacOS, your system version is probably too old for Jekyll. The simplest way to install a current version of Ruby is using Homebrew. First [install Homebrew](https://brew.sh/), then run `brew install ruby`. (More sophisticated options are available if you are having problems or will need to work a lot in Ruby; the [rbenv](https://github.com/rbenv/rbenv#readme) manager is widely recommended, though it can take some work to set up.)
* Jekyll ([installation instructions](https://jekyllrb.com/docs/installation/))
  * In most cases, Jekyll installation should be as simple as running `gem install jekyll`.

### Viewing the Site Locally

Once Jekyll is installed, in Terminal, navigate to the directory where you cloned this repository. You can have Jekyll build the site from the files in the repository by running `jekyll build`: the site will be output in the `/_site/` directory. (Note that it is not necessary to commit these files, as Jekyll will build them automatically on the server; `/_site/` is in `.gitignore` so Git won't notice these updates.)

In most cases, however, you're better served by running `jekyll serve`. This will initiate a local webserver on your computer, which you can use to view the site. Running the command will give you the server address; the default is `http://127.0.0.1:4000/`. Navigate to this address in your web browser and you will see a live version of your site. If you make any changes to the site code and save the files, Jekyll will automatically rebuild those pages and you can refresh them to see new versions. Note, however, that if you change the configuration in `_config.yml`, you may need to restart the server in order to see your changes.

Once you are satisfied with your changes, remember to commit them to the repository, and to push them to the server or initiate a pull request as appropriate.
