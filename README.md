# tenkan — a bunch of useful helpers for Jekyll-based sites

This repo consists of some useful _includes_, that you could use for your site or blog on Jekyll.

## Features

- Made for gh-pages. All the things tenk	an contains are just some magic made in Liquid — no ruby plugins. That means that it would work everywhere the Jekyll is run with `--safe`	flag, so it would perfectly work at GitHub Pages.

- Works perfectly as a git submodule: GitHub Pages allows you to use submodules in your repos, the only rule is to use `git://` link for them. So for include tenkan as a submodule go to your repo and write in console:

        git submodule add git://github.com/kizu/tenkan.git _includes/tenkan

    Doing so would clone the tenkan to your project, so you could use and update it without any hassle.

- - -

To be continued…