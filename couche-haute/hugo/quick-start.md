# 🇬🇧 Quick Start

## Step 1: Install Hugo  <a href="#step-1-install-hugo" id="step-1-install-hugo"></a>

```
brew install hugo
# or
port install hugo
```

To verify your new install:

## Step 2: Create a New Site  <a href="#step-2-create-a-new-site" id="step-2-create-a-new-site"></a>

The above will create a new Hugo site in a folder named `quickstart`.

## Step 3: Add a Theme  <a href="#step-3-add-a-theme" id="step-3-add-a-theme"></a>

See [themes.gohugo.io](https://themes.gohugo.io/) for a list of themes to consider. This quickstart uses the beautiful [Ananke theme](https://themes.gohugo.io/gohugo-theme-ananke/).

First, download the theme from GitHub and add it to your site’s `themes` directory:

```
cd quickstart
git init
git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
```

_Note for non-git users:_

* If you do not have git installed, you can download the archive of the latest version of this theme from: [https://github.com/budparr/gohugo-theme-ananke/archive/master.zip](https://github.com/budparr/gohugo-theme-ananke/archive/master.zip)
* Extract that .zip file to get a “gohugo-theme-ananke-master” directory.
* Rename that directory to “ananke”, and move it into the “themes/” directory.

Then, add the theme to the site configuration:

```
echo 'theme = "ananke"' >> config.toml
```

## Step 4: Add Some Content  <a href="#step-4-add-some-content" id="step-4-add-some-content"></a>

You can manually create content files (for example as `content//.`) and provide metadata in them, however you can use the `new` command to do a few things for you (like add title and date):

```
hugo new posts/my-first-post.md
```

Edit the newly created content file if you want, it will start with something like this:

```
---
title: "My First Post"
date: 2019-03-26T08:47:11+01:00
draft: true
---

```

## Step 5: Start the Hugo server  <a href="#step-5-start-the-hugo-server" id="step-5-start-the-hugo-server"></a>

Now, start the Hugo server with [drafts](https://gohugo.io/getting-started/usage/#draft-future-and-expired-content) enabled:

```
▶ hugo server -D

                   | EN
+------------------+----+
  Pages            | 10
  Paginator pages  |  0
  Non-page files   |  0
  Static files     |  3
  Processed images |  0
  Aliases          |  1
  Sitemaps         |  1
  Cleaned          |  0

Total in 11 ms
Watching for changes in /Users/bep/quickstart/{content,data,layouts,static,themes}
Watching for config changes in /Users/bep/quickstart/config.toml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

**Navigate to your new site at** [**http://localhost:1313/**](http://localhost:1313/)**.**

Feel free to edit or add new content and simply refresh in browser to see changes quickly (You might need to force refresh in webbrowser, something like Ctrl-R usually works).

## Step 6: Customize the Theme  <a href="#step-6-customize-the-theme" id="step-6-customize-the-theme"></a>

Your new site already looks great, but you will want to tweak it a little before you release it to the public.

### Site Configuration  <a href="#site-configuration" id="site-configuration"></a>

Open up `config.toml` in a text editor:

```
baseURL = "https://example.org/"
languageCode = "en-us"
title = "My New Hugo Site"
theme = "ananke"
```

Replace the `title` above with something more personal. Also, if you already have a domain ready, set the `baseURL`. Note that this value is not needed when running the local development server.

For theme specific configuration options, see the [theme site](https://github.com/budparr/gohugo-theme-ananke).

**For further theme customization, see** [**Customize a Theme**](https://gohugo.io/themes/customizing/)**.**

### Step 7: Build static pages  <a href="#step-7-build-static-pages" id="step-7-build-static-pages"></a>

It is simple. Just call:

```
hugo -D
```

Output will be in `./public/` directory by default (`-d`/`--destination` flag to change it, or set `publishdir` in the config file).\


## Résumé des commandes sous linux

(commandes et test par @Léolios)

```bash
# installation du binaire écrit en Go
snap install hugo --channel=extended
sudo apt-get install hugo

# création d'un nouveau site
hugo new site bluekeys

# initialisation de git et ajout d'un sous module
git init
git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke

# configuration du thème par rapport au sous module ajouté
echo 'theme = "ananke"' >> config.toml

# création d'un nouvelle article
hugo new posts/my-first-post.md

# exécution en local du site pour tester
hugo server -D

# génération du site statique
# Ou tu génére et tu vas dans le dossier public 
hugo -D
```

Thème dispo : [https://themes.gohugo.io/theme](https://themes.gohugo.io/theme/hugo-theme-introduction/)

### See Also

* [External Learning Resources](https://gohugo.io/getting-started/external-learning-resources/)
* [Use Hugo Modules](https://gohugo.io/hugo-modules/use-modules/)
* [Basic Usage](https://gohugo.io/getting-started/usage/)
