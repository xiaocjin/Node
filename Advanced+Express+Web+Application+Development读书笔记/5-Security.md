#安全

##设置Passport
  * npm install passport --save

##采用Cucumber和Zombie.js进行测试
  * npm install -g cucumber
  * npm install zombie --save-dev
  * npm install grunt-cucumber --save-dev

##Https

##Express和Socket.io共享Session

###添加依赖
    npm install session.socket.io --save
    
###代码
    function SocketHandler(httpServer, sessionStore, cookieParser) {
      var socketIo = new Socket(httpServer)
      var sessionSockets = new SessionSockets(socketIo, sessionStore,
      cookieParser);
      sessionSockets.on('connection', function(err, socket, session) {
        subscriber.subscribe("issues");
        subscriber.subscribe("commits");
        subscriber.client.on("message", function (channel, message) {
          socket.broadcast.to(message.projectId)
          .emit(channel, JSON.parse(message));
        });
        socket.on('subscribe', function (data) {
          var user = session ? session.passport.user : null;
          if (!user) return;
          socket.join(data.channel);
          session.touch();
        });
      });
      sessionSockets.on('error', function() {
        logger.error(arguments);
      });
    };
    module.exports = SocketHandler;

##使用http头和helmet提升安全
    var express = require('express')
      , helmet = require('helmet');
    function Security(app) {
      if (process.env['NODE_ENV'] === "TEST" ||
        process.env['NODE_ENV'] === "COVERAGE") return;
      app.use(helmet.xframe());
      app.use(helmet.hsts());
      app.use(helmet.iexss());
      app.use(helmet.contentTypeOptions());
      app.use(helmet.cacheControl());
      app.use(express.csrf());
    };
    module.exports = Security;
