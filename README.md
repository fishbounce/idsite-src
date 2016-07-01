# Stormpath ID Site Source #

[Stormpath](http://stormpath.com/) is a User Management API that reduces
development time with instant-on, scalable user infrastructure. Stormpath's
intuitive API and expert support make it easy for developers to authenticate,
manage, and secure users and roles in any application.

This is the development environment for the Stormpath hosted ID Site.  You can
use this repository to build the same single page application (SPA) that
Stormpath provides by default.  The SPA uses Angular and Browserify and it is
built using Grunt and Yeoman.

If you want to build your own ID Site and are comfortable with Angular, you should
use this repository.

If you are not using Angular, you can build your own ID Site from scratch
but you will need to use [Stormpath.js][] in order to communicate with our API
from your custom ID Site application.

## Browser Support

ID Site will work in the following web browser environments:

* Chrome (all versions)
* Internet Explorer 10+
* Firefox 23+
* Safari 8+
* Android Browser, if Android version is 4.1 (Jellybean) or greater

## Installation

It is assumed that you have the following tools installed on your computer

* [Bower][]
* [Grunt][]
* [Node.JS][]
* [Localtunnel.me][]

You should clone this repository and then run this within the repository:

```sh
npm install
bower install
```

## Using `ez_dev`

Within this repository, there's a script called `ez_dev.sh`. This will setup a complete development environment for you
to be able to work on your ID Site Content.

Before we get into running the script, let's take a step back to see what `ez_dev` sets up.

A typical flow for using ID Site is the following:

1. You make a request of the `/login` endpoint of your application in the browser.
2. Your application redirects to Stormapth (api.stormpath.com)
3. Stormpath redirects to the ID Site configured in the Stormpath Admin Console
4. ID Site responds to your browser with the `login` view

When developing locally, you need an application that will connect to ID Site. And, in particular, ID Site being served
locally from the `idsite-src` repo that you cloned so that you can rapidly make changes and see them in action in real
time. And, there's a requirement that the traffic SSL encrypted.

The `ez_dev` script sets up the necessary architecture needed. The components are:

1. `fakesp` - A minimal application that will connect to ID Site
2. `localtunnel.me` - A free service that sets up an HTTPS proxy from the public internet to a locally running server.
3. `grunt serve` - A locally running instance of ID Site.

Once this is all running, you browser will automatically open up to: `http://localhost:8001` and you will be able to see the
updated ID Site content you are working on. Any saved changes to the ID Site content are immediately reflected in your browser
when you go to an ID Site view (such as `/login`).

Here's a visual representation of what is setup and the flow of the requests.

![tunnel architecture][tunnel_image]

Note: The `ez_dev` script alters the ID Site settings in your Stormpath Admin Console. When you are done working on
ID Site, it is recommended that you go back to your Admin Console and revert the `Domain Name`,
`Authorized Javascript Origin URLs`, and `Authorized Redirect URLs` settings.

## Development Process - Stormpath Tenants

Do you use Stormpath?  Are you forking this repository so that you can build a
modified version of our default application?  This section is for you.

After you have started your environment (all the steps above) and made your
customizations to your fork of this  repository, there are two ways you can
deploy your changes:

* **Directly form your fork, unminified**.  If you've forked this library and your changes
exist in your fork, you can simply point your ID Site Configuration to the URL
of your forked github repository.  Nothing else is required, however you will
be serving un-minified assets, which may be slower.

* **With Minification**.  If you want to serve minified assets for performance,
you need to run `grunt build` to build the application into it's minified form.
This will be placed in the `dist/` folder, which is not tracked by git.  You
will need to commit this output to another github repository, and then point
your ID Site configuration at that git repository.

## Development Process - Stormpath Engineers

Are you a Stormpath Engineer who is working on this module?  This section is
for you.

You need to create a build of this application, after you have merged all of
your changes into master.  You create the build with the `grunt build` task.
After the build is created, you need to copy the output from the `dist/` folder
in this repo (which is not tracked by git) and commit it to the [ID Site Repository][].

The purpose of the [ID Site Repository][] is to hold the minified output of this
development environment.  When a tenant is using the default ID Site configuration,
they are receiving the files that exist in the [ID Site Repository][].

When you push a commit to master on the [ID Site Repository][], the files
are consumed and pushed to our CDN for serving to any tenant which is using the
default ID Site.

Are you working on a feature that is not yet ready for master, but you'd like a
tenant to try it out?  No problem! Just create a named topic branch in the
[ID Site Repository][] and place the build output there.  The tenant can then
point their ID Site Configuration at the branch for testing purposes.  When you
merge the feature into master and deploy a master release, you should delete
the branch.

In this situation, you'll likely want to use `grunt build:debug` to create a
non-obfuscated build output (which will make in-browser debugging easier).


### Testing

To run the Selenium tests, you need to install Protractor:

```
npm install -g protractor
```

Start the development server by running `grunt serve`,
then run Protractor with the config file in this repo:

```
protractor protractor.conf.js
```

**WARNING**: This will modify the ID Site Configuration of the Stormpath Tenant
that is defined by these environment variables:

```
STORMPATH_CLIENT_APIKEY_ID
STORMPATH_CLIENT_APIKEY_SECRET
```
Alas, you must ensure that you are using a tenant that is not used by your
production application!

## Copyright

Copyright &copy; 2014 Stormpath, Inc. and contributors.

This project is open-source via the [Apache 2.0
License](http://www.apache.org/licenses/LICENSE-2.0).

[Bower]: http://bower.io
[createIdSiteUrl()]: https://docs.stormpath.com/nodejs/api/application#createIdSiteUrl
[Fake SP]: https://github.com/robertjd/fakesp
[Grunt]: http://gruntjs.com
[ID Site Repository]: https://github.com/stormpath/idsite
[Localtunnel.me]: http://localtunnel.me/
[Node.JS]: http://nodejs.org
[Stormpath Admin Console]: https://api.stormpath.com
[Stormpath.js]: https://github.com/stormpath/stormpath.js
[tunnel_image]: https://github.com/stormpath/idsite-src/blob/media/docs_images/idsite_tunnel_dev.png
