Global Names Docs and Blog
==========================

This project contains news and documents about Global Names Architecture -- an
international effort to improve handling scientific names for biodiversity
informatics, medicine and biology.

You can visit [globalnames.org] to see this site on the web.


Adding a New Post
-----------------

```bash
git clone git@github.com:GlobalNamesArchitecture/GlobalNamesArchitecture.github.io.git gn
cd gn
git checkout jekyll
bundle install
cd _posts
```

then copy a post that it the closest to your topic, rename it according to
the current date and its title, edit the content. Then

```bash
git add .
git commit -m 'your message'
git push
bundle exec jekyll build
git checkout master
cp -r _site/* .
git add .
git commit -m 'your message'
git push
```
The site should be updated at [globalnames.org] shortly.

[globalnames.org]: http://globalnames.org
