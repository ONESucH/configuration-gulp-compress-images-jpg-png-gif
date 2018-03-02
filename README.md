<pre>
// npm i gulp-imagemin imagemin-pngquant imagemin-zopfli imagemin-mozjpeg imagemin-giflossy --save-dev<br>
var gulp = require('gulp'),<br>
  imagemin = require('gulp-imagemin'),<br>
  imageminPngquant = require('imagemin-pngquant'),<br>
  imageminZopfli = require('imagemin-zopfli'),<br>
  imageminMozjpeg = require('imagemin-mozjpeg'),<br>
  imageminGiflossy = require('imagemin-giflossy');<br><br>

gulp.task('imgOptimizer', function () {<br>
  return gulp.src(['dist/**/*.{gif,png,jpg}'])<br>
    .pipe(imagemin([<br>
      imageminPngquant({<br>
        speed: 1, // скорость сжатия<br>
        quality: 98<br>
      }),<br>
      imageminZopfli({<br>
        more: true<br>
      }),<br>
      imageminGiflossy({<br>
        optimizationLevel: 3, // уровень сжатия<br>
        optimize: 3,<br>
        lossy: 2<br>
      }),<br>
      imagemin.svgo({<br>
        plugins: [{<br>
          removeViewBox: false // Удалить просматреваемые боксы<br>
        }]<br>
      }),<br>
      imagemin.jpegtran({<br>
        progressive: true // если картинка - иконка, то делает картинку с прозрачным фоном<br>
      }),<br>
      imageminMozjpeg({<br>
        quality: 85<br>
      })<br>
    ]))<br>
    .pipe(gulp.dest('dist'));<br>
});<br><br>

gulp.task('default', ['imgOptimizer']);<br>

</pre>
