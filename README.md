sensorberg-dev.github.io
========================

Documentation for the Sensorberg SDKs.

[![Build Status](https://travis-ci.org/sensorberg-dev/sensorberg-dev.github.io.svg?branch=master)](https://travis-ci.org/sensorberg-dev/sensorberg-dev.github.io)

If you want to see your drafts:

```
jekyll serve --drafts
```
The drafts will not be visible on the actual website.

If you want to check all links on your computer run these commands:

```
jekyll build --drafts
htmlproof ./_site
```

You need to install ```jekyll``` and ```html-proofer``` once on your computer to enable this workflow. Jekyll should *NOT* run when you run this test!

```
sudo gem install jekyll
sudo gem install html-proofer
```
