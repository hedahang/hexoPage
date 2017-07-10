---
title: 关于CSS hacks的使用
date: 2016-07-10 17:01:29
tags: 
	- CSS
---

#### 讨论CSS hacks，条件引用或者其他。

	background-color:#f1ee18;/*所有识别*/

	background-color:#00deff\9; /*IE6、7、8识别*/
	
	+background-color:#a200ff;/*IE6、7识别*/
	
	_background-color:#1e0bd1;/*IE6识别*/
	
	:root#test{background-color:purple\9;}:root是给ie9的，

	@media screen and (-webkit-min-device-pixel-ratio:0){
		.bb{background-color:#f1ee18}/* Safari(Chrome) 有效 */
	}{} 
	@media all and (min-width: 0px){ 
		.bb{
			background-color:#f1ee18;/*opera and Safari(Chrome) and firefox*/ 
			background-color:#4cac70\0;/* 仅 Opera 有效 */ 
		}
	}{} 

	.bb, x:-moz-any-link, x:default{
		background-color:#4eff00;/*IE7、Firefox3.5及以下 识别 */
	} 
	@-moz-document url-prefix(){
		.bb{
			background-color:#4eff00;/*仅 Firefox 识别 */
		}
	} 
	* +html .bb{background-color:#a200ff;}/* 仅IE7 识别 */<!-- more -->	

	
	
	
	
	
	
	
	
	
	
	