Salesforce workshop for Java Enterprise developers
==================================================


This is a Salesforce workshop for Enterprise Java developers.  It covers how to build a simple Java Spring MVC application and deploy it on Heroku using the Heroku plugin for Eclipse IDE.  The workshop also includes integration with forcedotcom and using addons with heroku.

The workshop is written in Markdown and deployed on Heroku cloud platform using a custom build pack by James Ward.  The build pack is located at [https://github.com/jamesward/heroku-buildpack-markdown](https://github.com/jamesward/heroku-buildpack-markdown)

PDF documents are generated using a project called gimli,[https://github.com/walle/gimli](https://github.com/walle/gimli).  This project requires a working copy of Ruby installed.  To generate a new pdf document of the workshop, simply call gimli with the specific input file (-f) and style file (-s):

gimli -f index.md -s index.css


TODO: Add gimli as a gem to heroku and modify the build pack to call gimli each time a new `git push` deploy happens on heroku.
