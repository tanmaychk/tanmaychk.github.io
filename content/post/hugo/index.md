+++
authors = ["tanmay"]
categories = ["WebDev"]
date = 2022-06-16T18:30:00Z
featured = true
gallery_item = []
lastmod = ""
projects = []
subtitle = "with GitHub Actions to automatically publish site on each check-in"
summary = "A step-by-step tutorial to create a static Resume and Portfolio website using Hugo with free hosting on GitHub pages."
tags = ["Workflow", "CI/CD", "GitHub", "Hugo"]
title = "How I created my Resume/Portfolio website using Hugo and GitHub pages?"
[image]
caption = "Photo by Kaitlyn Baker on Unsplash"
focal_point = "smart"
preview_only = false

+++
Honestly, you might be coming across umpteenth article on this topic, but I had to jot down my experiments with static website setup. For the uninitiated, apart from being the name of a famous 2011 [Martin Scorsese movie](https://en.wikipedia.org/wiki/Hugo_(film)), [Hugo](https://gohugo.io/) is also an extremely popular static website generator. Hugo uses data files, configuration, layout templates, static files, and content written in [Markdown](https://en.wikipedia.org/wiki/Markdown "Markdown") to render a static website, which means no database, blazing fast site speed and no danger of cross-site scripting and SQL injection attacks.

## Installation & Prerequisites

To start with I installed Hugo on my Windows 10 laptop (instructions here), I already had [Git Bash](https://git-scm.com/download/win) installed, but it’s easy to install it from here. You may also choose to use a Git GUI tool like [Tortoise Git](https://tortoisegit.org/). Next, I created a new Git repository at my [GitHub](https://github.com/) account with the name _\[my-github-username\].github.io_ containing just a blank README.md file (note that its necessary to name the repo this way, in order to leverage the [GitHub pages](https://pages.github.com/) feature).

While GitHub will create a “_main_” code branch by default, at this time I also created an empty new branch and named it “_gh-pages_” (following sections will talk about the significance of creating this other code branch).

Next, we need to tell GitHub that our repository will use GitHub Pages. To do that, click the "Gear Icon > Settings" for the repo, and then click "_Pages_" on the left menu. Then, under "_Source_" on the right pane, select the "_Branch_" as "_gh-pages_". This tells GitHub that the website would be rendered using the content in the _gh-pages_ branch (and not the _main_ branch). You might also configure a custom domain for the website, which is not something I did.

All set? Let’s move on.

## Hugo Site and Theme

Every Hugo site has an associated theme to apply the site style and look. I decided to use the freely available [Academia Hugo](https://github.com/themefisher/academia-hugo) theme, and since I might need to make changes to the theme files I decided to [fork](https://github.com/tanmaychk/academia-hugo) their Git repo to my GitHub account.

To begin building my site, I first clone my GitHub pages repo locally, as follows:

```bash
git clone https://github.com/tanmaychk/tanmaychk.github.io
cd tanmaychk.github.io
```

I will add the Hugo theme to my site as a Git submodule, this will help me pull-in the theme (which is a separate Git repo) changes easily. This is especially useful if you are using a theme from a Git repo we don't own.

```bash
git submodule add https://github.com/tanmaychk/academia-hugo.git themes/academia-hugo
```

The Academia Hugo theme thankfully comes with a core set of files needed to create a Hugo site from the theme in a folder "_exampleSite_", so I recursively copy this folder's entire content to my Hugo site root, as follows:

```bash
 cp -r themes/academia-hugo/exampleSite/* . 
```

Now the basic structure of my website is ready, and I can immediately test it using the local Hugo server (yes, without even deploying my site yet), as follows:

```bash
hugo server -b http://localhost:1313
```

The site will now be accessible at http://localhost:1313 (your port could vary, if you supplied it differently). The parameter _-b_ is to specify the _baseURL_ for the site because as of now I haven't touched the Hugo _config.toml_ file, which contains all the website configurations. I could have also added the _baseURL_ to _config/_default/config.toml_ file directly, in lieu of specifying this parameter.

Academia theme has in-built support to add details about Courses, Projects, Skills, and Blogs. I changed the content (text as well as images) according to my needs and kept checking it locally. Images were added under the _static/img_ folder. I also added my resume PDF under _static/files_ folder and added the link to the PDF via the  _config/_default/menus.toml_ config file.

## Generating the website

Hugo can help generate the HTML and static files that we would deploy to our website (remember raw HTML are the crux if the static website thingy). You can deploy them using online CMS tools like Netlify and Forestry.io, after generating the public website through the following simple command:

```bash
hugo
```

I see a new folder _public_ now in my Hugo site folder that contains all the generated HTML files and static assets which can be straightway deployed to any hosting platform.

Since this article is about deploying the site to GitHub pages, I will now focus on that ask. At this point I can safely delete the complete _public_ folder.

## Deploying to GitHub pages

Before I start, a quick recall about the two code branches I created when setting up my repo, the default "_main_" and a "_gh-pages_" branch. I will now explain why I did that. While the "main" code branch will store all our core "source" files (that I created in above-said steps while creating my Hugo site), the "_gh-pages_" will contain the content that the _public_ folder in the previous paragraph contained, i.e. the entire generated site. But I will cleverly hand over the job of creating this generated website to GitHub via Actions.

GitHub Actions is a continuous integration and continuous delivery (CI/CD) platform that allows us to automate our build, test, and deployment pipeline.

In my case I create a workflow that will

1. get triggered on code check-in to the "_main_" branch
2. check-out our "_main_" branch code, including the theme submodule
3. perform a setup of Hugo (latest version)
4. generate the static website in "_public_" folder, using the "_hugo_" command
5. check-in the content of the "_public_" folder to the "_gh-pages_" branch of the repository

To create the workflow, click "Actions" link on the repository page, then click "New Workflow" and paste the YAML content provided below. Then save the workflow.

![GitHub Actions Workflow](post/gh-actions.jpg "GitHub Actions Workflow")

Interesting to note that GitHub stores this workflow in the "main" branch under _.github/workflows_ folder. For more details on this Action please refer the documentation and code at https://github.com/peaceiris/actions-gh-pages.

```yaml
name: GitHub Pages

on:
  push:
    branches:
      - main  # Set a branch name to trigger deployment
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    permissions:
      contents: write
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: ${{ github.ref == 'refs/heads/main' }}
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_branch: gh-pages
          keep_files: true
          publish_dir: ./public
```

Now it's time to check-in the code from our local laptop to the Github pages repo "_tanmaychk.github.io_". As soon as the code is pushed to GitHub, the "GitHub Pages" actions workflow gets triggered and in couple of minutes our website is published to the _gh-pages_ branch. Now the site can be viewed at https://tanmaychk.github.io/.