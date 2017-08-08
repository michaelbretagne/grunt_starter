## Quick Grunt install

Steps to follow if you want to install and start working with Grunt in less than 5 minutes!

## What is Grunt?
Grunt is a JavaScript task runner, a tool used to automatically perform frequently used tasks such as minification, compilation, unit testing, linting, etc.

## Requirement

- [node.js](https://nodejs.org/en/)

## Resource

- [gruntjs.com](https://gruntjs.com/getting-started)

## Plugins installed in this guide
- [grunt-contrib-uglify](https://github.com/gruntjs/grunt-contrib-uglify) (Minify JavaScript files with UglifyJS)
- [grunt-contrib-qunit](https://github.com/gruntjs/grunt-contrib-qunit) (Run QUnit unit tests in a headless PhantomJS instance)
- [grunt-contrib-concat](https://github.com/gruntjs/grunt-contrib-concat) (Concatenate files)
- [grunt-contrib-jshint](https://github.com/gruntjs/grunt-contrib-concat) (Validate files with JSHint)
- [grunt-contrib-sas](https://github.com/gruntjs/grunt-contrib-sass) (Compile Sass to CSS)
**Note:** Sass task requires to have [Ruby](https://www.ruby-lang.org/en/downloads/) and [Sass](http://sass-lang.com/install) installed
- [grunt-contrib-watch](https://github.com/gruntjs/grunt-contrib-watch) (Run predefined tasks whenever watched file patterns are added, changed or deleted)

More plugins can be foud here: [gruntjs.com](https://gruntjs.com/plugins)

## Files structure

After completing all the guide steps, the file structure should look like this:

    myproject/
    │── dist/                  // will hold all the final minified file
    │    └── css/
    │    └── js/
    │
    │── src/                   // will hold all the original files
    │    │── index.html
    │    └── css/
    │    └── scss/
    │        └── style.scss
    │    └── js/
    │         └── script.js
    │
    │── img/
    │    └── src/
    │
    │── node_modules/
    │
    │── README.md
    │── Gruntfile.js
    └── package.json    
    

## Steps by steps guide


**1. Check if npm is up-to-date**

    npm update -g npm

**2. Install Grunt's command line interface (CLI) globally**<br>
Skip this steps if you already installed Grunt for another project

    npm install -g grunt-cli

**3. Install grunt-init**<br>
Grunt-init is a scaffolding tool used to automate project creation

    npm install -g grunt-init

**4. Create a folder for your project**

    mkdir ~/desktop/myproject


**5. Inside "myproject" create a folder called "src", a folder called "build", and a "README.md"**

    cd ~/desktop/myproject
    mkdir src img img/src
    touch README.md

**6. Create folders to hold your files***

    cd src
    mkdir css scss js
    touch index.html
    cd scss
    touch style.scss
    cd ..
    cd js
    touch script.js

**7. Create a project based**

    cd ~/desktop/myproject
    npm init

then enter info about your project

**8. Install the plugins you need for your project**<br>

    npm install grunt --save-dev
    npm install grunt-contrib-uglify --save-dev
    npm install grunt-contrib-qunit --save-dev
    npm install grunt-contrib-concat --save-dev
    npm install grunt-contrib-jshint --save-dev
    npm install grunt-contrib-sass --save-dev
    npm install grunt-contrib-watch --save-dev

**8. Create a basic Gruntfile.js**

    touch Gruntfile.js

**9. Set up the Gruntfile**

    vim Gruntfile.js

**Copy** and **paste** the code bellow into **Gruntfile.js**<br>
**Note:** If you install other plugins, update the code bellow with the plugins configurations

    module.exports = function(grunt) {

       grunt.initConfig({
         pkg: grunt.file.readJSON('package.json'),
         concat: {
           options: {
             separator: ';'
           },
           dist: {
             src: ['src/**/*.js'],
             dest: 'dist/<%= pkg.name %>.js'
           }
         },
         uglify: {
           options: {
             banner: '/*! <%= pkg.name %> <%= grunt.template.today("dd-mm-yyyy") %> */\n'
           },
           dist: {
             files: {
               'dist/<%= pkg.name %>.min.js': ['<%= concat.dist.dest %>']
             }
           }
         },
         qunit: {
           files: ['test/**/*.html']
         },
         jshint: {
           files: ['Gruntfile.js', 'src/**/*.js', 'test/**/*.js'],
           options: {
             // options here to override JSHint defaults
             globals: {
               jQuery: true,
               console: true,
               module: true,
               document: true
             }
           }
         },
         sass: {
           dist: {
             files: {
               'src/css/style.css': 'src/scss/style.scss'
             }
           }
         },  
         watch: {
           scripts: {
             files: ['<%= jshint.files %>'],
             tasks: ['jshint', 'qunit']
           },
           sass: {
             files: ['src/scss/*.scss'],
             tasks: ['sass']
           }
         }
       });

       grunt.loadNpmTasks('grunt-contrib-uglify');
       grunt.loadNpmTasks('grunt-contrib-qunit');
       grunt.loadNpmTasks('grunt-contrib-concat');
       grunt.loadNpmTasks('grunt-contrib-jshint');
       grunt.loadNpmTasks('grunt-contrib-sass');
       grunt.loadNpmTasks('grunt-contrib-watch');

       grunt.registerTask('test', ['jshint', 'qunit']);

       grunt.registerTask('default', ['jshint', 'qunit', 'concat', 'uglify', 'sass']);

    };

To **save** and **exit** Gruntfile.js from the text editor vim, press **Esc** key, type **:x**

**10. Run Grunt**

    grunt

If everything works, you can start working on your project while Grunt is watching you!

    grunt watch
