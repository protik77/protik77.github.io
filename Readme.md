This is the content of my [homepage](https://protikdas.com).
It's rendered via [Jekyll](https://jekyllrb.com) and hosted on [Cloudflare Pages](https://pages.cloudflare.com).
The skeleton of this site is taken from source code of [nickcraver.com](https://github.com/NickCraver/nickcraver.github.com) with permission.

See a problem with my site? Please leave a comment, open an issue, or just submit a pull request.

## Local development

The project targets Ruby 3.4 (see `.ruby-version`).
With a matching Ruby and [Bundler](https://bundler.io) installed:

```sh
bundle install
bundle exec jekyll serve
```

The site will be available at http://localhost:4000.

## Cloudflare Pages deployment

The build is configured for Cloudflare Pages.
`wrangler.toml` declares the build output directory (`_site`).
The remaining build settings are set in the Cloudflare dashboard or via the Git integration:

| Setting              | Value                       |
| -------------------- | --------------------------- |
| Build command        | `bundle exec jekyll build`  |
| Build output dir     | `_site`                     |
| Environment variable | `RUBY_VERSION = 3.4.4`      |

`Gemfile.lock` is committed so Cloudflare builds are reproducible.
