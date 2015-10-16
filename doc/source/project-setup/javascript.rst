===================================
JavaScript Project Setup Guidelines
===================================

.. note::

   We support the current version of node.js and npm available in the LTS
   releases of Ubuntu. As of this writing, these are Node v0.10.29 and
   npm v1.4.21. Using newer versions may create unexpected results.

Quickstart
----------
These are generic project setup instructions for an OpenStack JavaScript
project. Individual projects may extend or override these instructions, so
please check for additional documentation.

Install Dependencies
====================

On OSX (using homebrew)::

  brew install nodejs

.. note::
   Homebrew does not provide an easy way to install our supported version of
   node.js and npm. The above appears to be compatible, however unexpected
   results may still occur.

On Debian::

  # Using Ubuntu
  sudo curl -sL https://deb.nodesource.com/setup_0.10 | sudo -E bash -
  sudo apt-get install -y nodejs

  # Using Debian
  sudo curl -sL https://deb.nodesource.com/setup_0.10 | sudo bash -
  sudo apt-get install -y nodejs

On CentOS::

  sudo curl -sL https://rpm.nodesource.com/setup | bash -
  sudo yum install -y nodejs

Check out and initialize the project
====================================
All project initialization, and virtual environment creation, is handled via
npm. This process is fairly straightforward, and will ensure that all
dependencies, tools, and other resources are available.::

  # Check out the project
  git checkout git://git.openstack.org/openstack/<PROJECT NAME>.git

  # Initialize the project.
  cd <PROJECT NAME>
  npm install

Exercise the codebase
=====================
After initialization, you should be able to run some basic tests.

  :code:`npm run lint`
    This will run our codestyle checks.
  :code:`npm test`
    This will run tests and report on test coverage.
  :code:`npm start`
    For web-based projects, this command should launch a server.

Common Tools
------------
The following section addresses each command prescribed by the
Consistent Testing Interface, including tools and setup instructions for the
same.

Codestyle Checks
================
  Commands:
    :code:`npm run lint`

OpenStack requires the custom npm script 'lint' to execute our codestyle
checks. The tool we use is called ESLint, and our rules are published to npm
as eslint-config-openstack_. To initialize your project with these rules,
perform the following steps in your project root.

First, add eslint and eslint-config-openstack to your project, and initialize
a basic .eslintrc file.::

  npm install --save-dev eslint eslint-config-openstack
  echo "extends: openstack" > .eslintrc

Second, add the lint script to your :code:`project.json` file::

  {
    ...
    'scripts' : {
      'lint': 'eslint ./'
    },
    ...
  }

For more information on how to exclude files, override specific rules, or on
the rules themselves, please visit the `ESLint Project`_. If you'd like to
contribute to OpenStack's ESLint rules, you may do so at
`eslint-config-openstack`_.

Executing Tests and Coverage
============================
  Commands:
    :code:`npm test`

OpenStack requires a sane testing and code coverage strategy for each
project, though we do not prescribe the tools and coverage threshold, as
these may differ based on circumstance and project type. Generated test
reports should be placed in :code:`./reports` in your projects' root directory.
Generated coverage output should similarly be placed in :code:`./cover`.

Example setup for browser projects, using Karma, Jasmine, and Istanbul.::

  // ./package.json
  {
    ...
    "scripts": {
      ...
      "test": "karma start ./karma.conf.js",
      ...
    },
    "devDependencies": {
      ...
      "jasmine": "2.3.2",
      "karma": "0.13.9",
      "karma-chrome-launcher": "0.2.0",
      "karma-cli": "0.1.0",
      "karma-coverage": "0.5.0",
      "karma-firefox-launcher": "0.1.6",
      "karma-jasmine": "0.3.6",
      "karma-phantomjs-launcher": "0.2.1",
      "karma-threshold-reporter": "0.1.15",
      ...
    }
  }

  // ./karma.conf.js
  module.exports = function (config) {
    config.set({
      'frameworks': ['jasmine'],
      'browsers': ['PhantomJS', 'Chrome', 'Firefox'],
      'reporters': ['progress', 'coverage', 'threshold'],

      'plugins': [
        'karma-jasmine',
        'karma-coverage',
        'karma-threshold-reporter',
        'karma-phantomjs-launcher',
        'karma-chrome-launcher',
        'karma-firefox-launcher'
      ],

      'preprocessors': {
        'www/js/{!lib/**/*.js,*.js}': ['coverage']
      },

      'files': [
        // Library files, with some ordering.
        'www/js/lib/angular.js',
        'node_modules/angular-mocks/angular-mocks.js',
        'www/js/lib/*.js',

        // Application files
        'www/js/**/*.js',

        // Tests
        'test/js/**/*.js'
      ],

      'coverageReporter': {
        type: 'html',
        dir: 'cover',
        instrumenterOptions: {
          istanbul: {noCompact: true}
        }
      },

      // Coverage threshold values.
      thresholdReporter: {
        statements: 100,
        branches: 100,
        functions: 100,
        lines: 100
      },
      'singleRun': true
    });
  };

Example setup for node.js projects, using Jasmine and Istanbul::

    # /package.json
    {
      ...
      "scripts": {
        "test": "istanbul cover --print=detail --include-all-sources jasmine",
        ...
      },
      ...
      "devDependencies": {
        ...
        "istanbul": "0.3.17",
        "jasmine": "2.3.1",
        ...
      }
    }

    # /spec/support/jasmine.json
    {
      "spec_dir": "spec",
      "spec_files": [
        "**/*.js",
        "!helpers/**/*.js"
      ],
      "helpers": [
        "helpers/**/*.js"
      ]
    }

Package Tarball Generation
==========================
  Commands:
    :code:`npm pack`

OpenStack uses :code:`npm pack` to generate a release tarball, which will
compile all files listed in :code:`package.json`. If your project requires
concatenation, minification, or any other preprocessing to create a valid
tarball, you may use the npm :code:`prepublish` hook to trigger these steps.

An example package.json file may look as follows.::

  {
    ...
    'scripts': {
      'prepublish': 'uglify -s ./src/**/*.js -o ./lib/generated.min.js'
    },
    files: [
      'LICENSE',
      'README.md',
      'index.js',
      'lib/*.js'
    ],
    ...
  }

All packages should include:

 - A README
 - A LICENSE file
 - All source code

Generate Documentation
======================
  Commands:
    :code:`npm run document`

No canonical way of generating documentation for JavaScript projects has been
discussed yet. If you would like to contribute, please join the conversation
on freenode in #openstack-docs or #openstack-javascript.

Validate Dependency Licences
============================
  Commands:
    :code:`npm run document`

No common way of validating dependency licenses has been discussed yet. If you
would like to contribute, please join the conversation on freenode in
#openstack-javascript.

Import Translation Strings
==========================
  Commands:
    :code:`npm run translate`

No canonical way of importing translations for JavaScript projects has been
discussed yet. If you would like to contribute, please join the conversation
on freenode in #openstack-i18n and #openstack-javascript.

Best Practices
--------------
All best practices outlined in the general project setup guide apply to
JavaScript projects. What follows are additional suggestions.

Use the tool, not the adapter
=============================
This section in particular addresses gulp, grunt, and similar build
tools that aim to be the collect all build steps under one roof. While
useful, they often achieve this by wrapping a tool that already exists, while
adding dependencies of their own. In order to avoid this bloat - and
potential cross-dependency version mismatch - it is far easier to use the
tool directly, than maintaining the additional abstraction layer.

Examples:
* Use :code:`eslint` instead of :code:`gulp-eslint`
* Use :code:`karma` instead of :code:`grunt-karma`
* Use :code:`webpack` instead of :code:`gulp-webpack`

Do Not Minify
=============
Minification is a frequently used optimization step that reduces
JavaScript file size at the cost of legibility. While this step often provides
significant load reduction, especially when combined with HTTP GZip
compression and HTTP Cache headers, we prefer to leave this decision in the
hands of the operator.

Do Not Use Fuzzy Versions
=========================
Dependencies declared in package.json or bower.json use a fuzzy version
markup that allows installed dependencies to be flexible at the cost of
deterministic builds. This is especially dangerous if one of your project's
dependencies also uses fuzzy versioning, when an incompatibility is introduced
in a transient dependency. Please use fixed versions for all your
dependencies, and (if possible) lock them using :code:`npm shrinkwrap`.

Naming conventions
==================
Naming a project is tricky, as each project must avoid name reservation
conflict, while clearly communicating its purpose and intent. Names should
consider the following:

* How does it distinguish itself - amongst openstack projects - as a
  javascript project rather than a python project.
* How does it distinguish itself - among npm or bower packages - that it is an
  openstack project.
* How does it clearly communicate which team it is associated with?
* How does it communicate its purpose and content?
* How does it acommodate our existing naming conventions in gerrit?
* How does it communiate peer library requirements, such as angular.js?

Suggested names patterns:
  :code:`openstack/oslo-jslib`
    A javascript library of common utilities used in other OpenStack
    projects, published to bower as :code:`openstack-oslo-jslib`
  :code:`openstack/ironic-jslib`
    A javascript library that integrates with the Ironic API, published to bower
    as :code:`openstack-ironic-jslib`
  :code:`openstack/ironic-jsclient`
    A commandline client for ironic, written in JavaScript, published to npm
    as :code:`openstack-ironic-jsclient`
  :code:`openstack/ironic-webclient`
    A web client that provides a human interface to the Ironic API.

Exceptions may occur where a tool prescribes an existing naming convention.

  eslint-config-openstack
    A list of JavaScript style rules for eslint.

Publish multiple artifacts
==========================
If your JavaScript project can be consumed by more than one style of
application, it's often helpful to publish separate artifacts for each
indented use.

Example artifact names might include:

* openstack-oslo-jslib.ng.js
* openstack-oslo-jslib.react.js

.. _How to contribute to OpenStack: https://wiki.openstack.org/wiki/How_To_Contribute
.. _NPM package scripts: https://docs.npmjs.com/misc/scripts
.. _ESLint Project: http://eslint.org
.. _eslint-config-openstack: http://git.openstack.org/cgit/openstack/eslint-config-openstack