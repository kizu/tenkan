# Tenkan — a bunch of useful helpers for Jekyll-based sites

This repo consists of some useful _includes_, that you could use for your site or blog on Jekyll.

_However_, all those things are very hacky (as almost anything you could do without plugins in Jekyll), so be cautious, things can go wrong any moment ;)

## Features

- Made for gh-pages. All the things Tenkan contains are just some magic made in Liquid — no ruby plugins. That means that it would work everywhere the Jekyll is run with `--safe` flag, so it would perfectly work at GitHub Pages.

- Works perfectly as a git submodule: GitHub Pages allows you to use submodules in your repos, the only rule is to use `git://` link for them. So for include Tenkan as a submodule go to your repo and write in console:

        git submodule add git://github.com/kizu/tenkan.git _includes/tenkan

    Doing so would clone the Tenkan to your project, so you could use and update it without any hassle.

- Documented code: the includes are made in a form of markdown documents, so if you'd open them on GitHub, like [this one](get_title.md), you'll get nice readable documents. All the code parts are highlighted as `django`, 'cause it's the closest available pygment to the Liquid.

## Install & Use

1. Download or clone this repo into your `_include/tenkan` folder, or just use a git submodule:

        git submodule add git://github.com/kizu/tenkan.git _includes/tenkan

2. In every layout or include you'd want to use Tenkan include setup all the things using this:

    ``` django
{% capture tenkan_cache %}{% include tenkan/setup.md %}{% endcapture %}
```

    You need to capture the contents of the setup in _cache_, 'cause there are documentation things you won't want to have.

    Note that if you'd like to include it without using extra cache etc, you could create a `setup` file in your `_includes/` as on [this gist](https://gist.github.com/4702989), so then you'd need to write only `{% include setup %}` to include the Tenkan's setup.


3. Use any of the available features!

    (the descriptions of those is coming soon)

## Developing

If you've cloned the repo as a submodule and you'd want to contribute, then you'll need to update the push url to the non-read-only one. For this repo it would be

    git remote set-url --push origin git@github.com:kizu/tenkan.git

If you've forked it, then replace `kizu` with your username.

- - -

To be continued…
