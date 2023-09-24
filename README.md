
**To enable automatic deployment:**

1. Click on **Actions** tab and **Enable GitHub Actions**; do not worry about creating any workflows as everything has already been set for you.
2. Go to Settings -> Actions -> General -> Workflow permissions, and give **Read and write permissions** to GitHub Actions
3. Make any other changes to your webpage, commit, and push. This will automatically trigger the **Deploy** action.
4. Wait for a few minutes and let the action complete. You can see the progress in the **Actions** tab. If completed successfully, in addition to the `master` branch, your repository should now have a newly built `gh-pages` branch.
5. Finally, in the **Settings** of your repository, in the Pages section, set the branch to `gh-pages` (**NOT** to `master`). For more details, see [Configuring a publishing source for your GitHub Pages site](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#choosing-a-publishing-source).


**Note:** Jekyll will clean `$HOME/repo/publishing-source` before building!

The quote below is taken directly from the [jekyll configuration docs](https://jekyllrb.com/docs/configuration/options/):

> Destination folders are cleaned on site builds
>
> The contents of `<destination>` are automatically cleaned, by default, when the site is built. Files or folders that are not created by your site will be removed. Some files could be retained by specifying them within the `<keep_files>` configuration directive.
>
> Do not use an important location for `<destination>`; instead, use it as a staging area and copy files from there to your web server.

If `$HOME/repo/publishing-source` contains files that you want jekyll to leave untouched, specify them under `keep_files` in `_config.yml`.
In its default configuration, al-folio will copy the top-level `README.md` to the publishing source. If you want to change this behavior, add `README.md` under `exclude` in `_config.yml`.

**Note:** Do _not_ run `jekyll clean` on your publishing source repo as this will result in the entire directory getting deleted, irrespective of the content of `keep_files` in `_config.yml`.


---

#### Upgrading from a previous version

If you installed **al-folio** as described above, you can configure a [GitHub action](https://github.com/AndreasAugustin/actions-template-sync) to automatically sync your repository with the latest version of the theme.

Go to Settings -> Actions -> General -> Workflow permissions, give Read and write permissions to GitHub Actions, check "Allow GitHub Actions to create and approve pull requests", and save your changes.

Then go to Actions -> New workflow -> set up a workflow yourself, setup the following workflow and commit your changes:

```yaml
name: Sync from template
on:
    # cronjob trigger
  schedule:
  - cron:  "0 0 1 * *"
  # manual trigger
  workflow_dispatch:
jobs:
  repo-sync:
    runs-on: ubuntu-latest
    steps:
      # To use this repository's private action, you must check out the repository
      - name: Checkout
        uses: actions/checkout@v3
      - name: actions-template-sync
        uses: AndreasAugustin/actions-template-sync@v0.7.3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          source_repo_path: alshedivat/al-folio
          upstream_branch: master
```

You will receive a pull request within your repository if there are some changes available in the template.

Another option is to manually update your code by following the steps below:

```bash
# Assuming the current directory is <your-repo-name>
$ git remote add upstream https://github.com/alshedivat/al-folio.git
$ git fetch upstream
$ git rebase v0.9.0
```

If you have extensively customized a previous version, it might be trickier to upgrade.
You can still follow the steps above, but `git rebase` may result in merge conflicts that must be resolved.
See [git rebase manual](https://help.github.com/en/github/using-git/about-git-rebase) and how to [resolve conflicts](https://help.github.com/en/github/using-git/resolving-merge-conflicts-after-a-git-rebase) for more information.
If rebasing is too complicated, we recommend re-installing the new version of the theme from scratch and port over your content and changes from the previous version manually.

---

If you are using release `v0.3.5` or later, the website will automatically and correctly re-deploy right after your first commit.

Please make some changes (e.g., change your website info in `_config.yml`), commit, and push.


2. **Q:** I am using a custom domain (e.g., `foo.com`).
   My custom domain becomes blank in the repository settings after each deployment.
   How do I fix that? <br>
   **A:** You need to add `CNAME` file to the `master` or `source` branch of your repository.
   The file should contain your custom domain name.

3. **Q:** My webpage works locally.
    But after deploying, it fails to build and throws `Unknown tag 'toc'`.
    How do I fix that? <br>
   **A:** Make sure you followed through the [deployment instructions](#deployment) in the previous section.
   You should have set the deployment branch to `gh-pages`.
   (Related issue: [1438](https://github.com/alshedivat/al-folio/issues/1438).)

4. **Q:** My webpage works locally.
    But after deploying, it is not displayed correctly (CSS and JS is not loaded properly).
    How do I fix that? <br>
   **A:** Make sure to correctly specify the `url` and `baseurl` paths in `_config.yml`.
   Set `url` to `https://<your-github-username>.github.io` or to `https://<your.custom.domain>` if you are using a custom domain.
   If you are deploying a personal or organization website, leave `baseurl` blank.
   If you are deploying a project page, set `baseurl: /<your-project-name>/`.
   If all previous steps were done correctly, all is missing is
   [for your browser to fetch again the site stylesheet](https://github.com/alshedivat/al-folio/issues/1398#issuecomment-1609518404).

5. **Q:** Atom feed doesn't work. Why?
   <br>
   **A:** Make sure to correctly specify the `url` and `baseurl` paths in `_config.yml`.
  RSS Feed plugin works with these correctly set up fields: `title`, `url`, `description` and `author`.
  Make sure to fill them in an appropriate way and try again.

6. **Q:** My site doesn't work when I enable `related_blog_posts`. Why? <br>
   **A:** This is probably due to the [classifier reborn](https://github.com/jekyll/classifier-reborn) plugin, which is used to calculate
   related posts. If the error states `Liquid Exception: Zero vectors can not be normalized...`, it means that it could not calculate related
   posts for a specific post. This is usually caused by [empty or minimal blog posts](https://github.com/jekyll/classifier-reborn/issues/64)
   without meaningful words (i.e. only [stop words](https://en.wikipedia.org/wiki/Stop_words)) or even
   [specific characters](https://github.com/jekyll/classifier-reborn/issues/194) you used in your posts. Also, the calculus for similar posts are
   made for every `post`, which means every page that uses `layout: post`, including the announcements. To change this behavior, simply add
   `related_posts: false` to the front matter of the page you don't want to display related posts on.

## Features

### Publications

Your publications' page is generated automatically from your BibTex bibliography.
Simply edit `_bibliography/papers.bib`.
You can also add new `*.bib` files and customize the look of your publications however you like by editing `_pages/publications.md`.

<p align="center"><img src="https://raw.githubusercontent.com/alshedivat/al-folio/master/assets/img/publications-screenshot.png" width=800></p>

<details><summary>(click to expand) <strong>Author annotation:</strong></summary>

In publications, the author entry for yourself is identified by string array `scholar:last_name` and string array `scholar:first_name` in `_config.yml`:

```yaml
scholar:
  last_name: [Einstein]
  first_name: [Albert, A.]
```

If the entry matches one form of the last names and the first names, it will be underlined.
Keep meta-information about your co-authors in `_data/coauthors.yml` and Jekyll will insert links to their webpages automatically.
The co-author data format in `_data/coauthors.yml` is as follows,

```yaml
"Adams":
  - firstname: ["Edwin", "E.", "E. P.", "Edwin Plimpton"]
    url: https://en.wikipedia.org/wiki/Edwin_Plimpton_Adams

"Podolsky":
  - firstname: ["Boris", "B.", "B. Y.", "Boris Yakovlevich"]
    url: https://en.wikipedia.org/wiki/Boris_Podolsky

"Rosen":
  - firstname: ["Nathan", "N."]
    url: https://en.wikipedia.org/wiki/Nathan_Rosen

"Bach":
  - firstname: ["Johann Sebastian", "J. S."]
    url: https://en.wikipedia.org/wiki/Johann_Sebastian_Bach

  - firstname: ["Carl Philipp Emanuel", "C. P. E."]
    url: https://en.wikipedia.org/wiki/Carl_Philipp_Emanuel_Bach
```

If the entry matches one of the combinations of the last names and the first names, it will be highlighted and linked to the url provided.

</details>

<details><summary>(click to expand) <strong>Buttons (through custom bibtex keywords):</strong></summary>

There are several custom bibtex keywords that you can use to affect how the entries are displayed on the webpage:

- `abbr`: Adds an abbreviation to the left of the entry. You can add links to these by creating a venue.yaml-file in the _data folder and adding entries that match.
- `abstract`: Adds an "Abs" button that expands a hidden text field when clicked to show the abstract text
- `arxiv`: Adds a link to the Arxiv website (Note: only add the arxiv identifier here - the link is generated automatically)
- `bibtex_show`: Adds a "Bib" button that expands a hidden text field with the full bibliography entry
- `html`: Inserts an "HTML" button redirecting to the user-specified link
- `pdf`: Adds a "PDF" button redirecting to a specified file (if a full link is not specified, the file will be assumed to be placed in the /assets/pdf/ directory)
- `supp`: Adds a "Supp" button to a specified file (if a full link is not specified, the file will be assumed to be placed in the /assets/pdf/ directory)
- `blog`: Adds a "Blog" button redirecting to the specified link
- `code`: Adds a "Code" button redirecting to the specified link
- `poster`: Adds a "Poster" button redirecting to a specified file (if a full link is not specified, the file will be assumed to be placed in the /assets/pdf/ directory)
- `slides`: Adds a "Slides" button redirecting to a specified file (if a full link is not specified, the file will be assumed to be placed in the /assets/pdf/ directory)
- `website`: Adds a "Website" button redirecting to the specified link
- `altmetric`: Adds an [Altmetric](https://www.altmetric.com/) badge (Note: if DOI is provided just use `true`, otherwise only add the altmetric identifier here - the link is generated automatically)
- `dimensions`: Adds a [Dimensions](https://www.dimensions.ai/) badge (Note: if DOI or PMID is provided just use `true`, otherwise only add the Dimensions' identifier here - the link is generated automatically)

You can implement your own buttons by editing the bib.html file.

</details>

---

### Collections

This Jekyll theme implements `collections` to let you break up your work into categories.
The theme comes with two default collections: `news` and `projects`.
Items from the `news` collection are automatically displayed on the home page.
Items from the `projects` collection are displayed on a responsive grid on projects page.

<p align="center"><img src="https://raw.githubusercontent.com/alshedivat/al-folio/master/assets/img/projects-screenshot.png" width=700></p>

You can easily create your own collections, apps, short stories, courses, or whatever your creative work is.
To do this, edit the collections in the `_config.yml` file, create a corresponding folder, and create a landing page for your collection, similar to `_pages/projects.md`.

---

### Layouts

**al-folio** comes with stylish layouts for pages and blog posts.

#### The iconic style of Distill

The theme allows you to create blog posts in the [distill.pub](https://distill.pub/) style:

<p align="center"><a href="https://alshedivat.github.io/al-folio/blog/2021/distill/" target="_blank"><img src="https://raw.githubusercontent.com/alshedivat/al-folio/master/assets/img/distill-screenshot.png" width=700></a></p>

For more details on how to create distill-styled posts using `<d-*>` tags, please refer to [the example](https://alshedivat.github.io/al-folio/blog/2021/distill/).

#### Full support for math & code

**al-folio** supports fast math typesetting through [MathJax](https://www.mathjax.org/) and code syntax highlighting using [GitHub style](https://github.com/jwarby/jekyll-pygments-themes):

<p align="center">
<a href="https://alshedivat.github.io/al-folio/blog/2015/math/" target="_blank"><img src="https://raw.githubusercontent.com/alshedivat/al-folio/master/assets/img/math-screenshot.png" width=400></a>
<a href="https://alshedivat.github.io/al-folio/blog/2015/code/" target="_blank"><img src="https://raw.githubusercontent.com/alshedivat/al-folio/master/assets/img/code-screenshot.png" width=400></a>
</p>

#### Photos

Photo formatting is made simple using [Bootstrap's grid system](https://getbootstrap.com/docs/4.4/layout/grid/).
Easily create beautiful grids within your blog posts and project pages:

<p align="center">
  <a href="https://alshedivat.github.io/al-folio/projects/1_project/">
    <img src="https://raw.githubusercontent.com/alshedivat/al-folio/master/assets/img/photos-screenshot.png" width="75%">
  </a>
</p>

---

### Other features

#### GitHub's repositories and user stats

**al-folio** uses [github-readme-stats](https://github.com/anuraghazra/github-readme-stats) and [github-profile-trophy](https://github.com/ryo-ma/github-profile-trophy)
to display GitHub repositories and user stats on the `/repositories/` page.

Edit the `_data/repositories.yml` and change the `github_users` and `github_repos` lists to include your own GitHub profile and repositories to the `/repositories/` page.

You may also use the following codes for displaying this in any other pages.

```html
<!-- code for GitHub users -->
{% if site.data.repositories.github_users %}
<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for user in site.data.repositories.github_users %}
    {% include repository/repo_user.html username=user %}
  {% endfor %}
</div>
{% endif %}

<!-- code for GitHub trophies -->
{% if site.repo_trophies.enabled %}
{% for user in site.data.repositories.github_users %}
  {% if site.data.repositories.github_users.size > 1 %}
  <h4>{{ user }}</h4>
  {% endif %}
  <div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% include repository/repo_trophies.html username=user %}
  </div>
{% endfor %}
{% endif %}

<!-- code for GitHub repositories -->
{% if site.data.repositories.github_repos %}
<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for repo in site.data.repositories.github_repos %}
    {% include repository/repo.html repository=repo %}
  {% endfor %}
</div>
{% endif %}
```

#### Theming

A variety of beautiful theme colors have been selected for you to choose from.
The default is purple, but you can quickly change it by editing the
`--global-theme-color` variable in the `_sass/_themes.scss` file.
Other color variables are listed there as well.
The stock theme color options available can be found at `_sass/_variables.scss`.
You can also add your own colors to this file assigning each a name for ease of
use across the template.

#### Social media previews

**al-folio** supports preview images on social media.
To enable this functionality you will need to set `serve_og_meta` to `true` in your `_config.yml`.
Once you have done so, all your site's pages will include Open Graph data in the HTML head element.

You will then need to configure what image to display in your site's social media previews.
This can be configured on a per-page basis, by setting the `og_image` page variable.
If for an individual page this variable is not set, then the theme will fall back to a site-wide `og_image` variable, configurable in your `_config.yml`.
In both the page-specific and site-wide cases, the `og_image` variable needs to hold the URL for the image you wish to display in social media previews.

#### Atom (RSS-like) Feed

It generates an Atom (RSS-like) feed of your posts, useful for Atom and RSS readers.
The feed is reachable simply by typing after your homepage `/feed.xml`.
E.g. assuming your website mountpoint is the main folder, you can type `yourusername.github.io/feed.xml`

#### Related posts

By default, there will be a related posts section on the bottom of the blog posts.
These are generated by selecting the `max_related` most recent posts that share at least `min_common_tags` tags with the current post.
If you do not want to display related posts on a specific post, simply add `related_posts: false` to the front matter of the post.
If you want to disable it for all posts, simply set `enabled` to false in the `related_blog_posts` section in `_config.yml`.

