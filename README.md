# Dart Website (dartlang.org)

[![Build Status](https://drone.io/github.com/dart-lang/dartlang.org/status.png)](https://drone.io/github.com/dart-lang/dartlang.org/latest)

The www.dartlang.org site. Built with [Jekyll](https://github.com/mojombo/jekyll)
and hosted on App Engine.

To give us feedback, please
[file issues on GitHub](https://github.com/dart-lang/dartlang.org/issues)

Contributions welcome!
(Just sign our [CLA](https://developers.google.com/open-source/cla/individual).)
Details on processes, formatting, and style are in
[Writing for dartlang.org](https://github.com/dart-lang/dartlang.org/wiki/Writing-for-dartlang.org).
You can fork and submit patches at https://github.com/dart-lang/dartlang.org.

## Orientation

<dl>
  <dt> ./src </dt>
  <dd> Contains all the working files. </dd>

  <dt> ./src/appengine </dt>
  <dd> Contains app engine configuration files. </dd>

  <dt> ./src/site </dt>
  <dd> Documents, HTML files, images, etc.
  You will work out of here normally. </dd>

  <dt> ./build </dt>
  <dd> Generated by Jekyll, to be deployed to server.
  This directory is transient, can be deleted. </dd>
</dl>


## Configuring your system

* Ensure you have Ruby 1.9.3 or 2.0.
* Ensure you have Python 2.7.
  * On a Mac? You might want the binary install of
    [Python 2.7.3](http://www.python.org/download/releases/2.7.3/)
* In a terminal, from the dartlang.org project root:
  1. Run `sudo gem install fast-stemmer -v '1.0.2'`
  2. Run `sudo gem install bundler`
  3. Run `sudo bundle install`, which installs the gems listed in `Gemfile`
    (liquid, jekyll, etc.).
    * If you see errors like
      `library X (at master) is not checked out. Please run 'bundle install'`,
      then run `bundle install` without `sudo`.
* Download and install the
  [App Engine launcher](https://developers.google.com/appengine/downloads)
  * Ensure App Engine is using Python 2.7. You will see "you're using 2.6" in
    the log if it is not.
    * If you are developing on a Mac, go to Preferences, and enter the direct
      path to the Python 2.7 binary, which you downloaded from
      [python.org](http://www.python.org/download/releases/2.7.3/).
      The full path is `/Library/Frameworks/Python.framework/Versions/2.7/bin/python2.7`.
* Ask an admin to invite you to modify the Dart project on the Google App Engine.


### Tips for Windows

* Install Python with the [Windows installer](https://www.python.org/download/windows/).
* Install Ruby with the [RubyInstaller site](http://rubyinstaller.org/downloads/).
* Install the Ruby DevKit from the [RubyInstaller site](http://rubyinstaller.org/downloads/)
* Run `gem install bundler`.
* Run `bundle install` from the root of your dartlang project.


### GitHub setup

* Create a [GitHub login](https://github.com/join) login if you don't already have one.
* Ask an admin to invite you to the dart-lang project on GitHub.


### Contributing via Chromium Code Review

On a Mac:
* Make sure you have Xcode (contains git)
* Install depot_tools:
  $ git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
* Add depot_tools to your PATH:
  $ export PATH="$PATH":`pwd`/depot_tools
  NOTE: You may want to add this to your .bashrc file or your shell's equivalent so that you don’t need to reset your $PATH manually each time you open a new shell.


## Development

* Open a terminal to the root of this project.
* Run the server with `make server`, and leave it running while you edit files.
* Your web browser opens to http://localhost:8081.
  * You may need to reload once.
* Edit, create docs as normal.
* To run tests, run `./runtests.sh`.

**Note:** If you see single-page breadcrumbs on pages such as
http://localhost:8081/tools/pub/cmd/pub-build.html, make sure that you've
installed the latest gem versions.
(Run `sudo bundle install` and then `bundle install`.)


### Windows development tips

You probably won't have **make** available on the command line by default.

* To just get up and running, run `jekyll` from the `src/site` folder.
* This starts up the Jekyll webserver and generates into `build/static`.
* If Jekyll does not generate output, you need to type `chcp 65001` at the
  command prompt to change the code page to UTF-8.
  (Jekyll fails silently if this is not done.)
* To **clean**, simply delete the contents of `build/static` and restart `jekyll`.


## Deploying the site

* Run `make clean && make deploy`.
  * This builds the site and places everything into `build/`, and then deploys
    the site. (Note: You can also run
    `make clean && make build` and then deploy manually using App Engine.)
  * This command uses the current branch for the App Engine version name.
* You will probably need to generate an
  [App-specific password](https://sites.google.com/a/google.com/second-factor/application-specific-passwords-faq).
  Save this password into the App Engine Launcher during the first deployment.

## Regenerating Dartisans playlists

Did you just run a Dartisans? Good for you! Here's what you need to do:

1. Update the description to be present tense (instead of future),
   and remove the link to the moderator page.
1. Ensure the episode is added to the Dartisans playlist, owned by
   Google Developers channel.
1. Sort the playlist by date.
1. Ensure your episode explicitly sets a Recorded On date.
1. Format your episode's title like this: "Dartisans ep. XX: Subtitle Here"
1. Pick a great image thumbnail. Don't use the static Dart logo.
1. Now run `make dartisansplaylist`
1. Test it, commit, and go!

## Updating the book

The files under `docs/dart-up-and-running/contents` are autogenerated from the DocBook files for
_Dart: Up and Running._ Here's how to update these files.

First, prepare:

* Make sure `dart` (the Dart VM) is in your `PATH`.

        > which dart
        /Users/me/dart/dart-sdk/bin/dart

* Make sure `xsltproc` is in your `PATH`.

        > which xsltproc
        /usr/bin/xsltproc

* Find a copy of the latest .xml files that make up dart-up-and-running. For example:

        > ls ~/Dart/dartbook/GITHUB
        LICENSE   bookinfo.xml  ch02.xml  ch05.xml  figs
        README.md ch00.xml  ch03.xml  code    foreword.xml
        book.xml  ch01.xml  ch04.xml  colo.xml

Now you're ready. From the top directory of this repo,
run the following command, specifying the directory that contains ch*.xml:

    make book BOOK_XML_DIR=<dir-with-xml-files>

For example:

    make book BOOK_XML_DIR=~/Dart/dartbook/GITHUB

Wait 4-5 minutes for results.

*Very* carefully check the diffs, paying special attention to the headers
at the top. Rerun the `make book` command if you aren't sure of the changes.

(Yes, different runs of `make book` can have different results, even when the
.xml files haven't changed. The problem I've noticed has been misnaming files
or skipping them in the navigation. Reported as dartbug.com/8128.)
