---
layout: post
title:  "Es6 React boilerplate using babel, gulp and webpack"
date:   2016-05-15 20:11:01 +0100
categories: "es6 react webpack gulp"
level: "Intermediate"
permalink: /blog/:title
---

I have been learning [React][react] for the last 2 months, it is easy to get started if you just want to fiddle with the library. But I struggled to find a good set up for a real project.

After reading and going through many projects set ups, I have put together an all es6 boilerplate. I will share my setup and i hope it helps. Even the webpack and gulp configuration files are in es6.

## Setting Up

Before we proceed please make sure you have the latest version of node. I am using v6.0.0.

Please clone the boilerplate and install the dependencies:

{% highlight javascript %}
git clone https://github.com/dahfool/react-boilerplate.git
npm install
{% endhighlight %}

### Webpack

Webpack will bundle all your js or jsx files together and use babel to compile your es6 code. To use es6 in your webpack.config.js file rename it to webpack.config.babel.js (already done in the boilerplate).

##### webpack.config.babel.js
{% highlight javascript %}
import projectPath from './projectpath.babel'

export default {
  entry: projectPath.js() + 'index.js',
  output: {
    path: projectPath.BuildJs(),
    filename: 'script.js'
  },
  module : {
    loaders : [
      {
        test : /\.jsx?/,
        include : projectPath.js(),
        loader : 'babel'
      }
    ]
  }
};
{% endhighlight %}

Find out more about [webpack configuration][webpack-config].

The source and destination path are stored in projectpath.babel.js for `maintainability` and `re usability` and is imported in the gulp and webpack config files.

##### projectpath.babel.js

{% highlight javascript %}
import path from 'path';

/* Environment Path */
const PATH = {
    public: path.resolve(__dirname, 'public/'),
    src: path.resolve(__dirname, 'src/'),
    sass () { return this.src + "/scss/" },
    js () { return this.src + "/js/" },
    BuildJs () { return this.public + "/js/" },
    buildCss () { return this.public + "/css/" }
};

export default PATH;
{% endhighlight %}

### Gulp

The gulpfile has also been rename gulpfile.babel.js to use es6.

##### Extract from gulpfile.babel.js
{% highlight javascript %}
import webpack from 'webpack-stream';
import webpackConfig from './webpack.config.babel';
import projectPath from './projectpath.babel';

/**
 * webpack - react es6 compilation
 */
gulp.task('webpack', () => {
  return gulp.src(projectPath.js()+'index.js')
    .pipe(webpack( webpackConfig ))
    .pipe(gulp.dest(projectPath.BuildJs()));
});


{% endhighlight %}

The webpack (webpack-stream') gulp task use webpackConfig to get the webpack configuration and project paths form projectPath.

## Getting Started

Your es6 changes should be made in `src/js/`, `src/js/index.js` is where your application starts.

Open `public/index.html` to see your changes.

## Useful links

 - [ReactJs][react]
 - [Javascript using classes] [jsclasses]
 - [Gulp] [gulp]
 - [Babel] [babel]

[react-get-started]: https://facebook.github.io/react/docs/getting-started.html
[webpack-config]: http://webpack.github.io/docs/configuration.html
[react]: https://facebook.github.io/react/index.html
[jsclasses]: https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes
[gulp]: http://gulpjs.com/
[babel]: https://babeljs.io/
