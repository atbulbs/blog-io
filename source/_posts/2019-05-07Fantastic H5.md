---
title: Fantastic H5
date: 2019-05-07
tags: HTML5
categories: HTML5
---

# Devices and Sensors 设备和传感器相关
  官方文档: https://www.w3.org/das/

## Vibration 震动
  官方文档: https://www.w3.org/TR/vibration/
  参考文档: https://www.sitepoint.com/the-buzz-about-the-vibration-api/

### 应用场景
  * 增强用户交互体验, 如游戏, 提醒

### 兼容性
  compatibility: https://developer.mozilla.org/en-US/docs/Web/API/Navigator/vibrate
  亲测微信小程序webview支持
### demo
  https://www.audero.it/demo/vibration-api-demo.html

```javascript
window.navigator.vibrate(1000)
window.navigator.vibrate([1000, 1000, 2000, 1000, 3000])
// 取消震动
window.navigator.vibrate(0)
window.navigator.vibrate([])
window.navigator.vibrate([0])
```

## Battery Status 电池状态
  官方文档: https://www.w3.org/TR/battery-status/

### 应用场景
  * 根据电池状态做相应节能处理, 改善用户体验, 延长电池寿命
  * 在电池耗尽之前保存关键数据, 防止数据丢失

### 兼容性
  compatibility: https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getBattery#Browser_compatibility
  亲测微信小程序webview支持

<!-- more -->

### demo 
```html
<!DOCTYPE html>
<html>
<head>
  <title>Battery Status API Example</title>
  <script>
    window.onload = function () {
      function updateBatteryStatus(battery) {
        document.querySelector('#charging').textContent = battery.charging ? 'charging' : 'not charging';
        document.querySelector('#level').textContent = battery.level;
        document.querySelector('#dischargingTime').textContent = battery.dischargingTime / 60;
      }

      navigator.getBattery().then(function(battery) {
        // Update the battery status initially when the promise resolves ...
        updateBatteryStatus(battery);

        // .. and for any subsequent updates.
        battery.onchargingchange = function () {
          updateBatteryStatus(battery);
        };

        battery.onlevelchange = function () {
          updateBatteryStatus(battery);
        };

        battery.ondischargingtimechange = function () {
          updateBatteryStatus(battery);
        };
      });
    };
  </script>
</head>
<body>
  <div id="charging">(charging state unknown)</div>
  <div id="level">(battery level unknown)</div>
  <div id="dischargingTime">(discharging time unknown)</div>
</body>
</html>
```

## Pointer Lock 鼠标锁定
  官方文档: https://w3c.github.io/pointerlock/

### 应用场景
  * 地图, 游戏, 全景视频中, 让用户不断地移动鼠标来持续旋转或操纵一个3D模型
  * 不用担心指针到达浏览器或者屏幕边缘的那一刻停止

### 兼容性
  compatibility: https://developer.mozilla.org/en-US/docs/Web/API/Pointer_Lock_API#Browser_compatibility

### demo
  https://www.zhangxinxu.com/study/201709/js-pointer-lock-demo.html
```javascript
var eleImage = document.getElementById('image')
eleImage.requestPointerLock()
document.exitPointerLock()
```

## deviceorientation 设备物理方向 与 devicemotion 设备加速信息
  官方文档: https://www.w3.org/html/ig/zh/wiki/DeviceOrientation%E4%BA%8B%E4%BB%B6%E8%A7%84%E8%8C%83
  参考文档: https://imweb.io/topic/56ab279be39ca21162ae6c75

### 应用场景
  * 摇一摇
  * 体感游戏
  * 罗盘

### 兼容性
  compatibility: https://developer.mozilla.org/en-US/docs/Web/API/Window/devicemotion_event#Browser_compatibility
  compatibility: https://developer.mozilla.org/en-US/docs/Web/API/Window/deviceorientation_event#Browser_compatibility

### demo
```
if (window.DeviceMotionEvent) {
    window.addEventListener('devicemotion', shakeEventHandler, false);
} else {
    alert('本设备不支持devicemotion事件');
}

var THRESHOLD = 1000;
var preX = preY = preZ = x = y = z = 0;
var preTime = 0;

function shakeEventHandler(event) {
    var acceleration = event.accelerationIncludingGravity; 
    var curTime = new Date().getTime();
    var diffTime = curTime-preTime;

    if (diffTime > 100) { 
        preTime = curTime;  
        x = acceleration.x;  
        y = acceleration.y;  
        z = acceleration.z;  

        var accelerationDiff = Math.abs(x + y + z - preX - preY - preZ) / diffTime * 10000;  

        if (accelerationDiff > THRESHOLD) {  
            alert("摇一摇有惊喜！");  
        }  
        preX = x;  
        preY = y;  
        preZ = z;  
    }  
}
```

## Ambient Light 环境光
  官方文档: https://www.w3.org/TR/ambient-light/
  官方文档: https://developer.mozilla.org/en-US/docs/Web/API/Window/ondevicelight

### 应用场景
  * 阅读时的自适应背景光

### 兼容性
  compatibility: https://developer.mozilla.org/en-US/docs/Web/API/Window/ondevicelight#Browser_compatibility

### demo
```javascript
var sensor = new AmbientLightSensor();
sensor.addEventListener("reading", function (event) {
  console.log(sensor.illuminance)
})
```

# Web Performance 性能相关
  官方文档: https://www.w3.org/webperf/
  git仓库地址: https://github.com/w3c/web-performance/

## Page Visibility Level 2 页面可见性
  官方文档: https://www.w3.org/TR/page-visibility/
  参考文档: https://www.sitepoint.com/introduction-to-page-visibility-api/

### 应用场景
  * 页面切到后台时的一些处理, 如暂停音视频, 暂停刷新画布, 减慢甚至停止CPU, 带宽消耗等

### 兼容性
  compatibility: https://developer.mozilla.org/en-US/docs/Web/API/Page_Visibility_API

### demo
```javascript
// Set the name of the hidden property and the change event for visibility
var hidden, visibilityChange; 
if (typeof document.hidden !== "undefined") { // Opera 12.10 and Firefox 18 and later support 
  hidden = "hidden";
  visibilityChange = "visibilitychange";
} else if (typeof document.msHidden !== "undefined") {
  hidden = "msHidden";
  visibilityChange = "msvisibilitychange";
} else if (typeof document.webkitHidden !== "undefined") {
  hidden = "webkitHidden";
  visibilityChange = "webkitvisibilitychange";
}

// Warn if the browser doesn't support addEventListener or the Page Visibility API
if (typeof document.addEventListener === "undefined" || hidden === undefined) {
  console.log("This demo requires a browser, such as Google Chrome or Firefox, that supports the Page Visibility API.");
  // 此处可以根据 setInterval 和 requestAnimationFrame 切到后台时的不同表现来hack
} else {
  // Handle page visibility change   
  document.addEventListener(visibilityChange, handleVisibilityChange, false);
}
```

## High Resolution Time Level 2 微秒级精度的时间
  官方文档: https://www.w3.org/TR/hr-time/
  参考文档: https://blogs.msdn.microsoft.com/ie_cn/2012/10/30/web-3/

### 应用场景
  * 同步动画和音视频, 用户计时, 监测页面性能时有更高的精度
  * 后续调用时间只差不会为负数或0, 单调递增, Date.now()无法保证这一点
  * 起点为文档导航启动的时间, 不受系统时间影响, 更好的做计时任务
  * 嵌套iframe时, 可以统一参考根文档导航启动的时间

### 兼容性
  compatibility: https://developer.mozilla.org/en-US/docs/Web/API/Performance/now#Browser_compatibility
  支持此API的浏览器是IE10，Opera 15, Safari iOS 9和Firefox 15+，不带前缀。Chrome 20+ 需“webkit”前缀，24+不需要前缀。

### demo
```javascript
performance.now()
// 根文档导航启动的时间
// top === window // true
top.performance.now()
```

## User Timing Level 2 用户时序
  官方文档: https://www.w3.org/TR/user-timing/
  参考文档: https://www.sitepoint.com/discovering-user-timing-api/

### 应用场景
  * 测试代码性能, 标记时间戳和测量两个标记之间经过的时间, 无需声明变量和产生额外的性能开销

### 兼容性
  compatibility: https://developer.mozilla.org/en-US/docs/Web/API/Performance/mark#Browser_compatibility

### demo
```javascript
performance.mark("startFoo");
// A time consuming function
foo();
performance.mark("endFoo");

performance.measure("durationFoo", "startFoo", "endFoo");

// Delete all Marks
performance.clearMarks();
// Delete the Measure "durationFoo"
performance.clearMeasure("durationFoo");

```

## Navigation Timing Level 2 导航时序
  官方文档: https://w3c.github.io/navigation-timing/
  参考文档: https://www.sitepoint.com/profiling-page-loads-with-the-navigation-timing-api/

### 应用场景
  * 获取DNS查找, TCP建立连接, 页面重定向, 构建DOM等消耗的时间
  ![timing_overview](/images/h5_api/timing_overview.png)

### 兼容性
  compatibility: https://caniuse.com/#feat=nav-timing

### demo
```javascript
window.addEventListener('load', function() {
  // 确保加载事件结束
  setTimeout(function () {
    var timing = window.performance.timing
    // 用户经历的总页面加载时间
    var userTime = timing.loadEventEnd - timing.navigationStart
    // DNS查找时间
    var dns = timing.domainLookupEnd - timing.domainLookupStart
    // 连接到服务器花费的时间
    var connection = timing.connectEnd - timing.connectStart
    // 请求发送到服务器并接受响应花费的时间
    var requestTime = timing.responseEnd - timing.requestStart
    // 文档获取(包括访问缓存)的时间
    var fetchTime = timing.responseEnd - timing.fetchStart
  }, 0)
}, false)

// or in console
// Get the first entry
const [entry] = performance.getEntriesByType('navigation')
// Show it in a nice table in the developer console
console.table(entry.toJSON())
```

## The Network Information 网络信息
  官方文档: https://www.w3.org/TR/netinfo-api/
  参考文档: https://www.sitepoint.com/use-network-information-api-improve-responsive-websites/

### 应用场景
  * 评估带宽, 判断网络连接是否受限
  * 根据网络信息响应式的加载高分辨率图片, 不必要的字体, 增加Ajax的timeout参数

### 兼容性
  compatibility: https://developer.mozilla.org/en-US/docs/Web/API/Navigator/connection

### demo
```javascript
console.table(navigator.connection)
// downlink: 3.4
// effectiveType: "3g"
// onchange: null
// rtt: 350
// saveData: false
```

## online and offline 在线和离线

### 应用场景
  * 监测用户在线和离线

### demo
```javascript
var condition = navigator.onLine ? "online" : "offline"
window.addEventListener('online',  updateOnlineStatus)
window.addEventListener('offline', updateOnlineStatus)
```

## 信号浮标 Beacon 

### 应用场景
  * 页面卸载完毕前异步发送数据, 不延迟当前页面的卸载以及下一个页面的载入

### 兼容性
  compatibility: https://developer.mozilla.org/en-US/docs/Web/API/Navigator/sendBeacon#Browser_compatibility

### demo
```javascript
window.addEventListener("unload", logData, false);

function logData() {
  navigator.sendBeacon("/log", analyticsData);
}

```

# Style 样式相关
  官方文档: https://www.w3.org/Style/CSS/

## Fullscreen 全屏
  官方文档: https://www.w3.org/TR/fullscreen/
  参考文档: https://www.sitepoint.com/use-html5-full-screen-api//

### 应用场景
  * 全屏展示某元素

### 兼容性
  compatibility: https://developer.mozilla.org/en-US/docs/Web/API/Element/requestFullScreen#Browser_compatibility

### demo
  http://blogs.sitepointstatic.com/examples/tech/full-screen/index2.html
```javascript
// 让某元素全屏
document.getElementById("myImage").requestFullscreen();
document.addEventListener("fullscreenchange", FShandler);
document.addEventListener("webkitfullscreenchange", FShandler);
document.addEventListener("mozfullscreenchange", FShandler);
document.addEventListener("MSFullscreenChange", FShandler);
// 退出全屏
document.exitFullscreen()
```
```css
/* 全屏样式 */
:-webkit-full-screen {
}

:-moz-full-screen {
}

:-ms-fullscreen {
}

:fullscreen {
}
```

# Multimedia 多媒体相关

## AudioContext 音频上下文
  官方文档: https://www.w3.org/TR/webaudio/

### 应用场景
  * 解决Audio标签受部分浏览器策略无法自动播放和调整音量的问题
  * 微信浏览器里可以放在wx.ready callbackFn里调用
  * 音频处理, 音频图形化
  * 模拟乐器如钢琴打碟APP, 汤姆猫APP

### 兼容性
  compatibility: https://developer.mozilla.org/en-US/docs/Web/API/AudioContext#Browser_compatibility

### demo
  https://codepen.io/leechikit/embed/vJxewz
```javascript
// 音頻地址
const SONG1 = 'https://leechikit.github.io/resources/article/AudioContext/song/fingfingxia1.mp3';
// AudioContext对象
let audioContext = null;
// 音频源
let bufferSource = null;
// 音量模块
let gainNode = null;
// 是否播放
let isStart = false;
// 播放按钮元素
let buttonEl = document.querySelector('#button1');
// 音量控件元素
let volumnEl = document.querySelector('#volumn1');

/**
* 创建AudioContext上下文
*
*/
function createAudioContext() {
    try {
        audioContext = new (window.AudioContext || window.webkitAudioContext)();
    } catch (e) {
        alert("Web Audio API is not supported in this browser");
    }
}

/**
* 解码音频文件
*
*/
function decodeAudioData(url, callback) {
    let request = new XMLHttpRequest();
    request.open('GET', url, true);
    request.responseType = 'arraybuffer';
    request.onload = () => {
        audioContext.decodeAudioData(request.response, (buffer) => {
            if (!buffer) {
                alert('error decoding file data: ' + url);
                return;
            } else {
                callback(buffer);
            }
        })
    }
    request.onerror = ()=> {
        alert('BufferLoader: XHR error');
    }
    request.send();
}

/**
* 创建AudioBufferSourceNode
*
*/
function createBufferSource(config) {
    bufferSource = audioContext.createBufferSource();
    bufferSource.buffer = config.buffer;
    bufferSource.loop = config.loop || false;
    bufferSource.onended = () => {
        bufferSource = null;
    }
}

/**
* 创建GainNode对象
*
*/
function createGainNode() {
    gainNode = audioContext.createGain();
}

/**
* 点击播放按钮
*
*/
function buttonClickEvent(buffer) {
    buttonEl.addEventListener('click', (event) => {
        // 停止播放
        if (isStart) {
            event.target.innerText = 'START';
            bufferSource && bufferSource.stop();
            // 开始播放
        } else {
            event.target.innerText = 'STOP';
            createBufferSource({
                buffer,
                loop: true
            });
            createGainNode();
            bufferSource.connect(gainNode);
            gainNode.connect(audioContext.destination);
            bufferSource.start();
        }
        isStart = !isStart;
    });
}

/**
* 改变音量事件
*
*/
function changeVolumnEvent() {
    volumnEl.addEventListener('change', (event) => {
        gainNode && (gainNode.gain.value = event.target.value / 50);
    });
}

/**
* 初始化
*
*/
function init() {
    createAudioContext();
    decodeAudioData(SONG1, (buffer) => {
        buttonEl.setAttribute('data-loaded', true);
        buttonClickEvent(buffer);
        changeVolumnEvent();
    });
}

init();
```

## Media Capture and Streams 媒体捕获和流
  官方文档: https://w3c.github.io/mediacapture-main/getusermedia.html
  官方文档: https://www.w3.org/2011/04/webrtc/
  参考文档: https://www.sitepoint.com/introduction-getusermedia-api/

### 应用场景
  * WebRTC, 实时直播
  * 拍照自定义头像
  * 视频聊天
  * 抖音
  * 美拍

### 兼容性
  compatibility: https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia#Browser_compatibility

### demo
  https://www.audero.it/demo/getusermedia-api-demo.html
```javascript
var video = document.querySelector('#video')
var myConstraints = {
  video: {
    facingMode: 'user' // 优先调用前置摄像头
  }
}
navigator.mediaDevices.getUserMedia(myConstraints).then((stream) => {
  video.src = window.URL.createObjectURL(stream)
  video.play()
}, (error) => {
  console.error(error.name || error)
})

```

## Web Speech web语音
  官方文档: https://www.w3.org/community/speech-api/
  官方文档: https://w3c.github.io/speech-api/speechapi.html#tts-section
  参考文档: https://www.sitepoint.com/talking-web-pages-and-the-speech-synthesis-api/

## 应用场景
  * 无障碍阅读
  * 语音控制
  * 游戏分数, 天气, 股市, 新闻播报

### 兼容性
  compatibility: https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis#Browser_compatibility

### demo
  https://codepen.io/matt-west/pen/wGzuJ
```javascript
// Create the utterance object
// 语音合成
var utterance = new SpeechSynthesisUtterance();
utterance.text = 'My name is Dino';
utterance.lang = 'it-IT';
utterance.rate = 1.2;
utterance.onend = function() {
  window.speechSynthesis.speak(utterance);
});
```

## Web Notification
  官方文档: https://www.w3.org/TR/notifications/
  参考文档: https://www.zhangxinxu.com/wordpress/2016/07/know-html5-web-notification/

### 应用场景:
  * 消息推送, 聊天
  
### 兼容性
  compatibility: https://developer.mozilla.org/en-US/docs/Web/API/notification/Notification#Browser_compatibility
  
### demo
  https://www.zhangxinxu.com/study/201607/web-notifications.html

# Semantics 语义相关

## MATHML 数学标签
  官方文档: https://www.w3.org/Math/
  官方文档: https://w3c.github.io/mathml/mathml.html

### 应用场景
  * 数学公式的展示

### 兼容性
  compatibility: https://developer.mozilla.org/en-US/docs/Web/MathML#Browser_compatibility

### demo
  ![mathml](/images/h5_api/mathml.png)
```javascript
<mrow>
  <mi>x</mi>
  <mo>=</mo>
  <mfrac>
    <mrow>
      <mrow>
        <mo>-</mo>
        <mi>b</mi>
      </mrow>
      <mo>&#xB1;<!--plus-minus sign--></mo>
      <msqrt>
        <mrow>
          <msup>
            <mi>b</mi>
            <mn>2</mn>
          </msup>
          <mo>-</mo>
          <mrow>
            <mn>4</mn>
            <mo>&#x2062;<!--invisible times--></mo>
            <mi>a</mi>
            <mo>&#x2062;<!--invisible times--></mo>
            <mi>c</mi>
          </mrow>
        </mrow>
      </msqrt>
    </mrow>
    <mrow>
      <mn>2</mn>
      <mo>&#x2062;<!--invisible times--></mo>
      <mi>a</mi>
    </mrow>
  </mfrac>
</mrow>
```

### 兼容性
  compatibility: https://developer.mozilla.org/en-US/docs/Web/API/NavigatorOnLine#Browser_compatibility

# 本文参考的其他文档
  * https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5
  * https://www.sitepoint.com/10-html5-apis-worth-looking/
  * https://github.com/AurelioDeRosa/HTML5-API-demos
  * https://whatwebcando.today/
