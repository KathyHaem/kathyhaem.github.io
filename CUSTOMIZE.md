
<!--ts-->

- [Customize](#customize)
  - [Project structure](#project-structure)
  - [Configuration](#configuration)
  - [GitHub Copilot Customization Agent](#github-copilot-customization-agent)
    - [What the Agent Can Help With](#what-the-agent-can-help-with)
    - [How to Use the Agent](#how-to-use-the-agent)
    - [Important: Verify Agent Output](#important-verify-agent-output)
  - [Understanding the Codebase with Code Wiki and DeepWiki](#understanding-the-codebase-with-code-wiki-and-deepwiki)
    - [What are these tools?](#what-are-these-tools)
    - [When to use them](#when-to-use-them)
  - [Technology Stack](#technology-stack)
    - [Frontend](#frontend)
    - [Backend](#backend)
    - [Build and Deployment](#build-and-deployment)
    - [Key Integration Points](#key-integration-points)
  - [Modifying the CV information](#modifying-the-cv-information)
  - [Modifying the user and repository information](#modifying-the-user-and-repository-information)
    - [Configuring external service URLs](#configuring-external-service-urls)
  - [Creating new pages](#creating-new-pages)
  - [Creating new blog posts](#creating-new-blog-posts)
  - [Creating new projects](#creating-new-projects)
  - [Adding some news](#adding-some-news)
  - [Adding Collections](#adding-collections)
    - [Creating a new collection](#creating-a-new-collection)
    - [Using frontmatter fields in your collection](#using-frontmatter-fields-in-your-collection)
    - [Collections with categories and tags](#collections-with-categories-and-tags)
  - [Adding a new publication](#adding-a-new-publication)
    - [Author annotation](#author-annotation)
    - [Buttons (through custom bibtex keywords)](#buttons-through-custom-bibtex-keywords)
  - [Changing theme color](#changing-theme-color)
  - [Customizing layout and UI](#customizing-layout-and-ui)
  - [Adding social media information](#adding-social-media-information)
  - [Adding a newsletter](#adding-a-newsletter)
  - [Configuring search features](#configuring-search-features)
  - [Managing publication display](#managing-publication-display)
  - [Updating third-party libraries](#updating-third-party-libraries)
  - [Removing content](#removing-content)
    - [Removing the blog page](#removing-the-blog-page)
    - [Removing the news section](#removing-the-news-section)
    - [Removing the projects page](#removing-the-projects-page)
    - [Removing the publications page](#removing-the-publications-page)
    - [Removing the repositories page](#removing-the-repositories-page)
    - [You can also remove pages through commenting out front-matter blocks](#you-can-also-remove-pages-through-commenting-out-front-matter-blocks)
  - [Adding Token for Lighthouse Badger](#adding-token-for-lighthouse-badger)
    - [Personal Access Token (fine-grained) Permissions for Lighthouse Badger:](#personal-access-token-fine-grained-permissions-for-lighthouse-badger)
  - [Customizing fonts, spacing, and more](#customizing-fonts-spacing-and-more)
  - [Scheduled Posts](#scheduled-posts)
    - [Name Format](#name-format)
    - [Important Notes](#important-notes)
  - [Setting up a Personal Access Token (PAT) for Google Scholar Citation Updates](#setting-up-a-personal-access-token-pat-for-google-scholar-citation-updates)
    - [Why is a PAT required?](#why-is-a-pat-required)
    - [How to set up the PAT](#how-to-set-up-the-pat)

<!--te-->

## Project structure

The project is structured as follows, focusing on the main components that you will need to modify:

```txt
.
├── 📂 assets/: contains the assets that are displayed in the website
│   └── 📂 json/
    │   └── 📄 resume.json: CV in JSON format (https://jsonresume.org/)
├── 📂 _bibliography/
│   └── 📄 papers.bib: bibliography in BibTeX format
├── 📂 _books/: contains the bookshelf pages
├── 📄 _config.yml: the configuration file of the template
├── 📂 _data/: contains some of the data used in the template
│   ├── 📄 cv.yml: CV in YAML format, used when assets/json/resume.json is not found
│   ├── 📄 repositories.yml: users and repositories info in YAML format
│   └── 📄 socials.yml: your social media and contact info in YAML format
├── 📂 _includes/: contains code parts that are included in the main HTML file
│   └── 📄 news.liquid: defines the news section layout in the about page
├── 📂 _layouts/: contains the layouts to choose from in the frontmatter of the Markdown files
├── 📂 _news/: the news that will appear in the news section in the about page
├── 📂 _pages/: contains the pages of the website
|   └── 📄 404.md: 404 page (page not found)
├── 📂 _posts/: contains the blog posts
├── 📂 _projects/: contains the projects
└── 📂 _sass/: contains the SASS files that define the style of the website
    ├── 📄 _base.scss: base style of the website
    ├── 📄 _cv.scss: style of the CV page
    ├── 📄 _distill.scss: style of the Distill articles
    ├── 📄 _layout.scss: style of the overall layout
    ├── 📄 _themes.scss: themes colors and a few icons
    └── 📄 _variables.scss: variables used in the SASS files
```

## Configuration

The configuration file [\_config.yml](_config.yml) contains the main configuration of the website.
Most of the settings is self-explanatory and we also tried to add as much comments as possible.
If you have any questions, please check if it was not already answered in the [FAQ](FAQ.md).

> Note that the `url` and `baseurl` settings are used to generate the links of the website, as explained in the [install instructions](INSTALL.md).

All changes made to this file are only visible after you rebuild the website. That means that you need to run `bundle exec jekyll serve` again if you are running the website locally or push your changes to GitHub if you are using GitHub Pages. All other changes are visible immediately, you only need to refresh the page.


## Modifying the CV information

There are currently 2 different ways of generating the CV page content. The first one is by using a json file located in [assets/json/resume.json](assets/json/resume.json). It is a [known standard](https://jsonresume.org/) for creating a CV programmatically. The second one, currently used as a fallback when the json file is not found, is by using a yml file located in [\_data/cv.yml](_data/cv.yml). This was the original way of creating the CV page content and since it is more human readable than a json file we decided to keep it as an option.

What this means is, if there is no resume data defined in [\_config.yml](_config.yml) and loaded via a json file, it will load the contents of [\_data/cv.yml](_data/cv.yml). If you want to use the [\_data/cv.yml](_data/cv.yml) file as the source of your CV, you must delete the [assets/json/resume.json](assets/json/resume.json) file.

## Modifying the user and repository information

The user and repository information is defined in [\_data/repositories.yml](_data/repositories.yml). You can add as many users and repositories as you want. Both informations are used in the `repositories` section.

### Configuring external service URLs

The repository page uses external services to display GitHub statistics and trophies. By default, these are:

- `github-readme-stats.vercel.app` for user stats and repository cards
- `github-profile-trophy.vercel.app` for GitHub profile trophies

**Important:** These default services are hosted by third parties and may not be available 100% of the time. For better reliability, privacy, and customization, you can self-host these services and configure your website to use your own instances.

To use your own instances of these services, configure the URLs in [\_config.yml](_config.yml):

```yaml
external_services:
  github_readme_stats_url: https://github-readme-stats.vercel.app
  github_profile_trophy_url: https://github-profile-trophy.vercel.app
```

To self-host these services, follow the deployment instructions in their respective repositories:

- [github-readme-stats](https://github.com/anuraghazra/github-readme-stats)
- [github-profile-trophy](https://github.com/ryo-ma/github-profile-trophy)

Once deployed, update the URLs above to point to your custom deployment.

## Creating new pages

You can create new pages by adding new Markdown files in the [\_pages](_pages/) directory. The easiest way to do this is to copy an existing page and modify it. You can choose the layout of the page by changing the [layout](https://jekyllrb.com/docs/layouts/) attribute in the [frontmatter](https://jekyllrb.com/docs/front-matter/) of the Markdown file, and also the path to access it by changing the [permalink](https://jekyllrb.com/docs/permalinks/) attribute. You can also add new layouts in the [\_layouts](_layouts/) directory if you feel the need for it.

## Creating new blog posts

To create a new blog post, you can add a new Markdown file in the [\_posts](_posts/) directory, which is the [default location for posts in Jekyll](https://jekyllrb.com/docs/posts/). The [name of the file must follow](https://jekyllrb.com/docs/posts/#creating-posts) the format `YYYY-MM-DD-title.md`. The easiest way to do this is to copy an existing blog post and modify it. Note that some blog posts have optional fields in the [frontmatter](https://jekyllrb.com/docs/front-matter/) that are used to enable specific behaviors or functions.

If you want to create blog posts that are not ready to be published, but you want to track it with git, you can create a [\_drafts](https://jekyllrb.com/docs/posts/#drafts) directory and store them there.

Note that `posts` is also a collection, but it is a default collection created automatically by Jekyll. To access the posts, you can use the `site.posts` variable in your templates.

## Creating new projects

You can create new projects by adding new Markdown files in the [\_projects](_projects/) directory. The easiest way to do this is to copy an existing project and modify it.

## Adding some news

You can add news in the about page by adding new Markdown files in the [\_news](_news/) directory. There are currently two types of news: inline news and news with a link. News with a link take you to a new page while inline news are displayed directly in the about page. The easiest way to create yours is to copy an existing news and modify it.

## Adding Collections

This Jekyll theme implements [collections](https://jekyllrb.com/docs/collections/) to let you break up your work into categories.
The theme comes with three default collections: `news`, `projects`, and `books`.
Items from the `news` collection are automatically displayed on the home page, while items from the `projects` collection are displayed on a responsive grid on the projects page, and items from the `books` collection are displayed on its own `bookshelf` page inside `submenus`.

You can easily create your own collections for any type of content—teaching materials, courses, apps, short stories, or whatever suits your needs.

### Creating a new collection

To create a new collection, follow these steps. We will create a `teaching` collection, but you can replace `teaching` with any name you prefer:

1. **Add the collection to `_config.yml`**

   Open the `collections` section in [\_config.yml](_config.yml) and add your new collection:

   ```yaml
   collections:
     news:
       defaults:
         layout: post
       output: true
     projects:
       output: true
     teaching:
       output: true
       permalink: /teaching/:path/
   ```

   - `output: true` makes the collection items accessible as separate pages
   - `permalink` defines the URL path for each collection item (`:path` is replaced with the filename)
     - Note: You can customize the [permalink structure](https://jekyllrb.com/docs/permalinks/#collections) as needed. If not set, it uses `/COLLECTION_NAME/:name/`.

2. **Create a folder for your collection items**

   Create a new folder in the root directory with an underscore prefix, matching your collection name. For a `teaching` collection, create `_teaching/`:

   ```text
   _teaching/
   ├── course_1.md
   ├── course_2.md
   └── course_3.md
   ```

3. **Create a landing page for your collection**

   Add a Markdown file in `_pages/` (e.g., `teaching.md`) that will serve as the main page for your collection. You can use [\_pages/projects.md](_pages/projects.md) or [\_pages/books.md](_pages/books.md) as a template and adapt it for your needs.

   In your landing page, access your collection using the `site.COLLECTION_NAME` variable:

   ```liquid
   {% assign teaching_items = site.teaching | sort: 'date' | reverse %}

   {% for item in teaching_items %}
     <h3>{{ item.title }}</h3>
     <p>{{ item.content }}</p>
   {% endfor %}
   ```

   Replace `COLLECTION_NAME` with your actual collection name (e.g., `site.teaching`).

4. **Add a link to your collection page**

   Update [\_pages/dropdown.md](_pages/dropdown.md) or the navigation configuration in [\_config.yml](_config.yml) to add a menu link to your new page.

5. **Create collection items**

   Add Markdown files in your new collection folder (e.g., `_teaching/`) with appropriate frontmatter and content.

For more information regarding collections, check [Jekyll official documentation](https://jekyllrb.com/docs/collections/) and [step-by=step guide](https://jekyllrb.com/docs/step-by-step/09-collections/).

### Using frontmatter fields in your collection

When creating items in your collection, you can define custom frontmatter fields and use them in your landing page. For example:

```markdown
---
layout: page
title: Introduction to Research Methods
importance: 1
category: methods
---

Course description and content here...
```

Then in your landing page template:

```liquid
{% if item.category == 'methods' %}
  <span class="badge">{{ item.category }}</span>
{% endif %}
```

### Collections with categories and tags

If you want to add category and tag support (like the blog posts have), you need to configure the `jekyll-archives` section in [\_config.yml](_config.yml). See how this is done with the `books` collection for reference. For more details, check the [jekyll-archives-v2 documentation](https://george-gca.github.io/jekyll-archives-v2/).


## Publications

To add publications create a new entry in the [\_bibliography/papers.bib](_bibliography/papers.bib) file.
You can find the BibTeX entry of a publication in Google Scholar by clicking on the quotation marks below the publication title, then clicking on "BibTeX", or also in the conference page itself.
By default, the publications will be sorted by year and the most recent will be displayed first. You can change this behavior and more in the `Jekyll Scholar` section in [\_config.yml](_config.yml) file.

You can add extra information to a publication, like a PDF file in the `assets/pdfs/` directory and add the path to the PDF file in the BibTeX entry with the `pdf` field.
Some of the supported fields are: `abstract`, `altmetric`, `annotation`, `arxiv`, `bibtex_show`, `blog`, `code`, `dimensions`, `doi`, `eprint`, `hal`, `html`, `isbn`, `pdf`, `pmid`, `poster`, `slides`, `supp`, `video`, and `website`.

### Author annotation

In publications, the author entry for yourself is identified by string array `scholar:last_name` and string array `scholar:first_name` in [\_config.yml](_config.yml).
For example, if you have the following entry in your [\_config.yml](_config.yml):

```yaml
scholar:
  last_name: [Einstein]
  first_name: [Albert, A.]
```

If the entry matches one form of the last names and the first names, it will be underlined.

Keep meta-information about your co-authors in [\_data/coauthors.yml](_data/coauthors.yml) and Jekyll will insert links to their webpages automatically.
The co-author data format is as follows, with the last names lower cased and without accents as the key:

```yaml
"adams":
  - firstname: ["Edwin", "E.", "E. P.", "Edwin Plimpton"]
    url: https://en.wikipedia.org/wiki/Edwin_Plimpton_Adams

"podolsky":
  - firstname: ["Boris", "B.", "B. Y.", "Boris Yakovlevich"]
    url: https://en.wikipedia.org/wiki/Boris_Podolsky

"rosen":
  - firstname: ["Nathan", "N."]
    url: https://en.wikipedia.org/wiki/Nathan_Rosen

"bach":
  - firstname: ["Johann Sebastian", "J. S."]
    url: https://en.wikipedia.org/wiki/Johann_Sebastian_Bach

  - firstname: ["Carl Philipp Emanuel", "C. P. E."]
    url: https://en.wikipedia.org/wiki/Carl_Philipp_Emanuel_Bach
```

If the entry matches one of the combinations of the last names and the first names, it will be highlighted and linked to the url provided. Note that the keys **MUST BE** lower cased and **MUST NOT** contain accents. This is because the keys are used to match the last names in the BibTeX entries, considering possible variations (see [related discussion](https://github.com/alshedivat/al-folio/discussions/2213)).

### Buttons (through custom bibtex keywords)

There are several custom bibtex keywords that you can use to affect how the entries are displayed on the webpage:

- `abbr`: Adds an abbreviation to the left of the entry. You can add links to these by creating a venue.yaml-file in the \_data folder and adding entries that match.
- `abstract`: Adds an "Abs" button that expands a hidden text field when clicked to show the abstract text
- `altmetric`: Adds an [Altmetric](https://www.altmetric.com/) badge (Note: if DOI is provided just use `true`, otherwise only add the altmetric identifier here - the link is generated automatically)
- `annotation`: Adds a popover info message to the end of the author list that can potentially be used to clarify superscripts. HTML is allowed.
- `arxiv`: Adds a link to the Arxiv website (Note: only add the arxiv identifier here - the link is generated automatically)
- `bibtex_show`: Adds a "Bib" button that expands a hidden text field with the full bibliography entry
- `blog`: Adds a "Blog" button redirecting to the specified link
- `code`: Adds a "Code" button redirecting to the specified link
- `dimensions`: Adds a [Dimensions](https://www.dimensions.ai/) badge (Note: if DOI or PMID is provided just use `true`, otherwise only add the Dimensions' identifier here - the link is generated automatically)
- `hal`: Adds a link to the HAL website (Note: only add the hal identifier (hal-xxx or tel-xxx) here - the link is generated automatically)
- `html`: Inserts an "HTML" button redirecting to the user-specified link
- `pdf`: Adds a "PDF" button redirecting to a specified file (if a full link is not specified, the file will be assumed to be placed in the /assets/pdf/ directory)
- `poster`: Adds a "Poster" button redirecting to a specified file (if a full link is not specified, the file will be assumed to be placed in the /assets/pdf/ directory)
- `slides`: Adds a "Slides" button redirecting to a specified file (if a full link is not specified, the file will be assumed to be placed in the /assets/pdf/ directory)
- `supp`: Adds a "Supp" button to a specified file (if a full link is not specified, the file will be assumed to be placed in the /assets/pdf/ directory)
- `website`: Adds a "Website" button redirecting to the specified link

You can implement your own buttons by editing the [\_layouts/bib.liquid](_layouts/bib.liquid) file.

## Changing theme color

A variety of beautiful theme colors have been selected for you to choose from.
The default is purple, but you can quickly change it by editing the `--global-theme-color` variable in the [\_sass/\_themes.scss](_sass/_themes.scss) file.
Other color variables are listed there as well.
The stock theme color options available can be found at [\_sass/\_variables.scss](_sass/_variables.scss).
You can also add your own colors to this file assigning each a name for ease of use across the template.

## Customizing layout and UI

You can customize the layout and user interface in [\_config.yml](_config.yml):

```yaml
navbar_fixed: true
footer_fixed: true
back_to_top: true
max_width: 930px
```

- `navbar_fixed`: When `true`, the navigation bar stays fixed at the top of the page when scrolling. When `false`, it scrolls with the page content.
- `footer_fixed`: When `true`, the footer remains fixed at the bottom of the viewport. When `false`, it appears at the end of the page content.
- `back_to_top`: Displays a "back to top" button in the footer. When clicked, it smoothly scrolls the page back to the top.
- `max_width`: Controls the maximum width of the main content area in pixels. The default is `930px`. You can adjust this to make your content wider or narrower.

## Adding social media information

You can add your social media links by adding the specified information in the [\_data/socials.yml](_data/socials.yml) file.
This information will appear at the bottom of the `About` page and in the search results by default, but this could be changed to appear at the header of the page by setting `enable_navbar_social: true` and doesn't appear in the search by setting `socials_in_search: false`, both in [\_config.yml](_config.yml).

## Adding a newsletter

You can add a newsletter subscription form by adding the specified information at the `newsletter` section in the [\_config.yml](_config.yml) file.
To set up a newsletter, you can use a service like [Loops.so](https://loops.so/), which is the current supported solution. Once you have set up your newsletter, you can add the form [endpoint](https://loops.so/docs/forms/custom-form) to the `endpoint` field in the `newsletter` section of the [\_config.yml](_config.yml) file.

Depending on your specified footer behavior, the sign up form either will appear at the bottom of the `About` page and at the bottom of blogposts if `related_posts` are enabled, or in the footer at the bottom of each page.

## Configuring search features

The theme includes a powerful search functionality that can be customized in [\_config.yml](_config.yml):

```yaml
search_enabled: true
socials_in_search: true
posts_in_search: true
bib_search: true
```

- `search_enabled`: Enables the site-wide search feature. When enabled, a search box appears in the navigation bar, allowing users to search across your site content.
- `socials_in_search`: Includes your social media links and contact information in search results. This makes it easier for visitors to find ways to connect with you.
- `posts_in_search`: Includes blog posts in the search index. Users can search for posts by title, content, or tags.
- `bib_search`: Enables search within your publications/bibliography. When enabled, a search box appears on the publications page, allowing visitors to filter publications by title, author, venue, or year.

All these search features work in real-time and do not require a page reload.

## Managing publication display

The theme offers several options for customizing how publications are displayed:

```yaml
enable_publication_thumbnails: true
max_author_limit: 3
more_authors_animation_delay: 10
```

- `enable_publication_thumbnails`: When `true`, displays preview images for publications (if specified in the BibTeX entry with the `preview` field). Set to `false` to disable thumbnails for all publications.
- `max_author_limit`: Sets the maximum number of authors shown initially for each publication. If a publication has more authors, they are hidden behind a "more authors" link. Leave blank to always show all authors.
- `more_authors_animation_delay`: Controls the animation speed (in milliseconds) when revealing additional authors. A smaller value means faster animation.

To add a thumbnail to a publication, include a `preview` field in your BibTeX entry:

```bibtex
@article{example2024,
  title={Example Paper},
  author={Author, First and Author, Second},
  journal={Example Journal},
  year={2024},
  preview={example_preview.png}
}
```

Place the image file in `assets/img/publication_preview/`.

## Updating third-party libraries

The theme uses various third-party JavaScript and CSS libraries. You can manage these in the `third_party_libraries` section of [\_config.yml](_config.yml):

```yaml
third_party_libraries:
  download: false
  bootstrap-table:
    version: "1.22.4"
    url:
      css: "https://cdn.jsdelivr.net/npm/bootstrap-table@{{version}}/dist/bootstrap-table.min.css"
      js: "https://cdn.jsdelivr.net/npm/bootstrap-table@{{version}}/dist/bootstrap-table.min.js"
    integrity:
      css: "sha256-..."
      js: "sha256-..."
```

- `download`: When `false` (default), libraries are loaded from CDNs. When `true`, the specified library versions are downloaded during build and served from your site.
   This can improve performance but increases your repository size.
- `version`: Specifies which version of each library to use. Update this to use a newer version.
- `url`: Template URLs for loading the library. The `{{version}}` placeholder is replaced with the version number.
- `integrity`: [Subresource Integrity (SRI)](https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity) hashes ensure that the library hasn't been tampered with. When updating a library version, you should also update its integrity hash.

To update a library:

1. Change the `version` number
2. Obtain the new integrity hash for the updated library version and update the `integrity` field with the new hash. You can:
   - Check if the CDN provider (e.g., jsDelivr, cdnjs, unpkg) provides the SRI hash for the file. Many CDN sites display the SRI hash alongside the file URL.
   - Generate the SRI hash yourself using a tool such as [SRI Hash Generator](https://www.srihash.org/) or by running the following command in your terminal:

     ```bash
     curl -sL [FILE_URL] | openssl dgst -sha384 -binary | openssl base64 -A
     ```

     Replace `[FILE_URL]` with the URL of the library file. Then, prefix the result with `sha384-` and use it in the `integrity` field.
     For detailed instructions on updating specific libraries, see the FAQ:
     - [How can I update Academicons version](FAQ.md#how-can-i-update-academicons-version-on-the-template)
     - [How can I update Font Awesome version](FAQ.md#how-can-i-update-font-awesome-version-on-the-template)
     - [How can I update Tabler Icons version](FAQ.md#how-can-i-update-tabler-icons-version-on-the-template)

## Removing content

Since this template has a lot of content, you may want to remove some of it.
The easiest way to achieve this and avoid merge conflicts when updating your code (as [pointed by CheariX ](https://github.com/alshedivat/al-folio/pull/2933#issuecomment-2571271117)) is to add the unwanted files to the `exclude` section in your `_config.yml` file instead of actually deleting them, for example:

```yml
exclude:
  - _news/announcement_*.md
  - _pages/blog.md
  - _posts/
  - _projects/?_project.md
  - assets/jupyter/blog.ipynb
```

Here is a list of the main components that you may want to delete, and how to do it.
Don't forget if you delete a page to update the `nav_order` of the remaining pages.


### You can also remove pages through commenting out front-matter blocks

For `.md` files in [\pages](_pages/) directory, if you do not want to completely edit or delete them but save for later use, you can temporarily disable these variables. But be aware that Jekyll only recognizes front matter when it appears as uncommented. The layout, permalink, and other front-matter behavior are disabled for that file.

For example, books.md do:

```md
<!-- ---
layout: book-shelf
title: bookshelf
permalink: /books/
nav: true
collection: books
--- -->

> What an astonishing thing a book is. It's a flat object made from a tree with flexible parts on which are imprinted lots of funny dark squiggles. But one glance at it and you're inside the mind of another person, maybe somebody dead for thousands of years. Across the millennia, an author is speaking clearly and silently inside your head, directly to you. Writing is perhaps the greatest of human inventions, binding together people who never knew each other, citizens of distant epochs. Books break the shackles of time. A book is proof that humans are capable of working magic.
>
> -- Carl Sagan, Cosmos, Part 11: The Persistence of Memory (1980)

## Books that I am reading, have read, or will read
```

### Removing the blog page

To remove the blog, you have to:

- delete [\_posts](_posts/) directory
- delete blog page [\_pages/blog.md](_pages/blog.md)
- remove reference to blog page in our [\_pages/dropdown.md](_pages/dropdown.md)
- remove the `latest_posts` part in [\_pages/about.md](_pages/about.md)
- remove the `Blog` section in the [\_config.yml](_config.yml) file and the related parts, like the `jekyll-archives`

You can also:

- delete [\_includes/latest_posts.liquid](_includes/latest_posts.liquid)
- delete [\_includes/related_posts.liquid](_includes/related_posts.liquid)
- delete [\_layouts/archive.liquid](_layouts/archive.liquid) (unless you have a custom collection that uses it)
- delete [\_plugins/external-posts.rb](_plugins/external-posts.rb)
- remove the `jekyll-archives-v2` gem from the [Gemfile](Gemfile) and the `plugins` section in [\_config.yml](_config.yml) (unless you have a custom collection that uses it)
- remove the `classifier-reborn` gem from the [Gemfile](Gemfile)

### Removing the news section

To remove the news section, you can:

- delete the [\_news](_news/) directory
- delete the file [\_includes/news.liquid](_includes/news.liquid) and the references to it in the [\_pages/about.md](_pages/about.md)
- remove the `announcements` part in [\_pages/about.md](_pages/about.md)
- remove the news part in the `Collections` section in the [\_config.yml](_config.yml) file

### Removing the projects page

To remove the projects, you can:

- delete the [\_projects](_projects/) directory
- delete the projects page [\_pages/projects.md](_pages/projects.md)
- remove reference to projects page in our [\_pages/dropdown.md](_pages/dropdown.md)
- remove projects part in the `Collections` section in the [\_config.yml](_config.yml) file

You can also:

- delete [\_includes/projects_horizontal.liquid](_includes/projects_horizontal.liquid)
- delete [\_includes/projects.liquid](_includes/projects.liquid)

### Removing the publications page

To remove the publications, you can:

- delete the [\_bibliography](_bibliography/) directory
- delete the publications page [\_pages/publications.md](_pages/publications.md)
- remove reference to publications page in our [\_pages/dropdown.md](_pages/dropdown.md)
- remove `Jekyll Scholar` section in the [\_config.yml](_config.yml) file

You can also:

- delete the [\_layouts/bib.liquid](_layouts/bib.liquid) file
- delete [\_includes/bib_search.liquid](_includes/bib_search.liquid)
- delete [\_includes/citation.liquid](_includes/citation.liquid)
- delete [\_includes/selected_papers.liquid](_includes/selected_papers.liquid)
- delete [\_plugins/google-scholar-citations.rb](_plugins/google-scholar-citations.rb)
- delete [\_plugins/hide-custom-bibtex.rb](_plugins/hide-custom-bibtex.rb)
- delete [\_plugins/inspirehep-citations.rb](_plugins/inspirehep-citations.rb)
- remove the `jekyll-scholar` gem from the [Gemfile](Gemfile) and the `plugins` section in [\_config.yml](_config.yml)

### Removing the repositories page

To remove the repositories, you can:

- delete the repositories page [\_pages/repositories.md](_pages/repositories.md)
- delete [\_includes/repository/](_includes/repository/) directory


## Customizing fonts, spacing, and more

You can customize the fonts, spacing, and more by editing [\_sass/\_base.scss](_sass/_base.scss). The easiest way to try in advance the changes is by using [chrome dev tools](https://developer.chrome.com/docs/devtools/css) or [firefox dev tools](https://firefox-source-docs.mozilla.org/devtools-user/). In there you can click in the element and find all the attributes that are set for that element and where are they. For more information on how to use this, check [chrome](https://developer.chrome.com/docs/devtools/css) and [firefox](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/examine_and_edit_css/index.html) how-tos, and [this tutorial](https://www.youtube.com/watch?v=l0sgiwJyEu4).

## Scheduled Posts

`al-folio` contains a workflow which automatically publishes all posts scheduled at a specific day, at the end of the day (23:30). By default the action is disabled, and to enable it you need to go to `.github/workflows/` and find the file called `schedule-posts.txt`. This is the workflow file. For GitHub to recognize it as one (or to enable the action), you need to rename it to `schedule-posts.yml`.

In order to use this you need to save all of your "Completed" blog posts which are scheduled to be uploaded on a specific date, in a folder named `_scheduled/` in the root directory.

> Incomplete posts should be saved in `_drafts/`

### Name Format

In this folder you need to store your file in the same format as you would in `_posts/`

> Example file name: `2024-08-26-This file will be uploaded on 26 August.md`

### Important Notes

- The scheduler uploads posts everyday at 🕛 23:30 UTC
- It will only upload posts at 23:30 UTC of their respective scheduled days, It's not uploaded in 23:59 in case there are a lot of files as the scheduler must finish before 00:00
- It will only upload files which follow the pattern `yyyy-mm-dd-title.md`
  - This means that only markdown files will be posted
  - It means that any markdown which do not follow this pattern will not be posted
- The scheduler works by moving posts from the `_scheduled/` directory to `_posts/`, it will not post to folders like `_projects/` or `_news/`
- The date in the name of the file is the day that file will be uploaded on
  - `2024-08-27-file1.md` will not be posted before or after 27-August-2024 (Scheduler only works for posts scheduled on the present day)
  - `2025-08-27-file2.md` will be posted exactly on 27-August-2025
  - `File3.md` will not be posted at all
  - `2026-02-31-file4.md` is supposed to be posted on 31-February-2026, but there is no 31st in February hence this file will never be posted either
