#延展
##使用Redis共享Express Session
    Redis = require('../cache/redis')
      , redis = new Redis()
      , RedisStore = require('connect-redis')(express);
    var sessionStore = new RedisStore({client: redis.client});

##使用Redis延展Socket.io
    var config = require('../configuration')
      , RedisStore = require('socket.io/lib/stores/redis')
      , redis = require('socket.io/node_modules/redis')
      , Redis = require('../cache/redis')
      , pub = new Redis().client
      , sub = new Redis().client
      , client = new Redis().client;
    
    function Socket(server) {
      /....
      socketio.set('store', new RedisStore({
      redis : redis
      , redisPub : pub
      , redisSub : sub
      , redisClient : client
      }));
      return socketio;
    };

##水平延展Express工程
    工程模块化（core、api、web……）

##使用集群进行垂直拓展
    var cluster = require('cluster')
      , http = require('http')
      , numCPUs = require('os').cpus().length
      , logger = require('../logger');
    function Cluster() {}
    Cluster.prototype.run = function(module){
      if (cluster.isMaster) {
        for (var i = 0; i < numCPUs; i++) {
        cluster.fork();
      }
      cluster.on('exit', function(worker, code, signal) {
        logger.info('Worker ' + worker.process.pid + ' died');
      });
      } else {
        require(module);
      }
    }
    module.exports = Cluster;


##使用Hipache进行负载均衡
  * npm install hipache -g
  
