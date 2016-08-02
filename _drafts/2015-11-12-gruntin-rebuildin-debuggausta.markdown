---
layout: post
title: Gruntin rebuildin debuggausta
---
Alkutilanne: kesti kauan, loppui virheeseen.

    Execution Time
    loading tasks          1.7s  ▇▇ 4%
    jshint:all            10.6s  ▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 25%
    concurrent:newercopy   2.2s  ▇▇▇ 5%
    mocha                 22.3s  ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 53%
    webpack:dev            3.9s  ▇▇▇▇▇ 9%
    stylus:build           1.2s  ▇ 3%
    Total 42.1s

Total kesti kauan, miksi jshint meni 10 sekuntia?

    Running "newer:jshint:all" (newer) task
    Running "jshint:all" (jshint) task
    >> 626 files lint free.

Ei tarvi kaikkia ladata! Grunt newer ei toimi oikein. https://github.com/tschaub/grunt-newer/issues/39

    jshint: {
      all: ['./src/**/*.js', 'Gruntfile.js'],
      options: {
        jshintrc: true
      }
    }

muotoon: 

    jshint: {
      all: {
        src: ['./src/**/*.js', 'Gruntfile.js'],
        options: {
          jshintrc: true
        }
      }
    }

Jonka jälkeen:


    Running "newer:jshint:all" (newer) task
    Running "jshint:all" (jshint) task
    >> 1 file lint free.

    Execution Time (2015-11-12 15:33:55 UTC)
    loading tasks          1.5s  ▇▇▇ 4%
    concurrent:newercopy   2.1s  ▇▇▇▇ 6%
    mocha                 23.6s  ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 70%
    webpack:dev            4.6s  ▇▇▇▇▇▇▇▇ 14%
    stylus:build           1.4s  ▇▇▇ 4%
    Total 33.5s

Voitto kotiin! Sitten testit joko uuteen terminaaliin tai taustalle ajamaan.
Uusi task watchiin testeille

    watch: {
      ...
      mocha: {
       files: ['./src/**/*.js', 'Gruntfile.js'],
       tasks: ['mocha:silent']
      }
    }

Uusi silent tester rebuildia varten

    mocha: {
      ...
      silent: {
        src: ['./**/*.spec.js'],
        options: {
          reporter: 'varmais-mocha-reporter'
        }
      }
    }

Uusi reporter ilmoittamaan testien kulun

    'use strict';
    var Base = require('mocha/lib/reporters/base');

    function VarmaisReporter(runner) {
      Base.call(this, runner);
      runner.on('end', function () {
        console.log('################');
        console.log('  VARMAIS TEST REPORT:');
        this.epilogue();
        console.log('################');
      }.bind(this));
    }

    VarmaisReporter.prototype.__proto__ = Base.prototype;
    module.exports = VarmaisReporter;

Jonka jälkeen rebuild:

    Execution Time
    loading tasks          1.9s  ▇▇▇▇▇▇▇ 17%
    newer:jshint:all      138ms  ▇ 1%
    newer:react:js        136ms  ▇ 1%
    concurrent:newercopy   2.8s  ▇▇▇▇▇▇▇▇▇▇ 25%
    webpack:dev            4.8s  ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 43%
    stylus:build           1.2s  ▇▇▇▇ 11%
    Total 11.2s

Ja testit:

    ###########
      VARMAIS TEST REPORT:
      2251 passing (21s)
      4 pending
    ###########
    Done, without errors.

    Execution Time
    loading tasks          1.8s  ▇▇▇ 7%
    mocha:silent          25.4s  ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 93%
    Total 27.2s

Ja parasta: Testien failaus ei pysäytä buildia vaan voi silti nähdä selaimessa.

No mutta vielä:

    Running "newer:copy:assets" (newer) task
    Running "copy:assets" (copy) task
    Copied 1 file
    Done, without errors.

    Execution Time
    loading tasks      2.1s  ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 97%
    newer:copy:assets  39ms  ▇ 2%
    Total 2.1s

Mitä järkeä ladata taskia 2 sekuntia 40 millisekunnin hommalle? :D

    Execution Time (2015-11-12 15:44:20 UTC)
    loading tasks      2.1s  ▇▇▇▇▇▇▇▇▇▇▇▇ 24%
    newer:jshint:all  140ms  ▇▇ 2%
    newer:react:js    129ms  ▇ 1%
    webpack:dev        4.9s  ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 55%
    stylus:build       1.4s  ▇▇▇▇▇ 15%
    Total 8.8s

Noniin konteksti switch jne kaikki paremmin.
