#实时交互

##使用redis缓存对象

###Redis
    var redis = require('redis')
      , config = require('../configuration');
    function Redis() {
      this.port = config.get("redis:port");
      this.host = config.get("redis:host");
      this.password = config.get("redis:password");
      this.client = redis.createClient(this.port, this.host);
      if (this.password) this.client.auth(this.password,
        function() {});
    }
    module.exports = Redis;

###Publisher
    var Redis = require('../../cache/redis')
      , util = require('util');
    util.inherits(Publisher, Redis);
    function Publisher() {
      Redis.apply(this, arguments);
    };
    Redis.prototype.save = function(key, items) {
      this.client.set(key, JSON.stringify(items));
    };
    Redis.prototype.publish = function(key, items) {
      this.client.publish(key, JSON.stringify(items));
    };
    module.exports = Publisher;

###Subscriber
    var Redis = require('../../cache/redis')
      , util = require('util');
    util.inherits(Subscriber, Redis);
    function Subscriber() {
      Redis.apply(this, arguments);
    };
    Subscriber.prototype.subscribe = function(key) {
      this.client.subscribe(key);
    };
    module.exports = Subscriber;

##Socket.IO

###Schedule
  * npm install node-schedule --save
  
