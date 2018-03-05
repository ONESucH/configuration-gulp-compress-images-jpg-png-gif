<pre>
// npm i gulp-imagemin imagemin-pngquant imagemin-zopfli imagemin-mozjpeg imagemin-giflossy gulp-plumber gulp-uglify pump --save-dev

/* Оптимизируем картинки */
var gulp = require('gulp'),
    imagemin = require('gulp-imagemin'),
    imageminPngquant = require('imagemin-pngquant'),
    imageminZopfli = require('imagemin-zopfli'),
    imageminMozjpeg = require('imagemin-mozjpeg'),
    imageminGiflossy = require('imagemin-giflossy'),
/* Оптимизируем JS */
    plumber = require('gulp-plumber'),
    uglify = require('gulp-uglify'),
    pump = require('pump'); // доп. пакет для gulp-uglify

gulp.task('reduce', ['js'], function () {
  return gulp.src(['dist/**/*.{gif,png,jpg}'])
    .pipe(imagemin([
      imageminPngquant({
        speed: 1,
        quality: 98
      }),
      imageminZopfli({more: true}),
      imageminGiflossy({
        optimizationLevel: 3, // уровень сжатия
        optimize: 3,
        lossy: 2
      }),
      imagemin.svgo({
        plugins: [{removeViewBox: false}]
      }),
      imagemin.jpegtran({progressive: true}),
      imageminMozjpeg({quality: 85})
    ]))
    .pipe(gulp.dest('dist'));
});

gulp.task('js', function (cb) {
  pump([
    gulp.src('dist/**/*.js'),
    plumber(),
    uglify(),
    gulp.dest('dist')
  ], cb);
});


gulp.task('default', ['reduce']);
</pre>
