---
title: JavaScript代码收集
date: 2016-07-20 14:11:29
tags: 
	- JavaScript
---

## 1.根据userAgent判断是否手机
	
	if(/AppleWebKit.*Mobile/i.test(navigator.userAgent) || (/MIDP|SymbianOS|NOKIA|SAMSUNG|LG|NEC|TCL|Alcatel|BIRD|DBTEL|Dopod|PHILIPS|HAIER|LENOVO|MOT-|Nokia|SonyEricsson|SIE-|Amoi|ZTE/.test(navigator.userAgent))){
		if(window.location.href.indexOf("?mobile")<0){
			try{
				if(/Android|webOS|iPhone|iPod|BlackBerry/i.test(navigator.userAgent)){
					//移动端
				}else if(/iPad/i.test(navigator.userAgent)){
					//这里对ipad做了处理
				}else{
					//移动端
				}
			}catch(e){}
		}
	}else{
		//pc端
	}
	var ISWP = !!(navigator.userAgent.match(/Windows\sPhone/i));
<!-- more -->
## 2.this, constructor, prototype例子

	var Tinker = function(){
        this.elements = [];
    
    };
        Tinker.fn = Tinker.prototype = {
            constructor: Tinker,
            extend: function(obj){
                var p;
                for(p in obj){
                    this.constructor.prototype[p] = obj[p];//此处若看明白了, 那么前面的就理解了
                }
            }
        
        }
        Tinker.fn.extend({
           get: function(){
                var length = arguments.length,
                i = 0;
               for(; i < length; i++){
                   this.elements.push(arguments[i]); //此处若看明白了, 那么前面的就理解了
               }
               return this;//此处若看明白了, 那么前面的就理解了
           },
           each: function(fn){
                var i = 0,
                    length = this.elements.length;
                for(; i < length; i++){
                    fn.call(this.elements[i], i, this.elements[i]);
                }
                return this;//此处若看明白了, 那么前面的就理解了
           }
            
        });
## 3.模仿jquery each		
	
	function each(obj, fn){
        var i;
        if(Object.prototype.toString.call(obj) === '[object Array]'){
            for(i = 0, length = obj.length; i < length; i++){
                fn.call(obj[i], i, obj[i]);
            }
        }
        else if(typeof obj === 'object'){
            for(i in obj){
                if(obj.hasOwnProperty(i)){
                   fn.call(obj[i], i, obj[i]);
                }
            }
        }
        else{
            return false;
        }
    }
		
