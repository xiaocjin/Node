## 简介

>Channel主要用于需要广播推送消息的场景。可以把某个玩家加入到一个Channel中，当对这个Channel推送消息的时候，所有加入到这个Channel的玩家都会收到推送过来的消息。

## 特点

- 用户和Channel是多对多的关系。
- Channel都是服务器本地的，服务器A上创建的Channel，只能由服务器A才能给它推送消息。

## API

- add
- leave
- getMembers
- getMember
- destroy
- pushMessage
