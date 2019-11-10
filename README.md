# blog.translusion.com

Erik Engheim's homepage build with Hugo static site generator deployed on
github-pages.

## How to build site

Download the standard Hugo themes:

    $ git clone --recursive https://github.com/spf13/hugoThemes themes
  
Make sure you have [Hugo]() installed then just run

    $ hugo
    
from the commandline to build the site. The output will be placed in the `public` folder.

## Adding posts and publishing them

The way you use this is: inside the root directory of this project you put a symbolic link `public` to a separate git repostory which you push to a server for being published. So whenever you have added a post you

	$ git commit -am "just added another post"
	
to store it. Then you

	$ cd public
	$ git commit -am "new post added for publishing online"
	$ git push origin
	

