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

### Deployment to github pages

- Test in dev environment

- Commit and push to dev branch

- Go to github.com and create a PR to merge the changes to gh-pages branch

- Rebase and merge the PR

- Create a new release on the github repo

### Deployment to file hosting server

- First test and then deploy using this command from the root directory of the project

```
rsync -av _site comps0.cs.toronto.edu:/u/edithal/public_html
```