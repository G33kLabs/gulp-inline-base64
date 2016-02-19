GULP inline-base64
==================

This helper will inject images and fonts into your css files.

Warning ! This technique is really efficient with small files (<14 Kb) cause it avoids DNS requests and makes the page loading faster. But for larger files it will be a mistake to use it !

Install it
----------

```
npm install --save-dev gulp-inline-base64
```

Use it
------

Here is my sass config. As you can see, I use the 'maxSize' option to specitfy that files larger than 14kb are not injected into the css file.

```
var sass = require('gulp-sass'),
	inline_base64 = require('gulp-inline-base64'),
	autoprefixer = require('gulp-autoprefixer');
...

// SASS
gulp.task('sass', function() {
    return gulp.src([
        path_src + '/css/**/*.scss',
        '!' + path_src + '/css/**/_*.scss'
    ])
    .pipe(sass({
        includePaths: [
            path_src + '/css/',
            'bower_components/',
        ],
        imagePath: path_src
    }))
    .pipe(inline_base64({
        baseDir: path_src,
        maxSize: 14 * 1024,
        debug: true
    }))
    .pipe(autoprefixer("last 2 version", "> 1%", {
        cascade: true
    }))
    .pipe(gulp.dest(path_tmp + '/css'))
});
```

Options
-------
 - ``baseDir`` : the root path for assets
 - ``useRelativePath`` : overrides baseDir; root path is relative to the input file's directory
 - ``maxSize`` : define the limit size of injected assets
 - ``debug`` : show debug messages

Force asset injection
---------------------

In your css file, just add ``,true`` to the image url : it will force the asset to be injected in base64 in css file, event if the ``maxSize`` is reached.

```
div.logo {
	background: transparent url(/img/logo.png,true) no-repeat center center;
}
```