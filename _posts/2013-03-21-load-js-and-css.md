---
layout: post
title: "动态加载CSS和JS"
description: ""
category: 
tags: [Javascript, CSS]
---
{% include JB/setup %}

GIS组件用到了`arcgis api`中的对象，而这些`api`文件太大，不好封装到组件里，所以需要动态加载`css`和`js`文件，以便部署到不同的服务器。

在查阅各种资料后发现，动态加载js文件的过程其实可以分为以下两种：同步和异步。

而在这个问题处理之前，需要大概明白这么一件事情：**浏览器如何加载HTML页面？**这是一个比较复杂的问题，各种浏览器的实现方式也不一样，所以`HTML`页面加载的流程也不完全相同，但整个流程是大致确定的。

关于`HTML`的加载过程，总体上来说，浏览器会从上往下边加载边解释，同时生成`DOM`对象，如果遇到外部链接，比如引入`js`文件，在IE中会开辟一个线程，主线程继续执行；在火狐中会阻塞，知道js文件下载解释执行完成。

总结来看(网上摘录，不保证正确)：
	
    浏览器加载显示html的顺序是按下面的顺序进行的：
    1、IE下载的顺序是从上到下，渲染的顺序也是从上到下，下载和渲染是同时进行的。
    2、在渲染到页面的某一部分时，其上面的所有部分都已经下载完成（并不是说所有相关联的元素都已经下载完）。
    3、如果遇到语义解释性的标签嵌入文件（JS脚本，CSS样式），那么此时IE的下载过程会启用单独连接进行下载。
    4、并且在下载后进行解析，解析过程中，停止页面所有往下元素的下载。
    5、样式表在下载完成后，将和以前下载的所有样式表一起进行解析，解析完成后，将对此前所有元素（含以前已经渲染的）重新进行渲染。
    6、JS、CSS中如有重定义，后定义函数将覆盖前定义函数。
    
那么如何动态加载css或js呢？

代码1:

     function addFile(URL,FileType){
	     var oHead = document.getElementsByTagName('HEAD').item(0);
	     var addheadfile;
	     if(FileType=="js"){
	     addheadfile= document.createElement("script");
	     addheadfile.type = "text/javascript";
	     addheadfile.src=URL;
     }else{
	     addheadfile= document.createElement("link");
	     addheadfile.type = "text/css";
	     addheadfile.rel="stylesheet";
	     addheadfile.rev = "stylesheet";
	     addheadfile.media = "screen";
	     addheadfile.href=URL;
	     }
	     oHead.appendChild( addheadfile);
     }

用法：
	
	addFile("css/index.css","css");
	addFile("js/index.js","js");
	
很典型的创建动态标签，经过测试发现，上述带码会**异步**执行，而且在不同浏览器中文件的加载顺序也是不同的。如果两个js直接有依赖关系，这样的加载方式是不行的。

代码2：

	var classcodes =[];
	window.Import={
	    LoadFileList:function(_files,succes){
	        var FileArray=[];
	        if(typeof _files==="object"){
	            FileArray=_files;
	        }else{
	            if(typeof _files==="string"){
	                FileArray=_files.split(",");
	            }
	        }
	        if(FileArray!=null && FileArray.length>0){
	            var LoadedCount=0;
	            for(var i=0;i< FileArray.length;i++){
	                loadFile(FileArray[i],function(){
	                    LoadedCount++;
	                    if(LoadedCount==FileArray.length){
	                        succes();
	                    }
	                })
	            }
	        }
	        function loadFile(url, success) {
	            if (!FileIsExt(classcodes,url)) {
	                var ThisType=GetFileType(url);
	                var fileObj=null;
	                if(ThisType==".js"){
	                    fileObj=document.createElement('script');
	                    fileObj.src = url;
	                    fileObj.type = "text/javascript";
	                }else if(ThisType==".css"){
	                    fileObj=document.createElement('link');
	                    fileObj.href = url;
	                    fileObj.type = "text/css";
	                    fileObj.rel="stylesheet";
	                }
	                success = success || function(){};
	                fileObj.onload = fileObj.onreadystatechange = function() {
	                    if (!this.readyState || 'loaded' === this.readyState || 'complete' === this.readyState) {
	                        success();
	                        classcodes.push(url)
	                    }
	                }
	                document.getElementsByTagName('head')[0].appendChild(fileObj);
	            }else{
	                success();
	            }
	        }
	        function GetFileType(url){
	            if(url!=null && url.length>0){
	                return url.substr(url.lastIndexOf(".")).toLowerCase();
	            }
	            return "";
	        }
	        function FileIsExt(FileArray,_url){
	            if(FileArray!=null && FileArray.length>0){
	                var len =FileArray.length;
	                for (var i = 0; i < len; i++) {
	                    if (FileArray[i] ==_url) {
	                       return true;
	                    }
	                }
	            }
	            return false;
	        }
		    }
		}
用法：

	Import.LoadFileList(cssfiles,function(){
		  // TODO 成功加载的函数 
	});	

这样加入了一个判断状态是否完成代码，可以这样来嵌套调用：

	Import.LoadFileList(cssfiles,function(){
		   Import.LoadFileList(jsfile,function(){
		 	  	 Import.LoadFileList(mainjs,function(){
				});
			})
		});
这样就实现了**同步**加载JS文件。

也可以使用`XMLHttpRequest` 或`ajax`来实现同步加载，实现过程由各位自行脑补。

*------------------------无耻的分隔符----------------------------------*

**引申：如何判断文件加载完成？**

[CSS文件动态加载（续）—— 残暴的本相](http://www.binvor.com/a/xinxizhongxin/wangluokaifajishu/2013/0303/14795.html)

[关于html和javascript在浏览器中的加载顺序问题的讨论（zz）](http://www.cnblogs.com/beyondstorm/archive/2008/09/17/1292940.html)

[html的加载过程](http://bbs.csdn.net/topics/80438092)

[动态加载外部css或js文件](http://www.xker.com/page/e2007/1220/42497.html)

[Javascript在页面加载时的执行顺序](http://dancewithnet.com/2007/03/22/order-of-execution-of-javascript-on-web/)

[判断一个变量是否定义的方法](http://www.cftea.com/c/2007/04/NEVLTYFPFJI7WDJL.asp)