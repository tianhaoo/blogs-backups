---
title: ffmpeg-web-video
date: 2018-04-02 22:59:02
categories:
- 多媒体
tags:
    - ffmpeg
    - websocket
    - nodejs
---

## 本例程将会完成将摄像头接收的画面实时的呈现在网页上面的效果

## 测试摄像头是否可用

### 测试静态画面

#### 001.jpg应该是是摄像头应该捕捉的静态画面

`sudo apt1 install fswebcam`

```
cd /dev/
ls video*
```
<!--more-->

`fswebcam --device /dev/video0 ~/图片/001.jpeg`


### 测试动态画面

#### 安装ffmpeg

```
sudo add-apt-repository ppa:djcj/hybrid  
sudo apt-get update  
sudo apt-get install ffmpeg  
```

#### 创建一个配置文件，其中写入以下信息，并记住该文件位置

```
HTTPPort 8090
HTTPBindAddress 0.0.0.0
MaxHTTPConnections 100
MaxClients 10
MaxBandwidth 7000
CustomLog -

######### Definition of the live feeds #########
<Feed feed1.ffm>
File /tmp/feed1.ffm
FileMaxSize 200K
ACL allow 192.168.43.0 192.168.43.255
</Feed>

######### Definition of the stream #########
<Stream test1.mpg>
Feed feed1.ffm
Format mpeg
VideoBitRate 200
VideoBufferSize 40
VideoFrameRate 25
VideoSize 320x240
VideoGopSize 12
VideoCodec mpeg1video
NoAudio
ACL allow 192.168.0.0 192.168.255.255
</Stream>

######### stat.html #########
<Stream stat.html>
Format status
ACL allow 192.168.0.0 192.168.255.255
ACL allow 127.0.0.1
</Stream>

```

#### 以指定配置文件启动ffserver`ffserver -f ~/ffserver.conf`

#### 此时进入`localhost：8090/stat.html`网址应见到状态信息

#### 输入命令`ffmpeg -i /dev/video0 http://localhost:8090/feed1.ffm`让ffserver接收摄像头的数据

#### 输入命令`ffplay -v info -i http://localhost:8090/test1.mpg`可见对话框内出现摄像头的动态画面。

*所见的画面是ffmpeg经过转码将摄像头的数据传到`http://localhost:8090/feed1.ffm`，ffserver再将feed1转为test1，然后由ffplay在窗口播放*

### 在网页上播放视频

#### 首先得有一个播放视频的网页

```
<!DOCTYPE HTML>
<html>
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <script src="jsmpeg.min.js"></script>
    <title>title</title>
</head>

<body>

<h1>Demo</h1>

<canvas id="video-canvas"></canvas>
<script type="text/javascript">
    var canvas = document.getElementById("video-canvas");
    var url = "ws://" + document.location.hostname + ":8082/";
    var player = new JSMpeg.Player(url, {canvas: canvas});
</script>

</body>

</html>

```

#### 接着我们需要用到websocket，新建一个npm项目

```
npm init
npm install ws@2.3.1
```

#### 将必要的配置写入其中的websocket-relay.js文件

```
// Use the websocket-relay to serve a raw MPEG-TS over WebSockets. You can use
// ffmpeg to feed the relay. ffmpeg -> websocket-relay -> browser
// Example:
// node websocket-relay yoursecret 8081 8082
// ffmpeg -i <some input> -f mpegts http://localhost:8081/yoursecret

var fs = require('fs'),
    http = require('http'),
    WebSocket = require('ws');

if (process.argv.length < 3) {
    console.log(
        'Usage: \n' +
        'node websocket-relay.js <secret> [<stream-port> <websocket-port>]'
    );
    process.exit();
}

var STREAM_SECRET = process.argv[2],
    STREAM_PORT = process.argv[3] || 8081,
    WEBSOCKET_PORT = process.argv[4] || 8082,
    RECORD_STREAM = false;

// Websocket Server
var socketServer = new WebSocket.Server({port: WEBSOCKET_PORT, perMessageDeflate: false});
socketServer.connectionCount = 0;
socketServer.on('connection', function(socket) {
    socketServer.connectionCount++;
    console.log(
        'New WebSocket Connection: ',
        socket.upgradeReq.socket.remoteAddress,
        socket.upgradeReq.headers['user-agent'],
        '('+socketServer.connectionCount+' total)'
    );
    socket.on('close', function(code, message){
        socketServer.connectionCount--;
        console.log(
            'Disconnected WebSocket ('+socketServer.connectionCount+' total)'
        );
    });
});
socketServer.broadcast = function(data) {
    socketServer.clients.forEach(function each(client) {
        if (client.readyState === WebSocket.OPEN) {
            client.send(data);
        }
    });
};

// HTTP Server to accept incomming MPEG-TS Stream from ffmpeg
var streamServer = http.createServer( function(request, response) {
    var params = request.url.substr(1).split('/');

    if (params[0] !== STREAM_SECRET) {
        console.log(
            'Failed Stream Connection: '+ request.socket.remoteAddress + ':' +
            request.socket.remotePort + ' - wrong secret.'
        );
        response.end();
    }

    response.connection.setTimeout(0);
    console.log(
        'Stream Connected: ' +
        request.socket.remoteAddress + ':' +
        request.socket.remotePort
    );
    request.on('data', function(data){
        socketServer.broadcast(data);
        if (request.socket.recording) {
            request.socket.recording.write(data);
        }
    });
    request.on('end',function(){
        console.log('close');
        if (request.socket.recording) {
            request.socket.recording.close();
        }
    });

    // Record the stream to a local file?
    if (RECORD_STREAM) {
        var path = 'recordings/' + Date.now() + '.ts';
        request.socket.recording = fs.createWriteStream(path);
    }
}).listen(STREAM_PORT);

console.log('Listening for incomming MPEG-TS Stream on http://127.0.0.1:'+STREAM_PORT+'/<secret>');
console.log('Awaiting WebSocket connections on ws://127.0.0.1:'+WEBSOCKET_PORT+'/');

```


#### 运行它`node websocket-relay.js <secret>`

#### 运行如下命令

```
ffmpeg -i http://localhost:8090/test1.mpg -f mpegts -vcodec mpeg1video http://localhost:8081/<secret>
```

*这句话是将我们之前转化好的test1.mpg利用ffmpeg转码给刚开的websocket服务器， 他会负责转化test1.mpg为jsmpeg需要的视频文件，最后由网页负责播放*




