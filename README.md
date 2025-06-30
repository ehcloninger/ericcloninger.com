# ericcloninger.com

Uses the [Minimal Mistakes Jekyll theme](https://github.com/mmistakes/minimal-mistakes).

## Building

On WSL, issue these commands to get the environment up and running

```
sudo apt install make ruby ruby-dev build-essentials openssl
gem install bundler jekyll
bundle install
```

To build the site, issue

```
bundle exec jekyll build --config _config.yml
```

To build local and serve up as localhost, use

```
bundle exec jekyll serve --config _config-dev.yml --watch --incremental --force_polling
```

To view, enter [localhost:4000](http://localhost:4000) in your browser.