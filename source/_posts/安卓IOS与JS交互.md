---
title: 关于IOS、安卓与原生JS交互的总结
date: 2016-11-03 10:08:06
categories: JS
tags:
	- JS
	- IOS
	- Android
---

最近在App中需要做一些页面，其中涉及到了与原生APP交互的问题，现在做下总结方便以后查询

**Android与JS交互**
``` javascript
<script>
var browser={
    versions:function(){
        var u = navigator.userAgent, app = navigator.appVersion;
        return {//移动终端浏览器版本信息
            trident: u.indexOf('Trident') > -1, //IE内核
            presto: u.indexOf('Presto') > -1, //opera内核
            webKit: u.indexOf('AppleWebKit') > -1, //苹果、谷歌内核
            gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1, //火狐内核
            mobile: !!u.match(/AppleWebKit.*Mobile.*/)||!!u.match(/AppleWebKit/), //是否为移动终端
            ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), //ios终端
            android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1, //android终端或者uc浏览器
            iPhone: u.indexOf('iPhone') > -1 || u.indexOf('Mac') > -1, //是否为iPhone或者QQHD浏览器
            iPad: u.indexOf('iPad') > -1, //是否iPad
            webApp: u.indexOf('Safari') == -1 //是否web应该程序，没有头部与底部
        };
    }(),
    language:(navigator.browserLanguage || navigator.language).toLowerCase()
}
function showToast() {
android.showToast("'title: Money+Funny，这可能是全成都最适合你的兼职！','content:拼K高校首席体验官公开招募！底薪1000元多玩多赚！速来！速来！速来！','urlId:http://www.tanshikeji.com/Home/Active/special_form.html'");
//    android.showToast("{\"title\":\" 我报名了拼K高校首席体验官！ 你也快来！\",\"content\":\"拼K高校首席体验官公开招募！底薪1000元多玩多赚！速来！速来！速来！\",\"urlId:http://www.tanshikeji.com/Home/Active/special\"}");
}
function showClick(e) {
   if( navigator.userAgent.indexOf('Android') > -1 ||  navigator.userAgent.indexOf('Linux') > -1){
//        e.preventDefault();
    }
    android.showClick("http://www.tanshikeji.com/Home/Active/special_form.html");
}
document.getElementById('android_share').onclick = showToast;
document.getElementById('from_click').onclick = showClick;
</script>
```
<!-- more -->
**IOS与JS交互:**
``` javascript
<script>
/*这段代码是固定的，必须要放到js中*/
function setupWebViewJavascriptBridge(callback) {
    if (window.WebViewJavascriptBridge) { return callback(WebViewJavascriptBridge); }
    if (window.WVJBCallbacks) { return window.WVJBCallbacks.push(callback); }
    window.WVJBCallbacks = [callback];
    var WVJBIframe = document.createElement('iframe');
    WVJBIframe.style.display = 'none';
    WVJBIframe.src = 'wvjbscheme://__BRIDGE_LOADED__';
    document.documentElement.appendChild(WVJBIframe);
    setTimeout(function() { document.documentElement.removeChild(WVJBIframe) }, 0)
}
/*与OC交互的所有JS方法都要放在此处注册，才能调用通过JS调用OC或者让OC调用这里的JS*/
setupWebViewJavascriptBridge(function(bridge) {
    /*我们在这注册一个js调用OC的方法，不带参数，且不用ObjC端反馈结果给JS：打开本demo对应的博文*/
    document.getElementById('ios_share').onclick = function (e) {
        bridge.callHandler('getShareObjC', {
            'title': 'Money+Funny，这可能是全成都最适合你的兼职！',
            'content':'拼K高校首席体验官公开招募！底薪1000元多玩多赚！速来！速来！速来！',
            'urlId':'http://www.tanshikeji.com/Home/Active/special_form.html'
        }, function(response) {
//            alert("success"+response);
        })
    }
})
</script>
```


