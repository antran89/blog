# Use github for static web hosting

When making Github website.
https://help.github.com/articles/creating-project-pages-manually/


## make gh-pages branch
git checkout --orphan gh-pages

## push to the branch
git push origin gh-pages

## _config.yml
Set url to be nothing or http://0.0.0.0:4000


## Install and compile
bundle install
bundle exec jekyll build
bundle exec jekyll serve --watch# Installation


# Run the jekyll
Read through `Getting Started` from the original author

http://mmistakes.github.io/skinny-bones-jekyll/getting-started/

Need to install Gems

    bundle install


## Running and Testing 

Run jekyll

    bundle exec jekyll build
    bundle exec jekyll serve --watch


## New Post

Octopress to add new content

    get install octopress --pre

    octopress new post "Post Title With Space!"
    octopress new post "Portfolio Post Title" --dir portfolio --template media

## TODO

index page
research and projects

research, the point cloud converter
