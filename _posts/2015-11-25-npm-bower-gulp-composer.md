---
layout: post
title: "基于laravel构建npm,gulp,composer,bower"
description: "前端自动化"
keywords: "node.js,gulp,composer,bower"
category: 前端
tags: [前端]
---
{% include JB/setup %}

###gulp打造前端自动化工作流

最近基于laravel框架，使用gulp构建前端自动化工作流, 提高开发效率， 记录一下， go !以下基于mac进行，其它环境使用相应的包管理工具安装。

<!-- more -->

####node,npm安装

brew是mac下不错的包管理工具

    brew install node . 该命令执行后，自动装好node和npm

下面的bower,gulp需要依赖node


####gulp安装

    全局安装 npm isntall gulp -g
    cd 项目目录
    npm install --save-dev gulp
    package.json包含了我们所需要的npm包， 建立package.json
    npm install(生成文件在node_modules中)
    配置文件:gulpfile.js， 完成前端打包等工作

####bower安装

    全局安装 npm install bower -g
    cd 项目目录
    npm install --save-dev bower
    配置文件:bower.json包含了我们所需要的库文件, 建立bower.json
    bower install(生成文件在bower_components中)

####composer安装

    brew install composer
    配置文件:composer.json, 建立composer.json
    composer install(生成文件在vendor中)


####package.json

所需要依赖的node包

    {
      "repository": {},
      "devDependencies": {
        "del": "^0.1.2",
        "gulp": "^3.9.0",
        "gulp-autoprefixer": "0.0.10",
        "gulp-concat": "^2.3.4",
        "gulp-less": "^3.0.3",
        "gulp-minify-css": "^0.3.7",
        "gulp-rename": "^1.2.2",
        "gulp-rev": "^1.1.0",
        "gulp-uglify": "^1.0.0",
        "laravel-elixir": "*"
      }
    }

####gulpfile.js

for example: 完成css,js压缩打包，加版本号等

    var elixir = require('laravel-elixir');
    var gulp = require('gulp');
    var less = require('gulp-less');
    var autoprefixer = require('gulp-autoprefixer');
    var uglify = require('gulp-uglify');
    var concat = require('gulp-concat');
    var rev = require('gulp-rev');
    var del = require('del');
    var minifycss = require('gulp-minify-css');
    var rename = require('gulp-rename');


    /*
     |--------------------------------------------------------------------------
     | Elixir Asset Management
     |--------------------------------------------------------------------------
     |
     | Elixir provides a clean, fluent API for defining some basic Gulp tasks
     | for your Laravel application. By default, we are compiling the Less
     | file for our application, as well as publishing vendor resources.
     |
     */

    elixir(function(mix) {
        mix.task(['build', 'watch']);
    });


    // CSS task
    gulp.task('css', function() {

        // Cleanup old assets
        del(['public/build/css/*.css'], function (err) {});

        // Convert scss first
        gulp.src('resources/assets/less/*.less')
            .pipe(less())
            .pipe(autoprefixer('last 10 version'))
            .pipe(concat('app.css'))
            .pipe(minifycss())
            .pipe(rename(function (path) {
                path.basename += ".min"
            }))
            .pipe(rev())
            .pipe(gulp.dest('public/build/css'));
    });

    // JavaScript task
    gulp.task('js', function() {
        // Cleanup old assets
        del(['public/build/js/*.js'], function (err) {});

        // Concat and uglify the JavaScript assets
        // Afterwards add the MD5 hash to the filename
        gulp.src('resources/assets/js/*.js')
            .pipe(concat('app.js'))
            .pipe(uglify())
            .pipe(rename(function (path) {
                path.basename += ".min"
            }))
            .pipe(rev())
            .pipe(gulp.dest('public/build/js'));
    });

    //image task
    gulp.task('copy:image', function () {
      gulp.src('resources/images/**/*')
        .pipe(gulp.dest('public/build/images'));
    });


    gulp.task('build', ['css', 'js', 'copy:image']);

    gulp.task('watch', function(){
        gulp.watch('resources/assets/css/**/*.css', ['css']);
        gulp.watch('resources/assets/js/**/*.js', ['js']);
    });

    // The default task (called when you run `gulp` from cli)
    // gulp.task('default', ['build', 'watch']);

####bower.json

for example: 安装项目所需要的组件

    {
      "name": "laravel",
      "version": "1.0.0",
      "authors": [
        "caijinlin <caijinlin2012@gmail.com>"
      ],
      "license": "MIT",
      "ignore": [
        "**/.*",
        "node_modules",
        "bower_components",
        "test",
        "tests"
      ],
      "dependencies": {
        "bootstrap": "3.1.1",
        "jquery": "1.11.3",
        "remarkable-bootstrap-notify": "3.1.3",
        "jquery_lazyload": "~1.9.1"
      }
    }


####composer.json

for example: 依赖laravel框架

    {
        "name": "laravel/laravel",
        "description": "The Laravel Framework.",
        "keywords": ["framework", "laravel"],
        "license": "MIT",
        "type": "project",
        "require": {
            "laravel/framework": "5.0.*"
        },
        "require-dev": {
            "phpunit/phpunit": "~4.0",
            "phpspec/phpspec": "~2.1",
            "barryvdh/laravel-debugbar": "~2.0"
        },
        "autoload": {
            "classmap": [
                "database"
            ],
            "psr-4": {
                "App\\": "app/"
            }
        },
        "autoload-dev": {
            "classmap": [
                "tests/TestCase.php"
            ]
        },
        "scripts": {
            "post-install-cmd": [
                "php artisan clear-compiled",
                "php artisan optimize"
            ],
            "post-update-cmd": [
                "php artisan clear-compiled",
                "php artisan optimize"
            ],
            "post-create-project-cmd": [
                "php -r \"copy('.env.example', '.env');\"",
                "php artisan key:generate"
            ]
        },
        "config": {
            "preferred-install": "dist"
        }
    }



