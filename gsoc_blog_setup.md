Once you have been accepted as a GSoC student NumFOCUS expects you to blog about your progress every week. It's okay if you are stuck, post about that too :) You could use various platforms like wordpress, blogger, etc. but in our experience it's usually a pain to get formatting right in case of code snippets, screenshots. We suggest you use GitHub Pages and Jekyll to blog about your summer. 
GitHub gives you free hosting and a free domain name (`github_username.github.io`) and [Jekyll](https://jekyllrb.com) is a blog friendly static site generator. This tutorial will help you setup your blog.

#### Install Ruby
For Debian or Ubuntu based users
```
$ sudo apt-get install ruby-full
```
Mac OS/OS X has ruby installed by default.

For more information go through https://www.ruby-lang.org/en/documentation/installation/

#### Install RVM and setup 
```
$ \curl -sSL https://get.rvm.io | bash -s stable
 ```
for more information https://rvm.io

```
$ rvm install ruby  #installs the latest version
$ rvm gemset create blog     #creates a new gemset (ruby environment)
$ rvm gemset use blog       #change to blog gemset
$ gem install bundler jekyll
```
#### Create the blog site

```
$ jekyll new gsoc_blog
$ cd gsoc_blog
$ bundle exec jekyll serve
```

You should have a local server running on port 4000 (http://127.0.0.1:4000/) with a sample post on the blog. This uses the default theme, there are various others themes available feel free to hack around.

#### Integrate with GitHub Pages

Initialize a git repository inside the gsoc_blog folder.
```
$ git init .      # make sure you are inside the gsoc_blog folder
```
`git status` should give you the output something like
```
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	.gitignore
	Gemfile
	Gemfile.lock
	_config.yml
	_posts/
	about.md
	index.md

nothing added to commit but untracked files present (use "git add" to track)
```
Make this as your first commit
```
$ git add .
$ git commit -m 'Initial commit'
```

Now go to GitHub and create a new repository and name it `github_username.github.io`. **Don't** add readme or license files. If you are already using GitHub pages for a personal website create a new repository and name it `gsoc_blog`. Make sure to turn on GitHub pages in settings for the repository. You can access the blog at `github_username.github.io/gsoc_blog`

Add remote link to the GitHub repository on your machine
```
$ git remote add origin https://github.com/github_username/github_username.github.io.git
```
or if you already are using `github_username.github.io`
```
$ git remote add origin https://github.com/github_username/gsoc_blog.git
```
Push the changes (Intial commit) to GitHub
```
$ git push -u origin master
```

Have a look at https://mriduls.github.io/blog/

#### Adding a new post

Once we are done with the initial setup, let's write our first blog post.
```
$ cd _posts
$ touch YEAR-MONTH-DAY-title.markdown   #creates a new file
```

All blog post files must begin with [YAML Front Matter](https://jekyllrb.com/docs/frontmatter/).
Open your favorite text editor and start writing your first blog post :)

```
---
layout: post
title:  "Accepted to GSoC"
date:   2017-03-03 16:00 +0530
categories: jekyll update
---

Hello :)
This is my first blog post. Excited for working with NumFOCUS this summer as a GSoC student
```

for more information https://jekyllrb.com/docs/posts/#creating-post-files

Now we need to push these changes to GitHub.
```
$ git add .
$ git commit -m 'Accepted for GSoC 2017 with NumFOCUS'
$ git push -u origin master
```

Your blog `github_username.github.io` will update to show your new post. (This could take a couple of minutes)

have a look at https://mriduls.github.io/blog/jekyll/update/2017/03/03/example-post.html

There is a lot of customisation available in Jekyll, go through the docs https://jekyllrb.com/docs/home/












