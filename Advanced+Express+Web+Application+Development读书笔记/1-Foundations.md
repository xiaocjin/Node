#Foundations
##Installation
  npm install -g express
##package.json
  	{
        "name":"",
        "version":"",
        "private":"",
        "scripts":{
          "start":"node app.js"
        },
        "dependences":{
          "express":"3.x"
        }
  	}
##使用Mocha和SuperTest测试
  * npm install -g mocha –-save-dev
  * npm install supertest –-save-dev

##JSCoverage测试覆盖率
  * [JSCoverage](http://siliconforks.com/jscoverage/)

##使用Nconf配置express
### npm install nconf --save
  
    var nconf = require('nconf');
        function Config(){
          nconf.argv().env("_");
          var environment = nconf.get("NODE:ENV") || "development";     
          nconf.file(environment, "config/" + environment + ".json");    
          nconf.file("default", "config/default.json");
        }
    
    	Config.prototype.get = function(key) {
     		 return nconf.get(key);
   	 };
    
   	 module.exports = new Config();

##处理404

    exports.index = function(req, res, next){
      res.json(404, 'Not Found.');
    };
    
    var express = require('express')
      , http = require('http')
      , notFound = require('../middleware/notFound')
      , app = express();
    app.set('port', config.get('express:port'));
    app.get('/heartbeat', heartbeat.index);
    app.use(notFound.index);
    http.createServer(app).listen(app.get('port'));
    module.exports = app;

##使用Winston处理log
    var winston = require('winston')
      , config = require('../configuration');
    function Logger(){
      return winston.add(winston.transports.File, {
        filename: config.get('logger:filename'),
        maxsize: 1048576,
        maxFiles: 3,
        level: config.get('logger:level')
      });
    }
    module.exports = new Logger();

##使用Grunt
* npm install -g grunt-cli
* npm install grunt –-save-dev
* npm install grunt-cafe-mocha –-save-dev
* npm install grunt-jscoverage –-save-dev
* npm install grunt-env –-save-dev

###gruntfile.js
    module.exports = function(grunt) {
      grunt.loadNpmTasks('grunt-jscoverage');
      grunt.loadNpmTasks('grunt-cafe-mocha');
      grunt.loadNpmTasks('grunt-env');
      grunt.initConfig({
        env: {
          test: { NODE_ENV: 'TEST' },
          coverage: { NODE_ENV: 'COVERAGE' }
        },
        cafemocha: {
          test: {
          src: 'test/*.js',
          options: {
            ui: 'bdd',
            reporter: 'spec',
          },
        },
        coverage: {
          src: 'test/*.js',
          options: {
            ui: 'bdd',
            reporter: 'html-cov',
            coverage: {
              output: 'coverage.html'
            }
          }
        },
      },
      jscoverage: {
        options: {
          inputDirectory: 'lib',
          outputDirectory: 'lib-cov',
          highlight: false
        }
      }
      });
      grunt.registerTask('test', [ 'env:test', 'cafemocha:test' ]);
      grunt.registerTask('coverage', [ 'env:coverage',
        'jscoverage', 'cafemocha:coverage' ]);
    };
  
