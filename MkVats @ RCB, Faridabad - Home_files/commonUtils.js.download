/*$Id:$*/
// common load script 
var touch=false;
var istab=false;
var uagent = navigator.userAgent;
if(uagent.indexOf("iPhone")!=-1||uagent.indexOf("iPad")!=-1||uagent.indexOf("Mobile Safari")!=-1||uagent.indexOf("Nokia")!=-1|| uagent.indexOf("Fennec")!=-1||  uagent.indexOf("Opera Mini")!=-1||uagent.indexOf("IEMobile")!=-1) {
    touch=true;
}
var mobile =!!(uagent.match(/(iPhone|iPod|blackberry|Nexus|android 0.5|htc|lg|midp|mmp|mobile|nokia|opera mini|palm|pocket|psp|sgh|smartphone|symbian|treo mini|Playstation Portable|SonyEricsson|Samsung|MobileExplorer|PalmSource|Benq|Windows Phone|Windows Mobile|IEMobile|Windows CE|Nintendo Wii|SM-T\d+)/i));

if(uagent.indexOf("iPad")!=-1 || (mobile && uagent.indexOf("iPhone")==-1 && window.innerWidth>=768)){//for ipad and tab
    istab=true;
}

function v(a,b){
        for(var c=0,d;d=a[c];c++){
            if(b==d)
            return c;
        }
        return-1;
}
function commonLoadScript(src,elem){
	if(elem == "gallery"){
		picasaUserNameValid = false;
	}
    var script = document.createElement('script');
    if(this.addEventListener){  
        script.onload = function(){fnAfterLoadScript(src,elem);};
    }
    else{
    	script.onreadystatechange = function(){
        	if(v(["loaded","complete"],this.readyState)>-1){
            	this.onreadystatechange= null;
                fnAfterLoadScript(src,elem);
        	}
    	}
    }
    script.onerror = function(){fnErrorOnLoadScript(src,elem);}
    script.src =src;
    document.getElementsByTagName('head')[0].appendChild(script);
}
function fnAfterLoadScript(src,elem){
	if(elem == "tWidget"){
		enableTwitterWidget();
	}
	else if(elem == "tweetButton"){
		enableTwitterButton();
	}
	else if(elem =="loadTwitterJS"){
		fnloadTwitterJS();
	}
	else if (elem == "gallery" && !window.ZS_PublishMode){ // ie 8
		 if(!picasaUserNameValid){
			parent.fnShowGalleryErrMsg();
         }       
	}
	else if(elem  == "facebook"){
		enableFacebookWidget();
	}
	else if(elem == "loadFacebookJS"){
		commonLoadScript("//connect.facebook.net/en_GB/all.js#xfbml=1","facebook");
	/*}else if(elem == "setFormContextPath"){
		scriptLoaded = true;
        if(window.ZCApp){
		    window.ZCApp.contextPath = "/siteforms";// No I18N
            renderFromSave();
            //getForm(0);
            if(window.ZCForm){
                ZCForm.callbackFunc = function(formLinkName, formAccessType, paramsMap, infoMsg, errorMsg, succMsg, succMsgDuration){
                    if(!formLinkName){
                        return;
                    }
                    var formMsgEle = document.getElementById("formMsg_"+formLinkName);
                    formMsgEle.style.color = "green";
                    var successMsg = (succMsg==="zc_success")?"":fnAsString(succMsg);//NO I18N
                    formMsgEle.innerHTML = successMsg;
                    formMsgEle.style.display = "block";
                    setTimeout(function(){formMsgEle.style.display="none"}, 5000);
                    var formElem = fnGetElementByAttribute("elname", formLinkName, "form");//NO I18N
                    formElem.reset();
                    relodCurrentForm = false;
                }
                ZCApp.showErrorDialog = function(headerMsg, message){
                    var formMsgEle = document.getElementById("formMsg_"+ZohoForms.submittedForm);
                    formMsgEle.style.color = "red";
                    formMsgEle.innerHTML = fnAsString(message);
                    formMsgEle.style.display = "block";
                    setTimeout(function(){formMsgEle.style.display="none"}, 5000);
                }
            }
        }*/
	}else if(elem == "map"){
		Map.loadMap();
	}
	else if(elem == "gplus"){
		fnCreateGPlus();
	}
	else if(elem == "rendergplus"){
		fnLoadGPlusJS();
	}
    else if(elem == "linkedInButton"){
	commonLoadScript('/zs-site/assets/v0/js/linkedin.js', "loadLIFrameworkAPI"); //No I18N
	loadLIShareButton();
    }
    else if(elem == "liShareButton"){
        loadLIShareButton();
    }
    else if(elem == "loadLIJS"){
        commonLoadScript('/js/linkedin.js', "loadLIFrameworkNoAPI");    // No I18N
    }
    else if(elem == "loadLIFrameworkNoAPI"){
        loadLIFrameworkNoAPI();
    }
    else if(elem == "loadLIJSAPI"){
        commonLoadScript('/js/linkedin.js', "loadLIFrameworkAPI");  // No I18N
    }
    else if(elem == "loadLIFramework"){
        IN.init();
    }
    else if(elem == "loadLIFrameworkAPI"){
        initializeLIFramework();
	/*} else if(elem == "loadCreatorScripts") {
        loadCreatorScripts();*/
    } else if(elem == "audio"){
        loadAudioFiles();
    }
}

function fnErrorOnLoadScript(src,elem){
	if(elem == "gallery" && !parent.window.ZS_PublishMode){
		if(!picasaUserNameValid){
			parent.fnShowGalleryErrMsg();
        }
	} else if(elem == "loadCreatorScripts") {
		creatorJsLoaded = false;
	}
}

// get domain name 
function getDomainName(){
    var dn= "your-domain-name.com";// No I18N
    if(window.ZS_PublishMode){
        if(window.location.href.indexOf("/preview/")==-1){
            dn = window.location.href;
            /*This will cause issue when menu is hidden.
                         *var homePage = document.getElementById('nav-top').children[0].firstChild.href;
            if(dn == homePage){
                dn = dn.replace(/(h(t)tp:\/\/.+\/).+\.html/,"$1");
            }*/
        }
    }else if(window.ZS_PreviewMode){
        if(domainName.indexOf("NotPublish") == -1){
            dn = domainName;
        }
        dn = "ht"+"tp://"+dn+"/"+document.title+".html";//NO I18N
    }else{
        if(window.CurrentSiteData && CurrentSiteData.siteData.published){
            dn=CurrentSiteData.siteData.domainName;
        }
        dn = "ht"+"tp://"+dn+"/"+pageUrl+".html";//NO I18N
    }
    return dn;
}

// get browser language 
function getBrowserLanguage(){
	return (window.navigator.userLanguage || window.navigator.language);
}

// cookie's functions 
function setCookie(name, value, expires, path, domain, secure) {
   document.cookie = name + "=" + escape (value) +
    ((expires && expires!="") ?("; expires=" + expires.toGMTString()):"") +// No I18N
    ((path) ?("; path=" + path):"") +// No I18N
    ((domain) ?("; domain=" + domain):"") +// No I18N
    ((secure) ?"; secure" : "");// No I18N
}
function getCookie(cname) {
  var rexPat = new RegExp(cname+"=[^;]*");
  var allCookie = document.cookie;
  var myCookie = rexPat.exec(allCookie);
  if(myCookie){
  	var cookie=myCookie[0].split("=");
	return unescape(cookie[1]);
  }
  else{
	return null;
  }
}
function delCookie(name,path,domain,secure) {
  var exp = new Date();
  exp.setTime (exp.getTime() - 1);
  var cval = getCookie(name);
  if(cval){
  	document.cookie = name + "=null; expires=" + exp.toGMTString()+// No I18N
	((path) ?("; path=" + path):"") +// No I18N
    ((domain) ?("; domain=" + domain):"") +// No I18N
    ((secure) ?"; secure" : "");// No I18N
  }
}

fnAsString = function(str){
    str = str.replace(/&/g, "&amp;");
    str = str.replace(/</g, "&lt;");
    str = str.replace(/>/g, "&gt;");
    str = str.replace(/\"/g, "&#34;");
    return str;
}

checkExternalUrl = function(url) {
    var retVal = false;
    var lDomain = window.location.host;
//    var previewLinkPattern = new RegExp('^((http(s)?://)?(labs|pre|features|module|integ[1-9]?)?site(preview|builder)-[0-9]+(.(local|dev)?zohositescontent.com)/[a-zA-Z0-9\\x00-\\x7F\-\/]+((.html)|(/))(\\?(mobile|responsive)=true)?)$');
    var previewLinkPattern = new RegExp('^((http(s)?://)?(labs|pre|features|module|integ[1-9]?)?site(preview|builder)-[0-9]+(.'+content_domain+')/[a-zA-Z0-9\\x00-\\x7F\-\/]+((.html)|(/))(\\?(mobile|responsive)=true)?)$');
    var jscriptPattern = new RegExp('^(javascript)');
    
    var t = document.createElement('a');
    t.href = url;
    var exDomain = t.host;
    
    if(previewLinkPattern.test(url)) {
        retVal = true;
    }
    else if (jscriptPattern.test(url)) {
        retVal = true;
    }
    return retVal; 
} 

checkMoreMenuLink =function(link) {
    var retVal= false;
    if(link.hasAttribute("more") && link.getAttribute("more") === "true") {
            retVal = true;
    }
    return retVal;
}
