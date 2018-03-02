// npm i gulp-imagemin imagemin-pngquant imagemin-zopfli imagemin-mozjpeg imagemin-giflossy --save-dev
var gulp = require('gulp'),
  imagemin = require('gulp-imagemin'),
  imageminPngquant = require('imagemin-pngquant'),
  imageminZopfli = require('imagemin-zopfli'),
  imageminMozjpeg = require('imagemin-mozjpeg'),
  imageminGiflossy = require('imagemin-giflossy');

gulp.task('imgOptimizer', function () {
  return gulp.src(['dist/**/*.{gif,png,jpg}'])
    .pipe(imagemin([
      imageminPngquant({
        speed: 1, // скорость сжатия
        quality: 98
      }),
      imageminZopfli({
        more: true
      }),
      imageminGiflossy({
        optimizationLevel: 3, // уровень сжатия
        optimize: 3,
        lossy: 2
      }),
      imagemin.svgo({
        plugins: [{
          removeViewBox: false // Удалить просматреваемые боксы
        }]
      }),
      imagemin.jpegtran({
        progressive: true // если картинка - иконка, то делает картинку с прозрачным фоном
      }),
      imageminMozjpeg({
        quality: 85
      })
    ]))
    .pipe(gulp.dest('dist'));
});

gulp.task('default', ['imgOptimizer']);
