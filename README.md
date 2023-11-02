### Recreate website in dev environment

- Build jekyll bundle

```
bundle install
```

- Run jekyll site

```
bundle exec jekyll serve
```

### Hosting static website on file hosting server

- Add following configuration keys to `_config.yml`
    - `url: https://www.cs.toronto.edu`
    - `baseurl: ~edithal`

- Run `bundle exec jekyll build`

- Copy contents under `_site` folder into the `public_html` root folder of your file hosting server