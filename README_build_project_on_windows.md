# How to start with Jekyll and how to build the project in Windows

Create a new repo in gitlab and use the template for just the docs theme: https://github.com/just-the-docs/just-the-docs-template

Remove aux_links in _config.yml if we don't want a link to the repo
Add an empty _includes/nav_footer_cutom.html to remove the reference to just the docs on the menu

Add your pages content in md formart in root directory or in /docs

Follow the guide to install Ruby and Jekyll on Windows: https://jekyllrb.com/docs/installation/windows/
	1.Download and install a Ruby+Devkit version from RubyInstaller Downloads (https://rubyinstaller.org/downloads/). Use default options for installation.
	2.Run the ridk install step on the last stage of the installation wizard. 
		This is needed for installing gems with native extensions. 
		You can find additional information regarding this in the RubyInstaller Documentation. 
		From the options choose MSYS2 and MINGW development toolchain.
	3.Open a new command prompt window from the start menu, so that changes to the PATH environment variable becomes effective. Install Jekyll and Bundler using: gem install jekyll bundler
	4.Check if Jekyll has been installed properly: jekyll -v


Build the project and make it available on localhost:
cd myblog

bundle exec jekyll serve


If it is missing some gem:
bundle install

It will be accessible at:
http://localhost:4000

To rebuild automatically when there is a change:
bundle exec jekyll serve --incremental


If something is not built properly, remove the _site directory and build again