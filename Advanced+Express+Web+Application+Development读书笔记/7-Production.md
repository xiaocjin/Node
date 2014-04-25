#产品化
##全局异常处理
    var cluster = require('cluster')
      , http = require('http')
      , numCPUs = require('os').cpus().length
      , logger = require('../logger')
      , domain = require('domain');
    function Cluster() {}
    Cluster.prototype.run = function(module) {
      if (cluster.isMaster) {
        for (var i = 0; i < numCPUs; i++) {
          cluster.fork();
        }
        cluster.on('exit', function(worker, code, signal) {
          logger.info('Worker ' + worker.process.pid + ' died');
          cluster.fork();
        });
      } else {
        var d = domain.create();
        d.on('error', function(err) {
          logger.info('Error ', err);
          process.exit(1);
        });
        d.run(function() {
          require(module);
        });
      }
    }
    module.exports = Cluster;

##使用Redis存放Session
  * npm install hiredis redis --save

##SSL termination

##缓存
    app.use(express.static('public',
      { maxAge: config.get('express:staticCache') }));
    app.use(express.static('public/components',
      { maxAge: config.get('express:staticCache') }));
    app.use('/bootstrap',express.static(
      'public/components/bootstrap/docs/assets/css',
        { maxAge: config.get('express:staticCache') }));
    app.use('/sockets',
      express.static('public/components/socket.io-client/dist/',
        { maxAge: config.get('express:staticCache') }));

##网站图标
    app.use(express.favicon('public/components/vision/favicon.ico'),
      { maxAge: config.get('express:staticCache') });
      
##Minification

###模块
    * npm install grunt-contrib-uglify --save-dev
    * npm install grunt-contrib-cssmin --save-dev

###代码

    grunt.loadNpmTasks('grunt-contrib-uglify');
    grunt.loadNpmTasks('grunt-contrib-cssmin');
    uglify: {
      dist: {
        files: {
          'public/components/vision/templates.min.js':
          'public/components/vision/templates.js',
          'public/components/vision/vision.min.js':
          'public/components/vision/vision.js',
          'public/components/json2/json2.min.js':
          'public/components/json2/json2.js',
          'public/components/handlebars/handlebars.runtime.min.js':
          'public/components/handlebars/handlebars.runtime.js'
        }
      }
    },
    cssmin: {
      minify: {
        expand: true,
        src: ['public/components/vision/vision.css'],
        ext: '.min.css'
      }
    }

##压缩
    app.use(express.compress());

##Logging
    if (process.env['NODE_ENV'] !== "production")
      app.use(express.logger({ immediate: true, format: 'dev' }));
