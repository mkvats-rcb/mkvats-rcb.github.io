/*$Id: navigation-load.js,v 2126:25524a5ec327 2011/07/28 10:46:26 prashantd $*/
var _domloaded,drtimer;
var xmlhttp;
var formscount = new Array();
var formscache = new Array();
var formsLoaded = new Array();
var scriptLoaded = false;

var creatorJsLoaded = true;
var startVal;
var endVal;
var MAX_EXTRIES = 10;
var navigArray = {"first":10, "last1":0, "previous":10, "next":20, "last":0};   //NO I18N
var cbGlobal = {};
var cbNavig = {};
var commentsArr;
var cbCRHTML;
var commentsTempArr=[];
var cbElm;
var origOverlayWidth=0,origOverlayHeight=0,tempOverlayWidth=0,tempOverlayHeight=0,tempOrigOverlayWidth=0,tempOrigOverlayHeight=0;
var prevWidth=parent.window.innerWidth;
var curWidth=parent.window.innerWidth;
var cbDet;
var isResize = false;
var inpElm;
 var formsLoadedCount=0;

var drChecker = function(e){
    if(e && (e.type == "DOMContentLoaded" || e.type == "load")) {
        fireDOMReady();
    } else if (document.readyState) {
        if((/loaded|complete/).test(document.readyState)) {
            fireDOMReady();
        }
        else if(typeof document.documentElement.doScroll == 'function') {
            try {
                loaded || document.documentElement.doScroll('left');
            }
            catch(e) {
                return;
            }
            fireDOMReady();
        }
    }
}
var fireDOMReady = function() {
    if(!_domloaded) {
        _domloaded=true;
        if(document.removeEventListener) {
            document.removeEventListener("DOMContentLoaded", drChecker, false);
        }
        document.onreadystatechange = null;
        window.onload = null;
        clearInterval(drtimer);
        drtimer = null;
        onloadFunction();
    }
}
function setScroll(){
    if(window.scrollY!=scrolly){
        scroll=true;
        scrolly=window.scrollY;
    }
}

if(document.addEventListener)
    document.addEventListener("DOMContentLoaded", drChecker, false);
    document.onreadystatechange = drChecker;
    window.onload = drChecker;
    drtimer = setInterval(drChecker,5);

var albumCount;
var loadingAlbumCount = 1;
var galleryElements;
var Gallery = {};
var twitterElements = new Array();
var twitterWidgetElem = new Array();
var twitterButtonElem = new Array();
var mapElem = new Array();
var gplusElem = new Array();
var dyncontElem = new Array();
var gplusBlogElm = new Array();
var imgElem = new Array();
var ownGallery = {}
var carousel;
var audios=[];
var playLists=[];
var hash="";
var scroll=false;
var scrolly=window.scrollY;
var assetsUrl="";
var newsletter_elts = new Array();

getElementsByName_iefix=function(tag, name) {
    var elem = document.getElementsByTagName(tag);
    var elems = [];
    for(i = 0,iarr = 0; i < elem.length; i++) {
        att = elem[i].getAttribute("name");
        if(att == name && name == "gallery") {
            albumCount.push(elem[i]);
            galleryElements.push(document.getElementById('zpg'+elem[i].id));
         }else{
            elems.push(elem[i]);
         }
    }
    return elems;
}

getElementsByClassName_ieFix = function(context, search){
    var elements_arr = [];
    elements_arr[0] = context.getElementsByTagName("div");
    elements_arr[1] = context.getElementsByTagName("input");
    var result = [];
    for(var i = 0; i < elements_arr.length; i++){
        var elements = elements_arr[i];
        for(var index = 0; index < elements.length; index++){
            if(elements[index].className === search || elements[index].className.indexOf(search) != -1) {
                result.push(elements[index]);
            }
        }
    }
    return result;
}

if(typeof String.prototype.trim !== "function"){
    String.prototype.trim = function() {
        return this.replace(/^\s+|\s+$/g, ''); 
    }
}

var usrAgent = window.navigator.userAgent;
if(window.ZS_PreviewMode){
        assetsUrl="/zs-site/assets/v0";//NO I18N
}
var creatorJqueryFile = (usrAgent.toLowerCase().indexOf("msie")!=-1) ? "/siteforms/appcreator/live/common/js/jqueryie.js" : "/siteforms/appcreator/live/common/js/jquery.js";//NO I18N
//var creatorScriptSrcs =[creatorJqueryFile,"/siteforms/appcreator/live/common/js/form.js","/siteforms/appcreator/live/common/js/generatejs.js","/siteforms/appcreator/live/common/js/searchableInput.js","/siteforms/appcreator/live/common/js/app.js",assetsUrl+"/js/form-renderer.js",assetsUrl+"/js/formSubmitJs.js"];//NO I18N
var preCreatorScript =["/siteforms/appcreator/live/common/js/form.js","/siteforms/appcreator/live/common/js/generatejs.js","/siteforms/appcreator/live/common/js/searchableInput.js","/siteforms/appcreator/live/common/js/app.js","/siteforms/appcreator/live/common/js/securityutil.js"];//NO I18N
var creatorScriptSrcs =[assetsUrl+"/js/form-renderer.js",assetsUrl+"/js/formSubmitJs.js"];//NO I18N

function spcLoadScript (src,load,fun) {
    var script = document.createElement('script');
    if(load){
        if(this.addEventListener){
            script.onload = function(){fun();};
        }
        else{
            script.onreadystatechange = function(){
                if(v(["loaded","complete"],this.readyState)>-1){
                    this.onreadystatechange= null;
                    fun();//check call sites scrip
                }
            }
        }
    }
    script.onerror = function(){creatorJsLoaded = false;};
    script.src =src;
    document.getElementsByTagName('head')[0].appendChild(script);
}

function loadCreatorScripts() {
    if(creatorScriptSrcs.length>0) {
        var loadScriptNow = creatorScriptSrcs.shift();
        //commonLoadScript(loadScriptNow,(creatorScriptSrcs.length==0)?"setFormContextPath":"loadCreatorScripts");///NO I18N
        spcLoadScript(loadScriptNow,true,(creatorScriptSrcs.length==0)?setFormContextPath:loadCreatorScripts);///NO I18N
    }
}

function loadPreCreatorScripts() {//creator js
    var loadScriptNow = preCreatorScript.shift();
    if(preCreatorScript.length==0) {
        spcLoadScript(loadScriptNow,true,loadCreatorScripts);
    }else{
        spcLoadScript(loadScriptNow,true,loadPreCreatorScripts);
    }
}

function getAllFormMeta (){
    for(i=0;i<formscount.length;i++){
        getForm(i);
    }
}

function setFormContextPath (){
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
    }
}


getForm = function(formno){
    var form = formscount[formno];
    if(!form){
        return;
    }
    var formDiv = form.firstChild;
    while(!formDiv.tagName) {
        formDiv = formDiv.nextSibling;
    }
    var frmid =formDiv.getAttribute("formid");
    var siteid = frmid.split("-")[0].replace(/,/g,"");
    var formid = frmid.split("-")[1].replace(/,/g,"");
    if(formsLoaded.indexOf(formid)!=-1){
        return;
    }else{
        formsLoaded.push(formid);
        try{
            var xmlhttp;
            if(window.XMLHttpRequest)
                xmlhttp = new XMLHttpRequest();
            else
                xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
            xmlhttp.onreadystatechange = function(){
                if(xmlhttp.readyState ==4 && xmlhttp.status == 200)
                    renderOrSave(formno,form,this.responseText);
            }
            appName = "zsa-"+siteid;
            var time = new Date().getTime();
            if(window.ZS_PreviewMode){
                xmlhttp.open("GET", "/zs-site/api/v0/getFormMeta?siteId=" + siteid + "&formid="+formid, true);
            }else{
                xmlhttp.open("GET","/siteforms/showFormJson.do?formLinkName="+formid+"&sharedBy="+siteid+"&appLinkName="+appName+"&type=json&raw=true&metaData=complete&time="+time,true);
            }
            xmlhttp.send();
        }catch(e){}
    }
}

renderOrSave = function(formno,form,metaData){
    try {
        if(metaData!="formNotExist"){
            JSON.parse(metaData);
        }
    } catch (e) {
        formsLoadedCount++;
        if(formsLoadedCount==formscount.length&&hash!=""&&!scroll){
            window.location.hash=hash;
            hash="";
        }
        return false;
    }
    if(scriptLoaded){
        installForm(formno,form,metaData);
    }else{
        var formJSON ={};
        formJSON.data=metaData;
        formJSON.formno=formno;
        formJSON.form=form;
        formscache.push(formJSON);
    }
}

renderFromSave = function(){
    for(j=0;j<formscache.length;j++){
        var metaData = formscache[j].data;
        var no = formscache[j].formno;
        var form = formscache[j].form;
        installForm(no,form,metaData);
    }
}

installForm = function(formno,form,metaData){
    if(metaData=="formNotExist"){
        var result=metaData;
    }else{
        var result=JSON.parse(metaData);
        result.DisplayName = result.DisplayName.replace(/\\\"/g,"\"");
    }
    var formName = form.children[0].id.replace("zpform_","");
    if(window.ZS_PreviewMode && ((result.errorlist && result.errorlist[0].error[0]==2893)|| result=="formNotExist") && formName=="Contact_Us"){
        fnGetContactUsFormMeta(form, formno);//need check
        return true;
    }

    var formid;
    if(result)
        formid=result["formLinkId"];
    if(result && (formid!=undefined)){
        var formDiv = form.firstChild;
        while(!formDiv.tagName) {
            formDiv = formDiv.nextSibling;
        }
        formDiv.innerHTML =ZohoForms.renderForm(result);
        ZohoForms.loadCaptcha(formDiv);
        formsLoadedCount++;
        if(formsLoadedCount==formscount.length&&hash!=""&&!scroll){
            window.location.hash=hash;
            hash="";
        }
        try {
            var scripts = formDiv.getElementsByTagName("script");
        } catch(e){}
        if(creatorJsLoaded==true){
            ZCForm.inZohoCreator = false;
            var onLoadExist = result.onLoadExist;
            var appLinkName = result.appLinkName;
            var formLinkName = result.formLinkName;
            var formDispName = fnAsString(result.DisplayName);
            var formAccessType = result.recType;
            var formID = result.formid;
            ZCForm.zcFormAttributes['genScriptURL'] = "/siteforms/generateJS.do";
            ZCForm.zcFormAttributes['formParentDiv'] = false;
            ZCForm.zcFormAttributes['customCalendar'] = true;
            ZCForm.zcFormAttributes['browseralert'] = false;
            ZCForm.zcFormAttributes['ajaxreload'] = true;
            ZCForm.zcFormAttributes['fieldContainer'] = "div";
            ZCForm.zcFormAttributes['eleErrTemplate'] = "<div tag='eleErr'> insertMessage </div>";
            relodCurrentForm = false;
            var paramsMapString = "formID=" + formID + ",appLinkName=" + appLinkName + ",formDispName="+ formDispName + ",formAccessType=1,formLinkName="+formLinkName;
            ZCForm.addToFormArr(paramsMapString, formLinkName);
            if(onLoadExist){
                doActionOnLoad(formID, ZCForm.getForm(formLinkName, formAccessType));
            }else{
                ZCForm.enableForm(formLinkName,formAccessType);
            }
            ZCForm.regFormEvents(formLinkName,formAccessType);
        }
    }
    return true;
}


onloadFunction = function() {
    if(responsiveTheme)
    {
     var iframes=document.getElementsByTagName("iframe");
        for(var i=0;i<iframes.length;i++)
         {
            if(iframes[i].src.indexOf("//www.youtube.com/embed/")>=0&&iframes[i].clientWidth>iframes[i].parentNode.clientWidth)
              {
                var aspect_ratio=iframes[i].width/iframes[i].height;
                iframes[i].width="100%";
                iframes[i].height=iframes[i].clientWidth/aspect_ratio;
              }
         }   
    }
    getPortalUserName();
    if(document.getElementById('nav-top')){
    try{
        if(parent.document.getElementById("zp_tmplcustom")==null ||parent.document.getElementById("zp_tmplcustom").style.display=="block"){
            navActivate();
        }
    }catch(e){
        navActivate();
    }
    }
    var winHref = window.location.href;
    if(winHref.indexOf("zc_success")!=-1){
        var loc =winHref.split("?")[0];
        var msg = winHref.split("?")[1];
        if(msg.split("=")[1]=="true"){
            alert("Form Submitted Successfully");
            window.location=loc;
        }
    }
    albumCount = new Array();
    galleryElements = new Array();
    twitterElements = new Array();
    facebookElem = new Array();
    mapElem = new Array();
    gplusElem = new Array();
    gplusBlogElm = new Array();
    tE = new Array();
        linkedInElem = new Array();
        imgElem = new Array();
    var picasa;
    var flickr;
    var form;
    var isBlogPage = document.getElementById("blogPage-container");
    var isSlideBanner = document.getElementById("carouselImg");
    if(document.querySelectorAll){
        picasa = document.querySelectorAll('.picasa');//NO I18N
        flickr = document.querySelectorAll('.flickr');//NO I18N
        var ownGallery = document.querySelectorAll('.ownGallery');//NO I18N
        for(p=0;p<picasa.length;p++){
            var accordionParent=findParent(picasa[p],"zs-tabs-accordion-content");
            if(accordionParent==false || accordionParent.style.display=="block"){
                albumCount.push(picasa[p]);
                galleryElements.push(document.getElementById("zpg"+picasa[p].id));
            }
        }
        for(f=0;f<flickr.length;f++){
            var accordionParent=findParent(flickr[f],"zs-tabs-accordion-content");
            if(accordionParent==false || accordionParent.style.display=="block"){
            albumCount.push(flickr[f]);
            galleryElements.push(document.getElementById("zpg"+flickr[f].id));
            }
        }
        for(o=0;o<ownGallery.length;o++){
            var accordionParent=findParent(ownGallery[o],"zs-tabs-accordion-content");
            if(accordionParent==false || accordionParent.style.display=="block"){
                albumCount.push(ownGallery[o]);
                galleryElements.push(document.getElementById("zpg"+ownGallery[o].id));
            }
        }
    }
    else if(!document.querySelectorAll){
        getElementsByName_iefix('div','gallery');//NO I18N
    }
    
    
    albumCount.reverse();
    galleryElements.reverse();
    if(albumCount.length>0){
        if(window.ZS_PreviewMode){
            commonLoadScript(assetsUrl+"/js/gallery.js");//NO I18N
            commonLoadScript(assetsUrl+"/js/collage.js");//NO I18N
            loadingAlbumCount = 1;
        }else{
            xml("GET","/gallery.txt",function(res){//NO I18N
                window.ownGallery = JSON.parse(res);
                commonLoadScript("/js/gallery.js");//NO I18N
                commonLoadScript("/js/collage.js");//NO I18N
                loadingAlbumCount = 1;
                },"");
        }
    }
    if(document.querySelectorAll){
        carousel = document.querySelectorAll('.carousel');//No I18N
    }else{
        carousel = getElementsByName_iefix('div','carousel');//No I18N
    }
    if(isSlideBanner || carousel){
        commonLoadScript(assetsUrl+"/js/animation.js");//NO I18N
        if(isSlideBanner){
            var func = function(){
                if(typeof(ImageRotator) != "undefined"){
                    clearInterval(window.interval);
                    var slider = new ImageRotator(isSlideBanner,slideImages.slideURL,slideImages.settings, slideImages.slideCss, slideImages.slideBg);
                }
            }
            window.interval = setInterval(func,16);
        }
        if(carousel){
            var func2 = function(res){
                var func3 = function(){
                    if(typeof(ImageRotator) != "undefined"){
                        clearInterval(window.interval1);
                        res = JSON.parse(res);
                        func(res);
                    }
                }
                window.interval1 = setInterval(func3,16);
            }
            var func = function(res){
                for(var c=0;c<carousel.length;c++){
                    var elem=carousel[c];
                    var carouselProp = res[elem.getAttribute("carouselId")];
                    if(carouselProp){
                    var imgs = carouselProp.slideURL;
                    var imgArr=[];
                    for(var i=0;i<imgs.length;i++){
                        if(window.ZS_PreviewMode){
                            imgArr.push(imgs[i]);
                        }else{
                            var imgSrc = imgs[i].split("/");
                            imgArr.push(imgs[i].replace(imgSrc[2]+"/",""));
                        }
                    }
                    var carCont = document.createElement("div");
                    carCont.innerHTML="<div style=\"position: absolute; top: 0px; left: 0px; opacity: 1; z-index: 2; overflow:hidden;background:#fff\"></div><div style=\"position: absolute; top: 0px; left: 0px; display: block; z-index: 1; overflow:hidden;\">";//No I18N
                    elem.children[0].appendChild(carCont);
                    var width = carCont.parentNode.clientWidth;
                    if(carouselProp.settings.thumbnail == "on" && (carouselProp.settings.thumbPos == "left" || carouselProp.settings.thumbPos == "right")){
                        width = width-65;
                    }
                    var height=Math.floor((width*9)/16);
                    carCont.style.cssText = "position:relative;width:"+width+"px;height:"+height+"px;overflow:hidden;float:left";//No I18N
                    carCont.id="ir"+Math.floor(Math.random()*1000000000000);
                    if((typeof(responsiveTheme)!="undefined" && responsiveTheme==true && mobile===true && carouselProp.settings.type==="film" )||(typeof(window.parent.responsivePreview)!="undefined" && window.parent.responsivePreview=="true" && carouselProp.settings.type==="film")){
                        carouselProp.settings.type="stripe";
                    }
                     var rotator=new ImageRotator(carCont,imgArr,carouselProp.settings);}
                }
            }
            if(window.ZS_PreviewMode){
                    func2(JSON.stringify(carouselProp));
            }else{
                xml("GET","/carousel.txt",func2,""); //No I18N
            }
        }
    }
    if(document.querySelectorAll){
        audios = document.querySelectorAll(".audio");//NO I18N
    }else{
        audios = getElementsByName_iefix("div", "audio");//NO I18N
    }
    if(document.querySelectorAll){
        playLists = document.querySelectorAll(".audioplaylist");//NO I18N
    }else{
        playLists = getElementsByName_iefix("div", "audioplaylist");//NO I18N
    }
    if(audios || playLists){
        commonLoadScript(assetsUrl+"/js/audio.js","audio"); //NO I18N
    }
    // for social share it is come first for render fb, twit, gp
    segregateElements('socialshare', twitterElements, "name");//NO I18N
    segregateElements('socialshare', facebookElem, "name");//NO I18N
    segregateElements('socialshare', gplusElem, "name");//NO I18N
    segregateElements('socialshare', linkedInElem,"name");//NO I18N
    // for twitter

    segregateElements('twitter', twitterElements, "name");//NO I18N
    if(twitterElements.length >0 || isBlogPage){
        commonLoadScript(assetsUrl+"/js/twitter.js","loadTwitterJS");//NO I18N
    }
    // for facebook
    segregateElements('facebook',facebookElem,"name");//NO I18N
    if(facebookElem.length >0 || isBlogPage){
        commonLoadScript(assetsUrl+"/js/facebook.js","loadFacebookJS");//NO I18N
    }

    // for linkedin
    segregateElements('linkedin',linkedInElem,"name");//NO I18N
    if(linkedInElem.length >0 || isBlogPage){
        commonLoadScript('//platform.linkedin.com/in.js', "loadLIFramework");
    }

    commentBoxElm = new Array();
    segregateElements('commentbox',commentBoxElm,"name");//NO I18N

    if(commentBoxElm.length > 0){
        renderCBRateSVG();
        
    }

//  table element - start
    var docTables = document.getElementsByTagName("table");
    for(var i=0; i<docTables.length;i++) {
        var currTable = docTables[i];

        var dummyDiv = document.createElement("div");
        dummyDiv.style.overflow="auto";

        currTable.parentNode.insertBefore(dummyDiv, currTable);
        dummyDiv.appendChild(currTable);
    }

//  resize images for small images in mobile
    segregateElements('image',imgElem,"name");//NO I18N

    var imgFix = function(){
        var img = this;
        if(img.clientWidth > img.naturalWidth && ZS_MobileVer) {
            img.style.width = img.naturalWidth+"px";
        }
    }
    for(var c=0;c<imgElem.length;c++){
        var tabflag = false;
        var tabimage = findParent(imgElem[c],"zs-tabs-accordion-content");
        if(tabimage && !(tabimage.style.display == "block")){
            tabimage.style.display = "block";
            tabflag =true;
        }
        var imgElCont = imgElem[c];
        var imgF = imgElCont.getElementsByTagName("img")[0];
        if(imgF.complete) {
             imgFix.apply(imgF);
        }
        else {
            imgF.onload = imgFix;
        }
        if(tabflag == true){
            tabflag = false;
            tabimage.style.display = "none";
        }
    }

    //for imagetext
    segregateElements('imagetext',imgElem,"name");//NO I18N
    for(var c=0;c<imgElem.length;c++){
        var tabflag = false;
        var tabimage = findParent(imgElem[c],"zs-tabs-accordion-content");
        if(tabimage && !(tabimage.style.display == "block")){
            tabimage.style.display = "block";
            tabflag =true;
        }
        var imgElCont = imgElem[c];
        var imgF = imgElCont.getElementsByTagName("img")[0];
        if(imgF.complete) {
             imgFix.apply(imgF);
        }
        else {
            imgF.onload = imgFix;
        }
        if(tabflag == true){
            tabflag = false;
            tabimage.style.display = "none";
        }
    }

    // for maps
    segregateElements('map',mapElem,"name");//NO I18N
    if(mapElem.length > 0){
        commonLoadScript('//maps.googleapis.com/maps/api/js?sensor=false&callback=loadMapJs');//NO I18N
    }
    // for gplus1
    segregateElements('gplus',gplusElem,"name");//NO I18N
    if(gplusElem.length > 0){
        commonLoadScript(assetsUrl+"/js/gplus.js","rendergplus");//NO I18N
    }
    if(isBlogPage){
        segregateElements('g-plusone',gplusBlogElm,"class");//NO I18N
        if(!(window.gapi && window.gapi.plus)){
            commonLoadScript(assetsUrl+"/js/gplus.js","rendergplus");//NO I18N
            fnGplusAction();
        }else{
            fnGplusAction();
        }
    }
    segregateElements('form',formscount,"name");//NO I18N
    var crmFormscount = new Array();;
    segregateElements('crmform',crmFormscount,"name");//NO I18N

    if(formscount.length){
        if(window.location.hash){
            hash=window.location.hash;
            window.location.hash="";
            window.addEventListener("scroll", setScroll, false);
        }
        getAllFormMeta();
        //loadPreCreatorScripts();
        spcLoadScript(creatorJqueryFile,true,loadPreCreatorScripts);
    }else if(crmFormscount.length){
        commonLoadScript(assetsUrl+"/js/form-renderer.js");///NO I18N
    }
    segregateElements('dynamiccontent', dyncontElem,"name");//NO I18N
    /*if(dyncontElem.length>0){
        fnRenderDCnt(0);
    }*/
    for(i=0;dyncontElem[i];i++){
        fnRenderDCnt(i);
    }
    if(ZS_ColumnFix){
        fnSetColumnsWidth();
    }
    var googlecart = document.getElementById('googlecart-script-add');
    if(googlecart){
        var script = document.createElement('script');
        script.id='googlecart-script';
        script.type="text/javascript";
        script.setAttribute('currency',googlecart.parentNode.parentNode.getAttribute('data-currency'));
        script.setAttribute('hide-cart-when-empty','true');
        script.setAttribute('post-cart-to-sandbox','false');
        script.setAttribute('integration','jscart-wizard');
        script.src="//checkout.google.com/seller/gsc/v2_2/cart.js?mid="+googlecart.getAttribute('mid');
        if(window.ZS_PreviewMode){
            parent.document.getElementsByTagName('body')[0].appendChild(script);
        }
        else{
            document.getElementsByTagName('body')[0].appendChild(script);
        }
    }
    segregateElements('newsletter', newsletter_elts, "name");//NO I18N
    if(newsletter_elts.length > 0){
        render_newsletter();
    }
    getBlogPostCommentsCount();
    resizeElements();
    if((typeof(responsiveTheme)!="undefined" && responsiveTheme==true )||(typeof(window.parent.responsivePreview)!="undefined" && window.parent.responsivePreview=="true")){
        var mobileSite=document.getElementById("mobileSite");
        if(mobileSite){
            mobileSite.parentNode.removeChild(mobileSite);
        }
        if((mobile && !istab) || window.innerWidth<640 ||(typeof(window.parent.responsivePreview)!="undefined" && window.parent.responsivePreview=="true")){
            var searchContainer=document.getElementsByClassName("themeSearchContainer");//NO I18N
            if(searchContainer[0]){
                //searchContainer[0].parentNode.removeChild(searchContainer[0]);
                searchContainer[0].style.display="none";
            }
        }

    }
    if(window.ZS_PreviewMode){
        fnBindHandleClickEvents();
    }
    checkCookie();
    if(ZS_adjustHeight){
        setTimeout(function(){fnSetEqualHeight()},1000);
    }
}

getPortalUserName = function(){
    if(document.getElementById("zpPortalSignIn") != null){
    //if(window.ZS_PublishMode){
        try{
            if(window.XMLHttpRequest){
                xmlhttp = new XMLHttpRequest();
            }else{
                xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
            }
            xmlhttp.onreadystatechange = function(){
                if(xmlhttp.readyState == 4 && xmlhttp.status == 200){
                    var dispName = this.responseText; 
                    var showDiv,otherDiv;
                    if(dispName.indexOf("null")!=-1){
                        showDiv = document.getElementById("zpPortalSignIn");
                        otherDiv = document.getElementById("zpPortalUser");
                        if(dispName.substring(dispName.indexOf("null")+4).trim()!="true"){
                            document.getElementById("zpPortalSignupLi").style.display="none";
                        }
                    }else{
                        showDiv = document.getElementById("zpPortalUser");
                        document.getElementById("zpUserName").innerHTML = "Hi "+dispName; //NO I18N
                        document.getElementById("zpLogout").href = "/signout?logout=true&serviceurl="+encodeURIComponent(document.location.protocol + "//" + document.location.host); //NO I18N
                        otherDiv = document.getElementById("zpPortalSignIn");
                    }
                    showDiv.style.display="block";     
                    otherDiv.style.display="none";     
                }            

            }
            xmlhttp.open("GET", "/getCurrentPortalUser", false);
            xmlhttp.send();
        }catch(e){}
    /*}else{
        document.getElementById("zpPortalSignIn").style.display="block";
    }*/
    }
}


segregateElements = function(elemType,elmArray,attrName){
    if(document.querySelectorAll){
        elmsArr = document.querySelectorAll('.'+elemType); //NO I18N
        for(var f=0;f<elmsArr.length;f++){
            elmArray.push(elmsArr[f]);
        }
    }
    else if(!document.querySelectorAll){
        var elem = document.getElementsByTagName('div');
        for(var i = 0; i < elem.length; i++) {
            var att = elem[i].getAttribute(attrName);
            if(att == elemType) {
                elmArray.push(elem[i]);
            }
        }
    }
}

loadAudioFiles=function(){
    if(audios){
        for(var i=0,audio;audio=audios[i];i++){
            fnSetupAudio(audio.children[0].children[0],audio.getAttribute("data-autoplay"),audio.getAttribute("data-loop"));
        }
    }
    if(playLists){
        for(var i=0,play;play=playLists[i];i++){
            var audioCont;
            if(document.querySelector){
                audioCont = play.querySelector(".audioContainer");//NO I18N
            }else{
                audioCont = play.children[0].children[0];
            }
            if(audioCont.getAttribute("data-playlist") && play.clientHeight!=0){
            var audioList = audioCont.getAttribute("data-playlist").split(",");
            audioCont.playList = audioList;
            audioCont.curPlayItem = 0;
            audioCont.removeAttribute("data-playlist");
            fnSetupAudio(audioCont,play.getAttribute("data-autoplay"),play.getAttribute("data-loop"),true);
            }
        }
    }
}
loadMapJs = function(){
    commonLoadScript(assetsUrl+"/js/maps.js","map");//NO I18N
}
fnGplusAction =function(){
    var obj = {"size":"tall","annotation":"none","height":"24px"};//No I18N
    if(!(window.gapi && window.gapi.plus)){
        setTimeout(function(){fnGplusAction()},100);
    }else{
        while(gplusBlogElm.length>0){
            var gElm = gplusBlogElm.pop();
            gapi.plusone.render(gElm,obj);
        }
    }
}
fnloadTwitterJS = function(){
    twitterWidgetElem = new Array();
    twitterButtonElem = new Array();
    var tW=false,tB=false;
    for(var i=0;i < twitterElements.length;i++){
        var elem = twitterElements[i];
        if(elem.getAttribute("data-twType")=="share" || elem.getAttribute("data-layout")){
            twitterButtonElem.push(elem);
            tB = true;
        }
        else{
            twitterWidgetElem.push(elem);
            tW = true;
        }
    }
    if(tB || document.getElementById("blogPage-container") || tW){
        enableTwitterWidget();
    }
}
fnSetEqualHeight=function() {
    var page;
    if((typeof parent.mobilePreview != "undefined" && parent.mobilePreview == "true" || typeof parent.responsivePreview != "undefined" && parent.responsivePreview == "true") || (typeof mobile == "boolean" && (mobile))){
        return;
    }
    if(!window.blogPage){
        page = document.getElementById("page-container");
        page.style.height="";
    }else{
        page = document.getElementsByClassName("themeContentContainer")[0];
    }
    var side = document.getElementById("sidebar-container");
    if(!side){
        return;
    }
    //Check equalHeight before setting equal height to sidebar
    if(side.offsetHeight>page.offsetHeight){
        page.style.minHeight=side.offsetHeight+"px";//NO I18N
    }else if(side.offsetHeight<page.offsetHeight){
        side.style.minHeight=page.offsetHeight+'px';//No I18N
    } else if(window.ZS_adjustHeight){
        side.style.minHeight=page.offsetHeight+"px";//NO I18N
    }

}

fnSetColumnsWidth=function(){
    //take width of columns in % and set it as px value
    var columnElems=getClasses("zpcolumns","div");//('div','.zpcolumns'); //NO I18N
    var elem,i=0,innerElems,innerElem,wid,cal;
    for(;elem=columnElems[i];i++){
        innerElems=getClasses("zpflLeft","div",elem);//NO I18N
        for(var j=0;innerElem=innerElems[j];j++){
            if(innerElem.style.width.indexOf("%")!=-1){
                wid=parseInt(innerElem.style.width);
                cal = parseInt(elem.parentNode.offsetWidth*wid/100);
                innerElem.style.width=Math.round(cal)+"px";
            }
        }
    }
}
getClasses = function(clsName,tag,elem){
    var retVal = new Array();
    var elements;
    if(elem){
        elements = elem.childNodes;//getElementsByTagName(tag);
        for(var i = 0;i < elements.length;i++){
            if(elements[i].className){
                var classes = elements[i].className.split(" ");
                for(var j = 0;j < classes.length;j++){
                    if(classes[j] == clsName){
                        retVal.push(elements[i]);
                    }
                }
            }
        }
        return retVal;
    }
    if (tag == null) {
        tag="*";
    }
    elements = document.getElementsByTagName(tag);
    for(var i = 0;i < elements.length;i++){
        if(elements[i].className){
            var classes = elements[i].className.split(" ");
            for(var j = 0;j < classes.length;j++){
                if(classes[j] == clsName){
                    retVal.push(elements[i]);
                }
            }
        }
    }
    return retVal;
}

/*
getForm = function(formno){
    var form = formscount[formno];
    if(!form){
        return;
    }
    var formDiv = form.firstChild;
    while(!formDiv.tagName) {
        formDiv = formDiv.nextSibling;
    }
    var frmid =formDiv.getAttribute("formid");
    var siteid = frmid.split("-")[0].replace(/,/g,"");
    var formid = frmid.split("-")[1].replace(/,/g,"");
    if(ZohoForms.fnCheckFormsInPage(formid,true)){
        if(++formno<formscount.length){
            getForm(formno);
        }
        return true;
    }
    else{
        try{
            if(window.XMLHttpRequest)
                xmlhttp = new XMLHttpRequest();
            else
                xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
            xmlhttp.onreadystatechange = function(){
                if(xmlhttp.readyState ==4 && xmlhttp.status == 200)
                    installForm(formno,form);
                }
                appName = "zsa-"+siteid;
                var time = new Date().getTime();
                if(window.ZS_PreviewMode){
                    xmlhttp.open("GET", "/zs-site/api/v0/getFormMeta?siteId=" + siteid + "&formid="+formid, true);
                }else{
                    xmlhttp.open("GET","/siteforms/showFormJson.do?formLinkName="+formid+"&sharedBy="+siteid+"&appLinkName="+appName+"&type=json&raw=true&metaData=complete&time="+time,true);
                }
                xmlhttp.send();
            }catch(e){}
    }
}

installForm = function(formno,form){
    if(xmlhttp.responseText=="formNotExist"){
        var result=xmlhttp.responseText;
    }else{
        var result=JSON.parse(xmlhttp.responseText);
    }
    var formName = form.children[0].id.replace("zpform_","");
    if(window.ZS_PreviewMode && ((result.errorlist && result.errorlist[0].error[0]==2893)|| result=="formNotExist") && formName=="Contact_Us"){
        fnGetContactUsFormMeta(form, formno);
        return true;
    }

    var formid;
    if(result)
        formid=result["formLinkId"];
    if(result && (formid!=undefined)){
        var formDiv = form.firstChild;
        while(!formDiv.tagName) {
            formDiv = formDiv.nextSibling;
        }
        formDiv.innerHTML =ZohoForms.renderForm(result);
    try {
    var scripts = formDiv.getElementsByTagName("script");
    } catch(e){}
      if(creatorJsLoaded==true){
        ZCForm.inZohoCreator = false;
        var onLoadExist = result.onLoadExist;
        var appLinkName = result.appLinkName;
        var formLinkName = result.formLinkName;
        var formDispName = fnAsString(result.DisplayName);
        var formAccessType = result.recType;
        var formID = result.formid;
        ZCForm.zcFormAttributes['genScriptURL'] = "/siteforms/generateJS.do";
        ZCForm.zcFormAttributes['formParentDiv'] = false;
        ZCForm.zcFormAttributes['customCalendar'] = true;
        ZCForm.zcFormAttributes['browseralert'] = false;
        ZCForm.zcFormAttributes['ajaxreload'] = true;
        ZCForm.zcFormAttributes['fieldContainer'] = "div";
        ZCForm.zcFormAttributes['eleErrTemplate'] = "<div tag='eleErr'> insertMessage </div>";
        relodCurrentForm = false;
        var paramsMapString = "formID=" + formID + ",appLinkName=" + appLinkName + ",formDispName="+ formDispName + ",formAccessType=1,formLinkName="+formLinkName;
        ZCForm.addToFormArr(paramsMapString, formLinkName);
        if(onLoadExist){
            doActionOnLoad(formID, ZCForm.getForm(formLinkName, formAccessType));
        }else{
            ZCForm.enableForm(formLinkName,formAccessType);
        }
        ZCForm.regFormEvents(formLinkName,formAccessType);
    }
    }
    if(++formno<formscount.length)
        getForm(formno);
    else if(hash!="" && !scroll){
        window.location.hash=hash;
    }
    return true;
}
*/

addPostComments = function(){
    if(window.ZS_PreviewMode){
        alert('Comments can be only added in Published Site');
        return;
    }
    var path = location.pathname;
    var pat = /\/blogs\/post\/(.*)\/?/;
    var blogPostUrl = path.replace(pat,"$1");
    var authorName = document.getElementById("authorName").value;
    var authorMailId = document.getElementById("authorMailId").value;
    var comments = document.getElementById("commentsMessage").value;
    var errorMsg =  document.getElementById("comments_error_msg");
    if(authorName === "" || comments===""){
        errorMsg.innerHTML="Message and Name fields are mandatory. Please try once again";//No I18N
        setTimeout(function() {
           errorMsg.innerHTML = "";
        },5000)
        return false;
    }
    if(authorMailId !==""){
        var mailP = new RegExp("^[a-zA-Z0-9]([\\w\\-\\.\\+\']*)@([\\w\\-\\.]*)(\\.[a-zA-Z]{2,8}(\\.[a-zA-Z]{2}){0,2})$","mig");
        var reg_mail = mailP.exec(authorMailId);
        if(!reg_mail){
            errorMsg.innerHTML="Provide a proper email-id";//No I18N
            setTimeout(function() {
                errorMsg.innerHTML = "";
            },5000)
            return false;
        }    
    }
    var paramsdata = "authorName="+encodeURIComponent(authorName)+"&authorMailId="+authorMailId+"&comments="+encodeURIComponent(comments);//No I18N
    var ie = /*@cc_on!@*/false;
    if(!ie){
        xml("POST","/blogs/post/"+blogPostUrl+"/addComment",fnAddedComment,paramsdata);//No I18N
    }else{
        xml("POST","/blogs/post/"+encodeURIComponent(blogPostUrl)+"/addComment",fnAddedComment,paramsdata);//No I18N
    }
}
xml = function(method,url,callBack,postdata,args){
    try{
        var xmlhttp;
        if(window.XMLHttpRequest) {
            xmlhttp = new XMLHttpRequest();
        }
        else {
            xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
        }
        xmlhttp.onreadystatechange = function(){
            if(xmlhttp.readyState ==4 && (xmlhttp.status == 200 ||  xmlhttp.status ==304)) {
                callBack(xmlhttp.responseText, args);
            }
        }
        if(method === 'POST') {
            xmlhttp.open(method,url, false);
            xmlhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
            xmlhttp.setRequestHeader("Content-length", postdata.length);
            xmlhttp.setRequestHeader("Connection", "close");
            xmlhttp.send(postdata);
        }
        else {
            xmlhttp.open(method,url, true);
            xmlhttp.send(null);
        }
    }
    catch(e) {
        return;
    }
}

checkPassword = function(){
    var pageUrl=window.location.pathname.substr(1);
    if(document.secured_page_form.password.value.length > 0 && document.secured_page_form.password.value.length < 3 || document.secured_page_form.password.value.length > 25){
        document.getElementById("zp_invalidMsg").style.display = "";
    }else{
        document.secured_page_form.action = "/"+pageUrl;
        document.secured_page_form.submit();
    }
}

fnAddedComment = function(res){
    var result=JSON.parse(res);
    document.getElementById("authorName").value="";
    document.getElementById("authorMailId").value="";
    document.getElementById("commentsMessage").value="";
    if(result.moderation=="0"){
        alert("Your comments will be displayed only after moderation");
        return;
    }else if(result.moderation=="1"){
        var newElem = document.createElement("div");
        newElem.className="commentPadBottom";//No I18N
        var headElem = document.createElement("p");
        headElem.className="zs-text-highlight-color";
        headElem.innerHTML=result.COMMENTS_ADDED_BY;
        var emElem = document.createElement("span");
        emElem.className="commentDateStyle zs-text-light-color";
        emElem.style.display="block";
        emElem.innerHTML="Posted on : "+result.COMMENTS_ADDED_TIME;//No I18N
        emElem.style.marginTop = "3px"; //No I18N
        var pElem = document.createElement("p");
        pElem.className="commentTxtStyle";
        pElem.innerHTML=result.COMMENTS_CONTENT.replace(/\r?\n/g, '<br/>');
        var hrElem = document.createElement("hr");
        hrElem.size="1";
        newElem.appendChild(headElem);
        newElem.appendChild(emElem);
        newElem.appendChild(pElem);
        newElem.appendChild(hrElem);
        getBlogPostCommentsCount();
        //var comElem = document.getElementById("zpCommentsCount");
        //var comCount = comElem.getAttribute("data-commentscount");
        //var newCount = parseInt(comCount)+1;
        //comElem.setAttribute("data-commentscount",newCount);
        //comElem.innerHTML="Comments("+newCount+")";//No I18N
        var elem = document.getElementById("commentsList");
        var noElm = document.getElementById("noCommentCont");
        elem.insertBefore(newElem,elem.childNodes[0]);
        if(noElm){
            noElm.style.display="none"
        }
    }else{
        alert("Some problem in adding your comment...Please try later");
    }
}

fnRenderDCnt =function(cnt){
    var dc =dyncontElem[cnt];
    var zpPos = getClasses("zpAlignPos","div",dc);//NO I18N
    var applicationName = zpPos[0].getAttribute("data-applicationName");
    var formName = zpPos[0].getAttribute("data-formName");
    var viewname = zpPos[0].getAttribute("data-templateid").replace("dyncont","");
    //var filename="/view/view"+applicationName+"_"+formName+"_"+viewname+".txt";//NO I18N
    try{
        var xmlHttp;
        if(window.XMLHttpRequest)
            xmlHttp = new XMLHttpRequest();
        else
            xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
        xmlHttp.onreadystatechange = function(){
            if(xmlHttp.readyState ==4){
                if(xmlHttp.status == 200){
                    zpPos[0].innerHTML= this.responseText;
                    fnDynamicContentSearch(dc, zpPos[0]);
                }
                /*if(++cnt<dyncontElem.length){
                    fnRenderDCnt(cnt);
                }*/
            }
        }
        if(window.ZS_PreviewMode){
            xmlHttp.open("GET","/zs-site/api/v0/readGeneratedFile?siteId="+parent.siteId+"&applicationName="+applicationName+"&formName="+formName+"&viewName="+viewname,true);
        }else{
            xmlHttp.open("GET", "/siteapps/searchView?applicationName=" + applicationName + "&viewName=" + viewname + "&formName=" + formName +"&startindex=1", true);
            //xmlHttp.open("GET",filename,true);
        }
        xmlHttp.send();
    }catch(e){}
}

fnPreviewRss = function(){
    if(window.ZS_PreviewMode){
     alert("RSS works only in published website");
    }
}

fnFormPreviewSubmit = function(){
    alert("This is a preview and data cannot be submitted now");
}

fnFormSubmit = function(formElem){
    ZohoForms.submittedForm = formElem.getAttribute("name");
}

fnGetContactUsFormMeta = function(formElem, formno){
    try{
        if(window.XMLHttpRequest)
            xmlhttp = new XMLHttpRequest();
        else
            xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
        xmlhttp.onreadystatechange = function(){
            if(xmlhttp.readyState ==4 && xmlhttp.status == 200){
                fnGetContactUsFormMetaRes(formElem, formno);
            }
        }
        xmlhttp.open("GET",assetsUrl+"/js/form-meta.js",true);
        xmlhttp.send();
    }catch(e){}
}

fnGetContactUsFormMetaRes = function(formElem, formno){
    var jsonText=xmlhttp.responseText;
    var formsmeta=JSON.parse(jsonText.substring(jsonText.indexOf("{")));
    var result = formsmeta["form7"];
    var formid=result["formLinkName"];
    var formDiv = formElem.firstChild;
    while(!formDiv.tagName) {
        formDiv = formDiv.nextSibling;
    }
    formDiv.innerHTML =ZohoForms.renderForm(result);
    ZohoForms.loadCaptcha(formDiv);
    //if(++formno<formscount.length)
    //    getForm(formno);
}

validateCrmForm = function(crmfrm){
    for(i=0;i<crmfrm.elements.length;i++){
        var elm = crmfrm.elements[i];
        var regx = new RegExp("([0-1][0-9]|[2][0-3]):([0-5][0-9]):([0-5][0-9])");
        var time = regx.exec(elm.value);
        if(elm.getAttribute("format") && time !==null){
            var hr=parseInt(RegExp.$1);
            var ampm;
            ampm= (hr>11)?"pm":"am"; //NO I18N
            hr=(hr>11)?(hr-12):hr;
            hr=(hr===0)?(12):hr;
            document.getElementsByName(elm.name)[0].value=elm.value.replace(time[0],"");;
            document.getElementsByName(elm.name+"minute")[0].value=RegExp.$2;
            document.getElementsByName(elm.name+"hour")[0].value=""+hr;
            document.getElementsByName(elm.name+"ampm")[0].value=ampm;
        }
        var remElem = document.getElementById(elm.getAttribute("data-labelName")+"-error");
        if(remElem)
            remElem.parentNode.removeChild(remElem);
    }
    for(i=0;i<crmfrm.elements.length;i++){
        var elm = crmfrm.elements[i];
        var dataReqd = elm.getAttribute("data-required");
        if(dataReqd=="true"){
            var boll=true;
            var errmsg="";
            if(elm.value==""){
                errmsg="Enter a value for <strong>"+elm.getAttribute("data-labelName")+" </strong>";///NO I18N
                boll=false;
            }else if(elm.type =="checkbox" && elm.checked == false){
                errmsg="Please accept <strong>"+elm.getAttribute("data-labelName")+" </strong>";///NO I18N
                boll=false;
            }else if(elm.nodeName=="SELECT" && (elm.options[elm.selectedIndex].text=="-None-" || elm.options[elm.selectedIndex].text=="-Select-")){
                errmsg="<strong>"+elm.getAttribute("data-labelName")+" </strong> cannot be none";///NO I18N
                boll=false;
            }
            if(!boll && errmsg!=""){
                var errElem = document.createElement("div");
                errElem.id=elm.getAttribute("data-labelName")+"-error";
                errElem.tag="eleErr";///NO I18N
                errElem.innerHTML=errmsg;
                elm.parentNode.parentNode.appendChild(errElem);
                return false;
            }
        }
    }
}

captchaReload = function(crmfrm){
        var remElem=crmfrm.parentNode.getElementsByTagName('img')[0];
        if(remElem){
            if(remElem.src.indexOf('&d') !== -1 ){
                remElem.src=remElem.src.substring(0,remElem.src.indexOf('&d'))+'&d'+new Date().getTime();
            }else{
                remElem.src=remElem.src+'&d'+new Date().getTime();
            }
        }
        return false;
}

fnToggleSubmitBtnStatus = function(privacyElm) {
   var formName = privacyElm.id.split("_")[1]; //No I18N
    if(privacyElm.checked) {
        document.getElementById("submit_" + formName).disabled = false;
    } else {
        document.getElementById("submit_" + formName).disabled = true;
    }
}

fnResetCRMForm = function(e) {
    e.preventDefault();
    var formName = e.currentTarget.id.split("_")[1]; //No I18N
    var formEle = document.getElementById(formName);
    formEle.reset();
    if(formEle.querySelector("[name=privacyTool]")) {
        formEle.querySelector("[type=submit]").disabled = true; //NO I18N
    }
}

fnGetElementByAttribute = function(attrName, attrValue, tagName){
    var attrElem;
    var elems;
    var elemArr = document.getElementsByTagName(tagName);
    for(var j=0; j<elemArr.length; j++){
        elems = elemArr[j];
        var eleAttr = elems.getAttribute(attrName);
        if(eleAttr && eleAttr==attrValue){
            attrElem = elems;
            break;
        }
    }
    return attrElem;
}

fnDynamicContentSearch = function(dc, zpPosElem){
    var formDataElem = zpPosElem.getElementsByClassName("zpformdata")[0];
    formDataElem.style.overflow="auto";
    if((formDataElem.getAttribute("data-searchenable") == null || !formDataElem.getAttribute("data-searchenable")) && (formDataElem.getAttribute("data-viewcount")==null || formDataElem.getAttribute("data-viewcount")<10)){
        return;
    }
    var optArrays = [{"value":"10 Per Page", "id":"10"}, {"value":"20 Per Page", "id":"20"}, {"value":"30 Per Page", "id":"30"}, {"value":"40 Per Page", "id":"40"}, {"value":"50 Per Page", "id":"50"}];//NO I18N
    var dyncId = dc.id;
    var dyncTopElem = document.createElement("DIV");
    dyncTopElem.id = dyncId+"_dyViewTopDiv";
    dyncTopElem.style.position = "relative";
    var searchTable = document.createElement("table");
    searchTable.className = "zpDDSearchPage";
    searchTable.width = "100%";
    var searchTbody = document.createElement("tbody");
    searchTable.appendChild(searchTbody);
    var searchtr = document.createElement("tr");
    searchTbody.appendChild(searchtr);
    var searchth = document.createElement("th");
    searchth.className = "zpDSTableBorder";
    searchth.height = "25px";
    searchth.colSpan = "4";
    if(formDataElem.getAttribute("data-searchenable")!=null && formDataElem.getAttribute("data-searchenable")){
        var searchBoxIcon = document.createElement("div");
        searchBoxIcon.id = dyncId+"_searchIcon";
        searchBoxIcon.className="zpDSSearchContainer";
        searchBoxIcon.onclick = fnShowDyViewSearch;
        searchth.appendChild(searchBoxIcon);
        fnConstructSearchDiv(dyncId, dyncTopElem, zpPosElem);
    }
    if(formDataElem.getAttribute("data-viewcount")!=null && formDataElem.getAttribute("data-viewcount")>10){
        var searchPageDiv = document.createElement("div");
        searchPageDiv.className = "zpDSfloatRight";
        searchPageDiv.appendChild(document.createTextNode("Show : "));//NO I18N
        var pgNaSeltElem = document.createElement("select");
        pgNaSeltElem.id = dyncId+"_pagenationoption";
        pgNaSeltElem.className = "zpDSPagePerOption";
        var pgNaOptElem = pgNaSeltElem.options;
        for(var i=0; i<optArrays.length; i++){
            var optArr = optArrays[i]
                pgNaOptElem[i] = new Option(optArr.value, optArr.id, false, false);
        }
    pgNaSeltElem.onchange = fnPageNationSel;
        searchPageDiv.appendChild(pgNaSeltElem);
        searchth.appendChild(searchPageDiv);
    }
    searchtr.appendChild(searchth);
    dyncTopElem.appendChild(searchTable);
    zpPosElem.insertBefore(dyncTopElem, zpPosElem.childNodes[0]);
    fnConstructDyViewPageNation(dyncId, zpPosElem);
}

fnConstructSearchDiv = function(dyncId, parentElem, zpPosElem){
    //Search div
    var searchDiv = document.createElement("div");
    searchDiv.className = "zpDSContainer";
    searchDiv.id = dyncId+"_searchDiv"
        searchDiv.appendChild(fnSearchRadioElement(dyncId));
    //search criteria element
    var criteriaDiv = document.createElement("div");
    criteriaDiv.className = "zpViewSearchCrit";
    //Field name select element
    var fieldList = zpPosElem.getAttribute("data-formfields").split("|");
    var fieldSelElem = document.createElement("select");
    fieldSelElem.className="zpsearchviewfield";
    var opt = new Option("-Fields-", "none", false, false);//NO I18N
    fieldSelElem.appendChild(opt);
    for(var i=0; i <fieldList.length; i++){
        var fieldData = fieldList[i].split(",");
        opt = new Option(fieldData[0], fieldData[1], false, false);
        fieldSelElem.appendChild(opt);
    }
    criteriaDiv.appendChild(fieldSelElem);
    //Criteria select element
    criteriaDiv.appendChild(viewSearchCritElem());
    //Search text field element
    var textElem = document.createElement("input");
    textElem.type = "text";
    textElem.className = "zpsearchviewtxt";
    textElem.placeholder = "Search term";//NO I18N
    criteriaDiv.appendChild(textElem);
    //Add link element
    var addLinkElem = document.createElement("span");
    addLinkElem.className = "zpAddMore";
    addLinkElem.onclick = fnAddNewCriteria;
    var addAnchor = document.createElement("a");
    addAnchor.innerHTML = "Add";//NO I18N
    addLinkElem.appendChild(addAnchor);
    criteriaDiv.appendChild(addLinkElem);
    searchDiv.appendChild(criteriaDiv);
    //Search button element
    var searchBtnElem = document.createElement("button");
    searchBtnElem.id = dyncId+"_searchbtn";
    var txtNode = document.createTextNode("Search");//NO I18N
    searchBtnElem.appendChild(txtNode);
    searchBtnElem.onclick = fnSearchDynamicView;
    searchDiv.appendChild(searchBtnElem);
    parentElem.appendChild(searchDiv);
}

fnConstructDyViewPageNation = function(dyncId, zpPosElem){
    var viewCount = zpPosElem.getElementsByClassName("zpformdata")[0].getAttribute("data-viewcount");//NO I18N
    if(viewCount==null || viewCount<10)
        return;
    zpPosElem.setAttribute("data-viewcount", viewCount);
    zpPosElem.setAttribute("data-startindex", "1");
    var pgNavDiv = document.createElement("div");
    pgNavDiv.id = dyncId+"_viewPgNation";
    var pgNavTable = document.createElement("table");
    pgNavTable.className = "zpDDSearchPage";
    pgNavTable.width = "100%";
    pgNavDiv.appendChild(pgNavTable);
    var pgNavTbody = document.createElement("tbody");
    pgNavTable.appendChild(pgNavTbody);
    var pgNavTableRow = document.createElement("tr");
    pgNavTbody.appendChild(pgNavTableRow);
    var pgNavTableth = document.createElement("th");
    pgNavTableth.className = "zpDSTableBorder";
    pgNavTableth.height = "25px";
    pgNavTableth.colSpan = "4";
    pgNavTableRow.appendChild(pgNavTableth);
    var pgNavContainer = document.createElement("div");
    pgNavContainer.className = "zpDSfloatRight";
    pgNavTableth.appendChild(pgNavContainer);
    var pgNavPreviousElem = document.createElement("span");
    pgNavPreviousElem.id = dyncId+"_previousPage";
    pgNavPreviousElem.className = "zpDSPrevNext";
    pgNavPreviousElem.innerHTML = "Prev";//NO I18N
    pgNavPreviousElem.onclick = fnDyViewPreviousPage;
    pgNavContainer.appendChild(pgNavPreviousElem);
    var pgNavNextElem = document.createElement("span");
    pgNavNextElem.id = dyncId+"_nextPage";
    pgNavNextElem.className = "zpDSPrevNext";
    pgNavNextElem.innerHTML = "Next";//NO I18N
    pgNavNextElem.onclick = fnDyViewNextPage;
    pgNavContainer.appendChild(pgNavNextElem);
    zpPosElem.appendChild(pgNavDiv);
}

fnSearchRadioElement = function(eleId){
    var radioList = [{"value":"or", "name":"Any of these Conditions"}, {"value":"and", "name":"All these Conditions"}];//NO I18N
    var critOpElem = document.createElement("div");
    critOpElem.className = "zpCriteriaOperation";
    for(var i=0; i<radioList.length; i++){
        var radioObj = radioList[i];
        var radioElem = document.createElement("input");
        radioElem.type = "radio";
        radioElem.name = eleId + "_zpCritOp";
        //radioElem.id = eleId + "_zpCritOp";
        radioElem.value = radioObj.value;
        radioElem.className = "zpCritOp";
        if(i==0)
            radioElem.setAttribute("checked", "checked");
        critOpElem.appendChild(radioElem);
        var radioLabel = document.createElement("label");
        radioLabel.className = "zpCritLabel";
        radioLabel.innerHTML = radioObj.name;
        critOpElem.appendChild(radioLabel);
    }
    return critOpElem;
}

viewSearchCritElem = function(){
    var critArr = [{"displayName":"Equals", "value":"equals"}, {"displayName":"Not equal to", "value":"notequals"}, {"displayName":"Starts With", "value":"startsWith"}, {"displayName":"Ends With", "value":"endsWith"}, {"displayName":"Contains", "value":"contains"}, {"displayName":"Does not contains", "value":"notcontains"}];//NO I18N
    var critSelElem = document.createElement("select");
    critSelElem.className = "zpsearchviewcrit";
    var opt = new Option("-Operator-", "none", false, false);//NO I18N
    critSelElem.appendChild(opt);
    for(var i=0; i<critArr.length; i++){
        opt = new Option(critArr[i].displayName, critArr[i].value, false, false);
        critSelElem.appendChild(opt);
    }
    return critSelElem;
}

fnShowDyViewSearch = function(){
    var dyncId = this.id.replace("_searchIcon", "");
    var dyViewSearchElem = document.getElementById(dyncId+"_searchDiv");
    dyViewSearchElem.style.right = (parseInt(this.parentNode.offsetWidth)-(parseInt(this.offsetLeft)+parseInt(this.offsetWidth)))+"px";//NO I18N
    dyViewSearchElem.style.top = (parseInt(this.offsetTop)+parseInt(this.offsetHeight)+3)+"px";
    if(dyViewSearchElem.style.display != "block"){
        dyViewSearchElem.style.display = "block";
    } else{
        dyViewSearchElem.style.display = "none";
    }
}

fnAddNewCriteria = function(){
    var critElem = this.parentNode;
    var criteriaDiv = document.createElement("div");
    criteriaDiv.className = "zpViewSearchCrit";
    //Field element
    var fieldCloneElem = getClasses("zpsearchviewfield", "div", critElem)[0].cloneNode(true);//NO I18N
    criteriaDiv.appendChild(fieldCloneElem);
    //operator element
    criteriaDiv.appendChild(viewSearchCritElem());
    //Text element
    var textElem = document.createElement("input");
    textElem.type = "text";
    textElem.className = "zpsearchviewtxt";
    textElem.placeholder = "Search term";//NO I18N
    criteriaDiv.appendChild(textElem);
    criteriaDiv.appendChild(this);
    critElem.parentNode.insertBefore(criteriaDiv, critElem.nextSibling);

    //Add remove criteria link
    var removeLinkElem = document.createElement("span");
    removeLinkElem.onclick = fnRemoveCriteria;
    removeLinkElem.appendChild(document.createTextNode("Remove"));//NO I18N
    critElem.appendChild(removeLinkElem);
}

fnRemoveCriteria = function(){
    var critElem = this.parentNode;
    critElem.parentNode.removeChild(critElem);
}

fnSearchDynamicView = function(){
    var dyncId = this.id.replace("_searchbtn", "");
    var dyncElem = document.getElementById(dyncId);
    var zpPos = getClasses("zpAlignPos", "div", dyncElem);//NO I18N
    var searchParam = fnGetSearchCriteria(dyncId);
    zpPos[0].setAttribute("data-searchResult", "true");
    zpPos[0].setAttribute("data-searchbtnclick", "true");
    fnSearchDynamicViewReq(zpPos[0], dyncId, searchParam);
}

fnGetViewSearchCriteria = function(fieldVal, criteriaVal, textVal){
    var searchCriteria = "";
    if(criteriaVal == "contains" || criteriaVal == "endsWith" || criteriaVal == "startsWith" || criteriaVal == "notcontains"){
        searchCriteria = "(" + fieldVal + "." + criteriaVal + "(\"" + textVal + "\"))";
    } else if(criteriaVal == "equals" || criteriaVal == "notequals"){
        operatorVal = (criteriaVal == "equals") ? "==" : "!=";//NO I18N
        searchCriteria = "(" + fieldVal + operatorVal +"\"" + textVal + "\")";
    }
    return searchCriteria;
}

fnGetSearchCriteria = function(dyncId){
    var searchElem = document.getElementById(dyncId+"_searchDiv");
    var criteriaElem = getClasses("zpViewSearchCrit", "div", searchElem);//NO I18N
    var searchCriteria = new Array();
    for(var i=0; i<criteriaElem.length; i++){
        var fieldSelElem = getClasses("zpsearchviewfield", "select", criteriaElem[i])[0];
        var fieldValue = fieldSelElem.options[fieldSelElem.selectedIndex].value;
        var critSelElem = getClasses("zpsearchviewcrit", "select", criteriaElem[i])[0];
        var critValue = critSelElem.options[critSelElem.selectedIndex].value;
        var txtValue = getClasses("zpsearchviewtxt", "input", criteriaElem[i])[0].value;
        searchCriteria.push(fnGetViewSearchCriteria(fieldValue, critValue, txtValue));
    }
    var critOperator;
    var radioList = searchElem.getElementsByClassName("zpCritOp");//NO I18N
    for(var j=0; j<radioList.length; j++){
        if(radioList[j].checked){
            critOperator = radioList[j].value;
            break;
        }
    }
    return "&criteria=" + searchCriteria + "&operator=" + critOperator;//NO I18N
}

fnDyViewPreviousPage = function(){
    var dyncId = this.id.replace("_previousPage", "");
    var dyncElem = document.getElementById(dyncId);
    var zpPos = getClasses("zpAlignPos", "div", dyncElem);//NO I18N
    var startIndex = zpPos[0].getAttribute("data-startindex");
    var pageNationOptElem = document.getElementById(dyncId+"_pagenationoption");
    var limit = parseInt(pageNationOptElem.options[pageNationOptElem.selectedIndex].value);
    if(startIndex == 1){
        alert("First Page");
        return;
    }
    startIndex = parseInt(startIndex)-limit;
    zpPos[0].setAttribute("data-startindex", startIndex);
    var searchParams = "&startindex=" + startIndex + "&limit=" + limit;//NO I18N
    if(zpPos[0].getAttribute("data-searchResult") == null){
        fnSearchDynamicViewReq(zpPos[0], dyncId, searchParams);
    } else{
        fnSearchDynamicViewReq(zpPos[0], dyncId, searchParams, true);
    }
}

fnDyViewNextPage = function(){
    var dyncId = this.id.replace("_nextPage", "");
    var dyncElem = document.getElementById(dyncId);
    var zpPos = getClasses("zpAlignPos", "div", dyncElem);//NO I18N
    var viewCount = zpPos[0].getAttribute("data-viewcount");
    var startIndex = zpPos[0].getAttribute("data-startindex");
    var pageNationOptElem = document.getElementById(dyncId+"_pagenationoption");
    var limit = parseInt(pageNationOptElem.options[pageNationOptElem.selectedIndex].value);
    startIndex = limit + parseInt(startIndex);
    if(startIndex > viewCount){
        alert("Last Page");
        return;
    }
    zpPos[0].setAttribute("data-startindex", startIndex);
    var searchParams = "&startindex=" + startIndex + "&limit=" + limit;//NO I18N
    if(zpPos[0].getAttribute("data-searchResult") == null){
        fnSearchDynamicViewReq(zpPos[0], dyncId, searchParams);
    } else{
        fnSearchDynamicViewReq(zpPos[0], dyncId, searchParams, true);
    }
}

fnPageNationSel = function(){
    var dyncId = this.id.replace("_pagenationoption", "");
    var dyncElem = document.getElementById(dyncId);
    var zpPos = getClasses("zpAlignPos", "div", dyncElem);//NO I18N
    zpPos[0].setAttribute("data-startindex", "1");
    var limit = parseInt(this.options[this.selectedIndex].value);
    var searchParams = "&startindex=1&limit=" + limit;//NO I18N
    if(zpPos[0].getAttribute("data-searchResult") == null){
        fnSearchDynamicViewReq(zpPos[0], dyncId, searchParams);
    } else{
        fnSearchDynamicViewReq(zpPos[0], dyncId, searchParams, true);
    }
}

fnSearchDynamicViewReq = function(resultElem, dyncId, searchParam, pgNationSchEnable){
    if(window.ZS_PreviewMode){
        return;
    }
    var applicationName = resultElem.getAttribute("data-applicationname");
    var formName = resultElem.getAttribute("data-formname");
    var viewName = resultElem.getAttribute("data-templateid").replace("dyncont", "");
    if(pgNationSchEnable){
        var searchStr = fnGetSearchCriteria(dyncId);
        searchParam += searchStr;
    }
    try{
        if(window.XMLHttpRequest)
            xmlhttp = new XMLHttpRequest();
        else
            xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");

        xmlhttp.onreadystatechange = function(){
            if(xmlhttp.readyState ==4 && xmlhttp.status == 200){
                if(this.responseText=="No result"){
                    alert("Your search criteria did not match");
                    return;
                }
                var formData=document.createElement("div");
                formData.innerHTML=this.responseText;
                var parent = document.getElementById(dyncId);
                var div = parent.getElementsByClassName("zpAlignPos");
                div[0].getElementsByClassName("zpformdata")[0].innerHTML=formData.innerHTML;
                var formDataElem = resultElem.getElementsByClassName("zpformdata")[0];
                formDataElem.style.overflow="auto";
                if(formDataElem.getAttribute("data-viewcount") != null){
                    var viewCount = formDataElem.getAttribute("data-viewcount");
                    resultElem.setAttribute("data-viewcount", viewCount);
                    if(resultElem.getAttribute("data-searchbtnclick") != null){
                        resultElem.setAttribute("data-startindex", "1");
                        resultElem.removeAttribute("data-searchbtnclick");
                    }
                }
            }
        }
        xmlhttp.open("GET", "/siteapps/searchView?applicationName=" + applicationName + "&viewName=" + viewName + "&formName=" + formName + searchParam, true);
        xmlhttp.send();
    }catch(e){}
}

/**
 * Code for commentbox - start
 */
addCBComments = function(arg)
{
    var eltCont = findParent(arg, "zpelement-wrapper commentbox");//NO I18N
    cbElm = eltCont.id;

    var resDiv = document.getElementById(cbElm+"_resultsDiv");
    if(resDiv) {
    resDiv.style.display="none";
    }

    var actualPath = location.pathname;
    var path = actualPath.substr(1,actualPath.lastIndexOf(".")-1);
    if(window.location.host.indexOf("sitepreview") !== -1 || window.location.host.indexOf("siteipreview") !== -1 || path.indexOf("preview/") !== -1) {
        alert("Comment Box will work only on published website");
        return false;
    }

    var cbName = document.getElementById("cbName"+cbElm).value;
    var cbEmail = document.getElementById("cbEmail"+cbElm).value;
    var comments = document.getElementById("cbComment"+cbElm).value;

    var cbMsg = document.getElementById("cbInfoMsg-"+cbElm);
    
    var rFlg = false;
    var rCont = document.getElementById("cbRatingContainer-"+cbElm);
    if(rCont) {
        rFlg = rCont.hasAttribute("curRating"); //No I18N
    }
    
    if((cbDet.CBVisible == "1" && cbDet.RatingVisible == "1") && (comments == "" && !rFlg)) {
        cbMsg.innerHTML="Provide either rating or comment or both to submit the data.";//No I18N
        setTimeout(function() {
            cbMsg.innerHTML = "";
        },4000)
        return false;
    }
    else if(cbDet.CBVisible == "1" && cbDet.RatingVisible == "0" && comments == "") {
        cbMsg.innerHTML="Please add a comment to submit.";//No I18N
        setTimeout(function() {
            cbMsg.innerHTML = "";
        },4000)
        return false;
    }
    cbName = (cbName == "") ? "Guest" : cbName;
    if(cbEmail != "") {
        var mailPattern = new RegExp("^[a-zA-Z0-9]([\\w\\-\\.\\+\']*)@([\\w\\-\\.]*)(\\.[a-zA-Z]{2,8}(\\.[a-zA-Z]{2}){0,2})$","mig");
        var reg_mail = mailPattern.exec(cbEmail);
        if(!reg_mail){
            cbMsg.innerHTML="Provide a proper email-id";//No I18N
            setTimeout(function() {
                cbMsg.innerHTML = "";
            },4000)
            return false;
        }
    }
    path = path.replace("mobile/","");
    if(path.indexOf("siteapps/protected/") != -1) {
      path = path.replace("siteapps/protected/","");
    }
    
    var paramsdata = "cbName="+encodeURIComponent(cbName);  //No I18N
    var voteVal;
    if(cbDet.CBVisible == "1" && cbDet.RatingVisible == "1") {
        if(rFlg) {
            voteVal = rCont.getAttribute("curRating");
            paramsdata = paramsdata + "&rateVal="+voteVal;  //NO I18N
            rCont.removeAttribute("curRating");
        }
    }
    
        setBrowserCookie();
//    if(comments == "" && rFlg) {
//        setBrowserCookie();
//    }
    
    var randVal = getBrowserCookie();
    var paramsdata = paramsdata + "&cbEmailid="+cbEmail+"&comments="+encodeURIComponent(comments)+"&path="+path+"&randVal="+randVal;//No I18N
    
    var ie = /*@cc_on!@*/false;
    if(!ie){
        xml("POST","/siteapps/commentbox/addCBComment",addCBCommentCallback,paramsdata);//No I18N
    }else{
        xml("POST","/siteapps/commentbox/addCBComment",addCBCommentCallback,paramsdata);//No I18N
    }
    return false;
}

cbFormReset = function(arg) {
    var eltCont = findParent(arg, "zpelement-wrapper commentbox");//NO I18N
    cbElm = eltCont.id;

    var rCont = document.getElementById("cbRatingContainer-"+cbElm);
    if(rCont.hasAttribute("curRating")) {
        rCont.removeAttribute("curRating");
    }
}

addCBCommentCallback = function(res){
    if(res == "-1") {
        alert("Problem while submitting. Please reload the page");
        
        document.getElementById("cbName"+cbElm).value="";
        document.getElementById("cbEmail"+cbElm).value="";
        document.getElementById("cbComment"+cbElm).value="";
        
        var rCont = document.getElementById("cbRatingContainer-"+cbElm);
        if(rCont.hasAttribute("curRating")) {
            rCont.removeAttribute("curRating");
        }
        
        return false;
    }
    
    var result=JSON.parse(res);
    
    document.getElementById("cbName"+cbElm).value="";
    document.getElementById("cbEmail"+cbElm).value="";
    document.getElementById("cbComment"+cbElm).value="";

    cbDet = result.CB_DET;
    var cbRatingHTML = result.CB_RATING_HTML;

    var cbComm = result.CB_COMMENTS_CONTENT;

    var commExist = (typeof cbComm != 'undefined' && cbComm != "") ? true : false;
    
    if(commExist) {
        commentsArr.unshift(result);
    }

    if(cbGlobal.MODERATE_COMMENTS === 1 && (commExist)){
        alert("Your comments will be displayed only after moderation");
    }
    else if(cbGlobal.MODERATE_COMMENTS === 0){
        if(commExist) {
            noOfRows = noOfRows + 1;
            for(var i = 0; i < commentBoxElm.length; i++) {
                cbElm = commentBoxElm[i];
                elmId = cbElm.id;
                if(noOfRows === 1) {
                        var cbCommNode = document.getElementById("cbCommentsContainer-"+elmId);
                    while (cbCommNode.firstChild) {
                        cbCommNode.removeChild(cbCommNode.firstChild);
                    }
                }
                cbCBCommentHTML(res, true, elmId);
            }
        }
    }

    for(var i = 0; i < commentBoxElm.length; i++) {
        cbElm = commentBoxElm[i];
        elmId = cbElm.id;

        cbNavig[elmId]={};
        cbNavig[elmId].STARTVAL = 1;
        cbNavig[elmId].ENDVAL = (noOfRows < cbGlobal["MAX_EXTRIES"]) ? noOfRows : cbGlobal["MAX_EXTRIES"];

        cbNavig[elmId].FIRSTVAL = 0;
        cbNavig[elmId].PREVVAL = 0;
        cbNavig[elmId].NEXTVAL = 0;
        cbNavig[elmId].LASTVAL = 0;

        if(noOfRows > cbGlobal["MAX_EXTRIES"]) {
            getPaginationValues(cbNavig[elmId].STARTVAL, noOfRows, elmId);
            constructComments(cbNavig[elmId].STARTVAL, cbNavig[elmId].ENDVAL, elmId);
            var pnId = document.getElementById("prevNextId"+elmId);
            pnId.style.display="block";
            pnId.innerHTML="<span class=\"zpinactivePrevNext\" id=\"prev-"+elmId+"\" onclick=\"fnPageNavigate(this)\"><p>Prev</p></span><span class=\"zpactivePrevNext\" id=\"next-"+elmId+"\" onclick=\"fnPageNavigate(this)\"><p>Next</p></span>";
            document.getElementById("prev-"+elmId).innerHTML="<p>Prev</p>";     //No I18N
            document.getElementById("next-"+elmId).innerHTML="<a href='javascript:;'>Next</a>";
        }

        if(cbDet.CBVisible == "1" && cbDet.RatingVisible == "1") {
            cbRatingHTML = cbRatingHTML.replace("rInfoIcon-","rInfoIcon-"+elmId);
            document.getElementById("cbRatingContainer-"+elmId).innerHTML = "";
            document.getElementById("cbRatingContainer-"+elmId).innerHTML = cbRatingHTML;
        }
        else if(cbDet.CBVisible == "0" && cbDet.RatingVisible == "1") {
            cbRatingHTML = cbRatingHTML.replace("rInfoIcon-","rInfoIcon-"+elmId);
            document.getElementById("cbRatingContainer-"+elmId).innerHTML = "";
            document.getElementById("cbRatingContainer-"+elmId).innerHTML = cbRatingHTML;
        }

        if(cbRatingHTML) {
            var rType = cbDet.RatingTypeId.substring(0,1);
            var cbLiId = document.getElementById("ratingFormId-"+elmId);
            if(rType == "1") {
            var cbInfoId = document.getElementById("rInfoIcon-"+elmId);
                if(cbInfoId) {
                    cbInfoId.onclick=showHideRateResults;
                }
            }

            var resultsDiv = document.createElement("div");
            resultsDiv.className = "zpDSContainer";
            resultsDiv.id = elmId+"_resultsDiv";
            cbLiId.parentNode.appendChild(resultsDiv);
        }
        if(result.CB_RATING_RESULT) {
            var resDiv = document.getElementById(elmId+"_resultsDiv");
            if(resDiv) {
                resDiv.innerHTML=result.CB_RATING_RESULT;
            }
        }
        if(inpElm) {
            inpElm.checked=false;
        }
    }
}

cbCBCommentHTML = function(res, arg, elm){
    var result=JSON.parse(res);
    var newElem = document.createElement("div");
    newElem.className="commentPadBottom";//No I18N
    newElem.style.clear="both"; //NO I18N

    var d1 = document.createElement("div");
    d1.id="cbCommRateCont-"+result.CB_COMMENT_ID;
    var cmtRateRes = result.CB_CMT_RATING;
    if(cmtRateRes){
    if(cmtRateRes.trim().length == 0) {
        var nDivElm = document.createElement("div");
        nDivElm.style.display="block";
        var pEl1 = document.createElement("p");
        pEl1.className="zs-text-highlight-color";
        pEl1.appendChild(document.createTextNode(result.CB_COMMENTS_ADDED_BY));
        nDivElm.appendChild(pEl1);
        d1.appendChild(nDivElm);       
    }                     
    else {                
        d1.innerHTML=cmtRateRes;
    }
    newElem.appendChild(d1);
    }
    var spElem = document.createElement("span");
    spElem.className="commentDateStyle zs-text-light-color";
    spElem.style.marginTop = "3px"; //No I18N
    spElem.style.display="block";
    spElem.innerHTML="Posted on : "+result.CB_COMMENTS_ADDED_TIME;  //No I18N
    
    var d2 = document.createElement("div");
    var pElem = document.createElement("p");
    pElem.className="commentTxtStyle";
    pElem.innerHTML=result.CB_COMMENTS_CONTENT.replace(/\r?\n/g, '<br/>');
    d2.appendChild(pElem);
    var hrElem = document.createElement("hr");
    hrElem.size="1";
    newElem.appendChild(spElem);
    newElem.appendChild(d2);
    newElem.appendChild(hrElem);
    var elem = document.getElementById("cbCommentsContainer-"+elm);
    if(arg) {
        elem.insertBefore(newElem, elem.firstChild);
    } else {
        elem.appendChild(newElem);
    }
}

renderCBRateSVG =function() {
    xml("GET","/zimages/cbRate.svg",renderCBRateSVGRes);//No I18N
}

renderCBRateSVGRes = function(res1) {
    var div = document.createElement("div");
    div.innerHTML = res1;
    div.style.display="none";
    document.body.insertBefore(div, document.body.childNodes[0]);
      
    loadCBComments();
}

loadCBComments = function(cbElm)
{
    var actualPath = location.pathname;
    var randVal = getBrowserCookie();

    if(window.location.host.indexOf("sitepreview") !== -1 || window.location.host.indexOf("siteipreview") !== -1 ) {
        for(var i = 0; i < commentBoxElm.length; i++) {
            cbElm = commentBoxElm[i];
            elmId = cbElm.id;
            document.getElementById(elmId).style.display="block";
        }
    }

    var path = actualPath.substr(1,actualPath.lastIndexOf(".")-1);
    path = path.replace("mobile/","");
    if(path.indexOf("siteapps/protected/") != -1) {
      path = path.replace("siteapps/protected/","");
    }

    var paramsdata = "?path="+path+"&randVal="+randVal;//No I18N

    xml("GET","/siteapps/commentbox/generateSiteCBComments"+paramsdata,loadCBCommentCallback);//No I18N
}

loadCBCommentCallback = function(res)
{
    if(res == "false") {
       return;
    }
    else {
        var resp = JSON.parse(res);

        cbGlobal["MAX_EXTRIES"] = resp["COMMENTS_PERPAGE"];
        cbGlobal["MODERATE_COMMENTS"] = resp["MODERATE_COMMENTS"];

        commentsArr = resp.CB_COMMENTS;
        noOfRows = commentsArr.length;

        var cbResults = resp.CB_RESULTS;
        var cbRatingHTML = cbRatingHTMLOrg = resp.CB_RATING_HTML;
        var cbRatingHTMLBot = resp.CB_RATING_HTML_BOT;
        cbCRHTML = resp.CB_CMT_RATING;
        
        cbDet = resp.CB_DET;
        var rType = cbDet.RatingTypeId.substring(0,1);
        
        for(var i = 0; i < commentBoxElm.length; i++) {
            cbElm = commentBoxElm[i];
            elmId = cbElm.id;
            
            var cbElmCont = document.getElementById(elmId);
            cbElmCont.style.display="block";
            
            var cbCont = document.getElementById("cbContainer-"+elmId);
        if(cbCont) {
            cbCont.style.display="block";
        }

            if(cbDet.CBVisible == "0") {
                cbCont.style.display="none";
            }
            
            var rBotCont = document.getElementById("cbRatingContainerBot-"+elmId);
        if(rBotCont) {
            rBotCont.style.display = "block";
            }
            if(cbDet.CBVisible == "1" && cbDet.RatingVisible == "1") {
            cbRatingHTML = cbRatingHTML.replace("rInfoIcon-","rInfoIcon-"+elmId);
                document.getElementById("cbRatingContainer-"+elmId).innerHTML = cbRatingHTML;
                rBotCont.innerHTML = cbRatingHTMLBot;
            }
            else if(cbDet.CBVisible == "0" && cbDet.RatingVisible == "1") {
                cbRatingHTML = cbRatingHTML.replace("rInfoIcon-","rInfoIcon-"+elmId);
                document.getElementById("cbRatingContainer-"+elmId).innerHTML = cbRatingHTML;
                rBotCont.innerHTML = "";
            }
            cbRatingHTML = cbRatingHTMLOrg;

            cbNavig[elmId]={};
            cbNavig[elmId].STARTVAL = 1;
            cbNavig[elmId].ENDVAL = (noOfRows < cbGlobal["MAX_EXTRIES"]) ? noOfRows : cbGlobal["MAX_EXTRIES"];

            cbNavig[elmId].FIRSTVAL = 0;
            cbNavig[elmId].PREVVAL = 0;
            cbNavig[elmId].NEXTVAL = 0;
            cbNavig[elmId].LASTVAL = 0;

            
            getPaginationValues(cbNavig[elmId].STARTVAL, noOfRows, elmId);
            constructComments(cbNavig[elmId].STARTVAL, cbNavig[elmId].ENDVAL, elmId);
            if(noOfRows > cbGlobal.MAX_EXTRIES) {
                var pnId = document.getElementById("prevNextId"+elmId);
                pnId.style.display="block";
                pnId.innerHTML="<span class=\"zpinactivePrevNext\" id=\"prev-"+elmId+"\" onclick=\"fnPageNavigate(this)\"><p>Prev</p></span><span class=\"zpactivePrevNext\" id=\"next-"+elmId+"\" onclick=\"fnPageNavigate(this)\"><p>Next</p></span>";
                document.getElementById("prev-"+elmId).innerHTML="<p>Prev</p>"; //No I18N
                document.getElementById("next-"+elmId).innerHTML="<a href='javascript:;'>Next</a>";
            }
            var cbFormCont = document.getElementById("cbFormContainer-"+elmId);
            if(cbFormCont) {
                cbFormCont.style.display="block";
                }
            //document.getElementById("cbFormContainer-"+elmId).style.display="block";
            //document.getElementById("commentsTitle-"+elmId).style.display="block";
            var cbCommTitle = document.getElementById("commentsTitle-"+elmId);
            if(cbCommTitle) {
                cbCommTitle.style.display="block";
            }
            if(cbDet.IsClosed == "0" && cbFormCont && cbCommTitle) {    
                cbFormCont.style.display="none";
                if (noOfRows == 0 && cbCommTitle) {
                    cbCommTitle.style.display="none";
                }
            }
            
            if(cbRatingHTML) {
                var cbLiId = document.getElementById("ratingFormId-"+elmId);
                if(rType == "1") {
                var cbInfoId = document.getElementById("rInfoIcon-"+elmId);
                    if(cbInfoId) {
                cbInfoId.onclick=showHideRateResults;
                    }
                }
                var resultsDiv = document.createElement("div");
                resultsDiv.className = "zpDSContainer";
                resultsDiv.id = elmId+"_resultsDiv";
                cbLiId.parentNode.appendChild(resultsDiv);

                var resDiv = document.getElementById(elmId+"_resultsDiv");
                if(resDiv) {
                    resDiv.innerHTML=resp.CB_RATING_RESULT;
                }
            }
            if (noOfRows == 0) {
                if(cbDet.IsClosed == "1") {
                    var newElem = document.createElement("div");
                    newElem.className="commentPadBottom";//No I18N
                    var p1Elem = document.createElement("p");
                    p1Elem.className="zs-text-highlight-color";
                    var spElem = document.createElement("span");
                    spElem.className="commentDateStyle zs-text-light-color";
                    spElem.style.marginTop = "3px"; //No I18N
                    spElem.style.display="block";
                    var pElem = document.createElement("p");
                    pElem.className="commentTxtStyle";
                    pElem.innerHTML="Be the first to comment ...";  //No I18N
                    newElem.appendChild(p1Elem);
                    newElem.appendChild(spElem);
                    newElem.appendChild(pElem);
                    var elem = document.getElementById("cbCommentsContainer-"+elmId);
                    elem.appendChild(newElem);
                }
            }
        }
    }
}
            
showHideRateResults = function() {
    var actualPath = location.pathname;
    var path = actualPath.substr(1,actualPath.lastIndexOf(".")-1);
    if(window.location.host.indexOf("sitepreview") !== -1 || window.location.host.indexOf("siteipreview") !== -1  || path.indexOf("preview/") !== -1) {
        alert("You can view the rating results only on the published site.");
        return false;
    }
    var elmId = this.id.replace("rInfoIcon-", "");
    
    var ratingResultsElem = document.getElementById(elmId+"_resultsDiv");
    
    if(ratingResultsElem.style.display != "block"){
        ratingResultsElem.style.display = "block";
        
        var rtCloseDial = ratingResultsElem.getElementsByClassName("rating-result-close")[0];
        rtCloseDial.addEventListener('click', function () {
            ratingResultsElem.style.display = "none";
        });
    } else{
        ratingResultsElem.style.display = "none";
    }
    
    if(mobile && ratingResultsElem.style.display == 'block' && !istab) {
        var wW = window.innerWidth || document.documentElement.clientWidth;
    
        ratingResultsElem.style.position = "absolute";
        var whw = ((wW)/2);
        var divW = ((ratingResultsElem.clientWidth)/2);
        var left = ((wW)/2) - ((ratingResultsElem.clientWidth)/2);
        ratingResultsElem.style.left = parseInt(left)+"px";//NO I18N
        ratingResultsElem.style.top = (parseInt(this.offsetTop)+parseInt(this.offsetHeight)+3)+"px";//NO I18N
}
    else {
        ratingResultsElem.style.left = (parseInt(this.offsetLeft)+parseInt(this.offsetWidth)+5)+"px";//NO I18N
        ratingResultsElem.style.top = (parseInt(this.offsetTop)+parseInt(this.offsetHeight)+3)+"px";
    }
}


 fnPageNavigate = function (e)
 {
    var obj = e;
    var elem;
    while(obj.parentNode) {
        obj = obj.parentNode;
        if(obj.className == "zpelement-wrapper commentbox") {
            elem = obj.id;
            break;
        }
    }
     
     var navId = e.id.replace("-"+elem,"");
     var showNavig;

     if(noOfRows > commentsArr.length) {
         loadCBComments();
     }
     noOfRows = commentsArr.length;   //actual

    var pElm = document.getElementById("prev-"+elem);
    var nElm = document.getElementById("next-"+elem);
    if(noOfRows > cbGlobal["MAX_EXTRIES"]) {
        pElm.className="zpactivePrevNext";
        pElm.innerHTML="<a href='javascript:;'>Prev</a>";
        nElm.className="zpactivePrevNext";
        nElm.innerHTML="<a href='javascript:;'>Next</a>";
    }
     if(navId == 'prev')
     {
         if(cbNavig[elem].STARTVAL != 1) {
             cbNavig[elem].STARTVAL = cbNavig[elem].PREVVAL;
             cbNavig[elem].ENDVAL = cbNavig[elem].FIRSTVAL-1;
         }
         getPaginationValues(cbNavig[elem].STARTVAL, noOfRows, elem);
         constructComments(cbNavig[elem].STARTVAL, cbNavig[elem].ENDVAL, elem);
    if(cbNavig[elem].STARTVAL == 1)
    {
            pElm.className="zpinactivePrevNext";
            pElm.innerHTML="<p>Prev</p>";   //No I18N
        }
     }
     else if(navId == 'next')
     {
         if(cbNavig[elem].LASTVAL != noOfRows)
         {
             cbNavig[elem].STARTVAL = cbNavig[elem].NEXTVAL;
             cbNavig[elem].ENDVAL = ((cbNavig[elem].NEXTVAL + cbGlobal["MAX_EXTRIES"]) > noOfRows) ? noOfRows : cbNavig[elem].NEXTVAL + cbGlobal["MAX_EXTRIES"]-1;
             getPaginationValues(cbNavig[elem].STARTVAL, noOfRows, elem);
             constructComments(cbNavig[elem].STARTVAL, cbNavig[elem].ENDVAL, elem);
         }
    if(cbNavig[elem].LASTVAL == noOfRows) {
            nElm.className="zpinactivePrevNext";
            nElm.innerHTML="<p>Next</p>";   //No I18N
        }
    }
}

function getPaginationValues(start, noOfRows, elm)
{
    var prevstartvalue;
    var endval;
    var nextstartvalue;

    var startToEndvalCount = cbGlobal["MAX_EXTRIES"] - 1;
    if(noOfRows > cbGlobal["MAX_EXTRIES"])
    {
        end = start + startToEndvalCount;
        if(end > noOfRows){
            end = noOfRows;
        }
        startvalue = 1;
        remainder = noOfRows % cbGlobal["MAX_EXTRIES"];
        if(remainder > 0){
            endval = noOfRows - remainder + 1;
        } else if(remainder == 0){
            endval = noOfRows - startToEndvalCount;
        }
    } else{
        end = noOfRows;
    }

    if(typeof start != 'undefined' && start != '')
    {
        tempnextstartvalue = start + cbGlobal["MAX_EXTRIES"];
        if(tempnextstartvalue <= noOfRows){
            nextstartvalue = tempnextstartvalue;
        }
        tempprevvalue = start - cbGlobal["MAX_EXTRIES"];

        if(tempprevvalue  > 0){
            prevstartvalue = tempprevvalue;
        }
    } else {
        if(noOfRows > cbGlobal.MAX_EXTRIES){
            nextstartvalue = cbGlobal["MAX_EXTRIES"] + 1;
        }
    }
    cbNavig[elm].FIRSTVAL = start;
    cbNavig[elm].PREVVAL = prevstartvalue;
    cbNavig[elm].NEXTVAL = nextstartvalue;
    cbNavig[elm].LASTVAL = end;

    navigArray['first'] = start;
    navigArray['last1'] = endval;
    navigArray['previous'] = prevstartvalue;
    navigArray['next'] = nextstartvalue;
    navigArray['last'] = end;
    return JSON.stringify(navigArray);
}

constructComments = function(start, end, elm)
{
    var cbCommNode = document.getElementById("cbCommentsContainer-"+elm);
    while (cbCommNode.firstChild) {
        cbCommNode.removeChild(cbCommNode.firstChild);
    }

    var rType = cbDet.RatingTypeId.substring(0,1);
    for(i = start-1; i < end; i++)
    {
        var cmtId = commentsArr[i].CB_COMMENT_ID;
        var jsObj = {};
        jsObj.CB_COMMENT_ID = cmtId;
        jsObj.CB_COMMENTS_ADDED_BY = commentsArr[i].CB_COMMENTS_ADDED_BY;
        jsObj.CB_COMMENTS_ADDED_TIME = commentsArr[i].CB_COMMENTS_ADDED_TIME;
        jsObj.CB_COMMENTS_CONTENT = commentsArr[i].CB_COMMENTS_CONTENT;
        jsObj.CB_CMT_RATING = commentsArr[i].CB_CMT_RATING;

        cbCBCommentHTML(JSON.stringify(jsObj), false, elm);
    }
}
/**
 * Code for commentbox - end
 */

fnSetBannerImg = function(src,banWid, banHei){
    if(typeof(window.parent.responsivePreview)!="undefined" && window.parent.responsivePreview=="true"){
        return;
    }else if(typeof(responsiveTheme)!="undefined" && responsiveTheme==true){//NO I18N
        if(banWid>window.innerWidth || !mobile){
                return;
        }
    }
    var imgSrc = src.src;
    if(imgSrc.indexOf("scaled") != -1)return;
    var width = (src.parentNode.tagName.toLowerCase() === "a"?src.parentNode.parentNode:src.parentNode).clientWidth; //NO I18N
    var height = (src.parentNode.tagName.toLowerCase() === "a"?src.parentNode.parentNode:src.parentNode).clientHeight; //NO I18N
    var img = new Image();
    img.onload = function(){
        var wid = this.width;
        var hei = this.height;
        if(wid > width){
            wid = width;
            hei = Math.floor(this.height*width/this.width);
            if(hei < height){
                hei = height;
                wid = Math.floor(this.width*height/this.height);
            }
        }else{
            wid = width;
            hei = Math.floor(this.height*width/this.width);
            if(hei < height){
                hei = height;
                wid = Math.floor(this.width*height/this.height);
            }
        }
        var cssText = src.style.cssText;
        if(banWid){
            if(width != banWid || height != banHei){
                var diffWid = width-banWid;
                if(diffWid<0){
                    diffWid=0;
                }
                var widApply = src.clientWidth+(diffWid);
                //src.style.width=widApply+"px";
                src.style.height='auto';
                if(src.offsetTop < -(src.clientHeight-height)){
                    //src.style.top = -(src.clientHeight-height)+'px'
                }
            }
        }
        if (cssText.indexOf( 'width' ) === -1 && cssText.indexOf( 'height' ) === -1) {
            src.style.cssText = "position:relative;top:"+Math.floor((height-hei)/2)+"px;left:"+Math.floor((width-wid)/2)+"px;width:"+wid+"px;height:"+hei+"px";//NO I18N 
        }


    }
    img.src = imgSrc;
}

fnOverlayClick = function(evt){
   var src = evt.target?evt.target:evt.srcElement;
   if(src.id == "overlay"){
        if(src.previousSibling.tagName.toLowerCase() == "a" && !window.ZS_PreviewMode){
            window.open(src.previousSibling.href,src.previousSibling.target);
        }
        else if(src.previousSibling.tagName.toLowerCase() == "a" && window.ZS_PreviewMode){
                fnPreviewClickInfoMsg(evt);
        }
   }
}

lightBox = function(src,caption){
    document.body.style.overflow = "hidden";
    var mask = document.createElement("div");
    mask.style.cssText = "position:fixed;top:0px;left:0;width:100%;height:100%;background:rgba(0,0,0,.85);z-index:401";//NO I18N
    var imgCont = document.createElement("div");
    var closeBtn = document.createElement("div");
    closeBtn.className="slideShowCloseCont"
    closeBtn.innerHTML="<div style='float: left;' class='slideShowCloseImg'></div><span style='float: left; padding-left: 5px;'>Close</span>";
    var pinitBtn = document.createElement("div");
    var imgSrc = src.src;
    var caption = "";
    if(src.nextSibling && src.nextSibling.nextSibling){
        caption =  src.nextSibling.nextSibling.innerHTML;
    }
    pinitBtn.style.cssText = "position:absolute;cursor:pointer;background:url(/zimages/zspinit.png);width:55px;height:25px;";// No I18N
    pinitBtn.onclick = function(){ ZP_Pinterest_Load(imgSrc,caption);}
    //closeBtn.style.cssText = "position:absolute;right:-13px;top:0px;cursor:pointer;background: url(../zimages/slideshow.png) 0 -80px no-repeat;width: 14px;height: 14px;";//NO I18N
    var func = function(){window.onresize=null;window.onkeypress=null;document.body.style.overflow = "auto";document.body.removeChild(mask);};
    window.onkeypress = function(e){
        if((e.which?e.which:e.keyCode) == 27){
           func();
        }
    }
    closeBtn.onclick=function(){func()};
    var winHeight = window.innerHeight || document.documentElement.clientHeight;
    var winWidth = window.innerWidth || document.documentElement.clientWidth;
    imgCont.style.cssText = "position:absolute;top:"+((winHeight-40)/2)+"px;left:"+((winWidth-40)/2)+"px;padding:10px;background:#ffffff;width:40px;height:40px;transition:all .5s ease-out;-webkit-transition:all .5s ease-out;-moz-transition:all .5s ease-out;-o-transition:all .5s ease-out";//NO I18N
    mask.appendChild(pinitBtn);
    mask.appendChild(imgCont);
    mask.appendChild(closeBtn);
    document.body.appendChild(mask);
    var transSupport = function(){
        var transitions = {
            'transition':'transitionend',// No I18N
            'OTransition':'oTransitionEnd',// No I18N
            'MSTransition':'msTransitionEnd',// No I18N
            'MozTransition':'transitionend',// No I18N
            'WebkitTransition':'webkitTransitionEnd'// No I18N
        }
        var style = document.body.style || document.documentElement.style;
        for(transition in transitions){
            if(style[transition] != undefined){
                return {transitionEnd:transitions[transition],'transition':transition};
            }
        }
        return false;
    }
    var height,width,actWidth,actHeight;
    var img = new Image();
    img.onload = function(){
        actHeight = height = this.height;
        actWidth = width = this.width;
        img.style.cssText = "width:auto;height:"+height+"px;opacity:0;transition:opacity .2s;-webkit-transition:opacity .2s;-moz-transition:opacity .2s;-o-transition:opacity .2s;";//NO I18N        
        imgCont.appendChild(this);
        if( winHeight < winWidth){
            if(this.height > winHeight-60){
                this.removeAttribute('width');
                this.removeAttribute('height');
                height = winHeight-60;
                this.setAttribute("height", winHeight-60);
                img.style.cssText = "width:auto;height:"+height+"px;opacity:0;transition:opacity .2s;-webkit-transition:opacity .2s;-moz-transition:opacity .2s;-o-transition:opacity .2s;";//NO I18N    
            }
            if(this.clientWidth>winWidth-60){
                this.removeAttribute('height');
                this.setAttribute("width", winWidth-60 ); 
                width = winWidth-60;                   
                img.style.cssText = "height:auto;width:"+width+"px;opacity:0;transition:opacity .2s;-webkit-transition:opacity .2s;-moz-transition:opacity .2s;-o-transition:opacity .2s;";//NO I18N                    
            }
        }else{
            if(this.width >winWidth-60){
                this.removeAttribute('width');
                this.removeAttribute('height');
                width = winWidth-60;
                this.setAttribute("width", winWidth-60 );
                img.style.cssText = "height:auto;width:"+width+"px;opacity:0;transition:opacity .2s;-webkit-transition:opacity .2s;-moz-transition:opacity .2s;-o-transition:opacity .2s;";//NO I18N    
            }
            if(this.clientHeight>winHeight-60){
                this.removeAttribute('width');
                this.setAttribute("height", winHeight-60 );                  
                height = winHeight-60;
                img.style.cssText = "width:auto;height:"+height+"px;opacity:0;transition:opacity .2s;-webkit-transition:opacity .2s;-moz-transition:opacity .2s;-o-transition:opacity .2s;";//NO I18N    
            }
        }
        imgCont.style.height = this.clientHeight+"px";
        imgCont.style.width = this.clientWidth+"px";
        imgCont.style.top = (winHeight-(this.clientHeight+20))/2+"px";
        imgCont.style.left = (winWidth-(this.clientWidth+20))/2+"px";
        var trans = transSupport();
        if(trans){
            imgCont.addEventListener(trans["transitionEnd"],function(){this.style[trans["transition"]]="";img.style.opacity=1},false);
        }else{
            img.style.opacity=1;
        }
    }
    img.src = src.src;
    window.onresize = function(){
        var winHeight =  document.documentElement.clientHeight;
        var winWidth =  document.documentElement.clientWidth;
        if( winHeight < winWidth){
            if(img.clientHeight > (winHeight-60) || img.clientHeight <actHeight){
                img.style.height = (winHeight-60) + "px";
                img.style.width="auto";
            }
            if(img.clientWidth > winWidth-60 ){
                img.style.width = (winWidth-60) +"px";
                img.style.height = "auto";
            }
        }else{
            if( img.clientWidth > (winWidth-60) || img.clientWidth <  actWidth){
                img.style.width = (winWidth-60) + "px";
                img.style.height="auto";
            }
            if(img.clientHeight > winHeight-60){
                img.style.height = (winHeight-60) + "px";
                img.style.width="auto";
            }
        }
        if(img.clientWidth >actWidth || img.clientHeight >actHeight){
            img.style.height=actHeight+"px";
            img.style.width="auto";
        }
        imgCont.style.height = img.clientHeight+"px";
        imgCont.style.width = img.clientWidth+"px";
        imgCont.style.top = (winHeight-(img.clientHeight+20))/2+"px";
        imgCont.style.left = (winWidth-(img.clientWidth+20))/2+"px";
    }
}



function ZP_Pinterest_Load (src,des){
    if(window.ZS_PublishMode && !window.ZS_PreviewMode){

        var url = encodeURIComponent(document.URL);
        if(src.indexOf(("ht"+"tp"))==-1){
            src = "ht"+"tp://"+document.domain+src;// No I18N
        }
        var src = encodeURIComponent(src);
        pinterestURL ="ht"+"tp://www.pinterest.com/pin/create/button/?url="+url+"&media="+src;// No I18N
        if(des && des!=' '){
            pinterestURL+="&description="+encodeURIComponent(des);// No I18N
        }
        window.open(pinterestURL ,"","width=800,height=300");// No I18N
    }
    else{
         parent.Dialog.alert(parent.i18n('pages.builder.pinit.msg'),'information');
    }

}

fnGetDocumentElements_IEfix = function(tag, attr, value, parentElem){
    var elemList = new Array();
    var tagList = (parentElem) ? parentElem.getElementsByTagName(tag) : document.getElementsByTagName(tag);
    for(var i=0; i<tagList.length; i++){
        var tagValue = tagList[i].getAttribute(attr);
        if(tagValue !== null && tagValue === value){
            elemList.push(tagList[i]);
        }
    }
    return elemList;
}

window.onresize=function(){
    if(prevWidth!=window.innerWidth && (typeof(responsiveTheme)!="undefined" && responsiveTheme==true)){
        isResize=true;
        prevWidth=window.innerWidth;
        if(document.getElementById("nav-top")){
        revort()
        navActivate();
        prevWidth=window.innerWidth;
        var navShowing=document.getElementById(document.getElementById("nav-top").getAttribute('navshowing'));
        if(navShowing!=null){
            if(window.innerHeight<600){
                navMobileHideMenu(document.getElementById("nav-top"));
                var moreElem=document.getElementById("nav-submenu-idMore");
                if(moreElem!=null){
                    moreElem.style.width="";
    }
                if(navShowing.id=="nav-li1234"){
                    var subMenu=document.getElementById(navShowing.getAttribute("navsub"));
                    if(subMenu){
                        subMenu.style.width="";
                        subMenu.parentNode.style.width="";
                    }
                }
            }else{
                if(navShowing.id=="nav-li1234"){
                    var subMenu=document.getElementById(navShowing.getAttribute("navsub"));
                    if(subMenu){
                        subMenu.style.opacity="";
                    }
                }
                navHideMenu(document.getElementById("nav-top"));
            }
        }
        }
    resizeElements("resize");//NO I18N
}
}

resizeElements = function(eve){
    if((typeof(responsiveTheme)!="undefined" && responsiveTheme==true) ||(typeof(window.parent.responsivePreview)!="undefined" && window.parent.responsivePreview=="true") ){
        var banner=document.getElementById("zpBannerBg");

        if(banner ){
            var bannerparent = banner.parentNode;
            if(bannerparent.tagName.toLowerCase() === "a"){
                bannerparent = bannerparent.parentNode;
            }
            var prevParWidth=bannerparent.clientWidth;
            var prevParHeight=bannerparent.clientHeight;
            
            banner.style.height = "auto";
            bannerparent.style.width="100%";
            bannerparent.style.width=bannerparent.clientWidth+"px";
            bannerparent.style.height=Math.floor(bannerparent.clientWidth*(prevParHeight/prevParWidth))+"px";
            //bannerparent.style.height=bannerparent.clientHeight+"px";
            //banner.style.width="100%";
            //banner.style.height="auto";
            if(banner.style.width === ""){
                banner.style.width = banner.width+"px";
            }

        banner.style.width = ((banner.style.width).replace("px","")*(bannerparent.clientWidth/prevParWidth))+"px";
            banner.style.left = ((banner.style.left).replace("px","")*(bannerparent.clientWidth/prevParWidth))+"px";
            banner.style.top = ((banner.style.top).replace("px","")*(bannerparent.clientWidth/prevParWidth))+"px";
            //banner.style.width = bannerparent.clientWidth+"px"; 
            
            var overlay=document.getElementById("overlay");
            if(overlay){
                var overlayHeight=overlay.clientHeight;
                var overlayWidth=overlay.clientWidth;
                if(origOverlayWidth==0){
                    origOverlayWidth=overlayWidth;
                }
                if(origOverlayHeight==0){
                    origOverlayHeight=overlayHeight;
                }
                overlay.style.width=bannerparent.clientWidth+"px";
                overlay.style.height=bannerparent.clientHeight+"px";
                var elem=overlay.childNodes;
                for(i=0;i<elem.length;i++){
                    if(!(overlay.clientWidth>origOverlayWidth) && overlayWidth!==overlay.clientWidth){ 
                        elem[i].style.left=Math.floor((elem[i].offsetLeft/overlayWidth)*overlay.clientWidth)+"px";
                        elem[i].style.top=Math.floor((elem[i].offsetTop/overlayHeight)*overlay.clientHeight)+"px";
                        var temp;
                        if(elem[i].className.indexOf("bannerBtn")!==-1){
                            temp= elem[i].getElementsByTagName("button")[0];
                            var prevWidth=temp.clientWidth;
                            var prevHeight=temp.clientHeight;
                            /*temp.style.width=Math.floor(((temp.offsetWidth-padding*2)/overlayWidth)*overlay.clientWidth)+"px";
                            temp.style.height=Math.floor((prevHeight/prevWidth)*temp.clientWidth)+"px";
                            if(temp.clientWidth<=50){
                                temp.style.width="50px";
                                temp.style.height="15px";
                            }*/
                        }else{
                            var prevWidth=elem[i].clientWidth;
                            var prevHeight=elem[i].clientHeight;
                            elem[i].style.width=Math.floor((elem[i].offsetWidth/origOverlayWidth)*overlay.clientWidth)+"px";
                            elem[i].style.height=Math.floor((elem[i].offsetHeight/origOverlayHeight)*overlay.clientHeight)+"px";
                            elem[i].style.minHeight = "";
                            temp=elem[i];
                            var font=temp.getElementsByTagName("font");
                            if(font!==undefined){
                                //var fSize1=parseInt(font.getAttribute("size"));
                               for(var z=0; z < font.length ; z++){
                                    var fSize1 = parseInt(navGetStyle(font[z],"font-size").replace("px",""));//No I18N
                                    if(fSize1!=null){
                                        fSize1=Math.floor((fSize1/origOverlayWidth)*overlay.clientWidth)+"px";//NO I18N
                                        font[z].removeAttribute("size");
                                        font[z].style.fontSize = fSize1;
                                    }
                                }
                            }
                        }
                        var fontSize=parseInt(navGetStyle(temp,"font-size").replace("px",""));//NO I18N
                        temp.style.fontSize=Math.floor((fontSize/origOverlayWidth)*overlay.clientWidth)+"px";//NO I18N
                        if(parseInt(navGetStyle(temp,"font-size").replace("px",""))<8){
                            temp.style.fontSize="8px";//NO I18N
                            //if(elem[i].className.indexOf("bannerBtn")==-1){
                            //}
                        }
                        if(navGetStyle(temp, 'font-size').replace('px','') < 8 || temp.scrollHeight > temp.offsetHeight || temp.offsetHeight + temp.offsetTop > overlay.offsetHeight){
                            var ratio=(temp.clientHeight/temp.clientWidth);
                            temp.style.minWidth=temp.style.width;
                            temp.style.width = '';//No I18N
                            var myMax = ratio*temp.clientWidth;
                            temp.style.maxHeight = (myMax+temp.offsetTop >= overlay.offsetHeight ? (overlay.offsetHeight-temp.offsetTop-2) : myMax )+'px';//No I18N
                            temp.style.height="";//No I18N
                        }
                        var fontElement;
                        for(j=1;j<7;j++){
                            fontElement=temp.getElementsByTagName("h"+j)[0];
                            if(fontElement!=null){
                                break;
                            }
                        }
                        if(fontElement!=null){
                            var fontSize=parseInt(navGetStyle(temp, "font-size").replace('px',''));//Math.floor((parseInt(navGetStyle(fontElement,"font-size").replace("px",""))/origOverlayWidth)*overlay.clientWidth);//NO I18N
                            if(fontSize<8){
                                fontSize=8;
                            }
                            var fDiv="<div style=\"font-size:"+fontSize*1.2+"px\">"+fontElement.innerHTML+"</div>";
                            fontElement.innerHTML=fDiv;
                        }
                        var padding=parseInt(navGetStyle(temp,"padding-left").replace("px",""));//NO I18N
                        temp.style.paddingLeft=temp.style.paddingRight=Math.floor((padding/origOverlayWidth)*overlay.clientWidth)+"px";//NO I18N
                        var padding1=parseInt(navGetStyle(temp,"padding-top").replace("px",""));//NO I18N
                        temp.style.paddingTop=temp.style.paddingBottom=Math.floor((padding1/origOverlayWidth)*overlay.clientWidth)+"px";//NO I18N
                        var border=parseInt(navGetStyle(temp,"border").replace("px",""));//NO I18N
                        temp.style.border=Math.floor((border/origOverlayWidth)*overlay.clientWidth)+"px solid";
                        if(border && temp.style.border=="0px solid"){
                            temp.style.border="1px solid";
                        }
                        while((temp.scrollHeight > temp.offsetHeight) && (temp.scrollHeight - temp.offsetHeight > 2) && (temp.offsetLeft-5 > 0)){ //code to resize the overlay bacckground while overflow // the differece should be greater than 5
                            temp.style.left = temp.offsetLeft - 5 + 'px';//No I18N
                        }
                        if((temp.scrollHeight > temp.offsetHeight) && (temp.scrollHeight - temp.offsetHeight > 2)){
                            while((temp.scrollHeight > temp.offsetHeight) && (temp.scrollHeight - temp.offsetHeight > 2)){
                                var mySize = parseInt(navGetStyle(temp, "font-size").replace("px",""));//No I18N
                                if(mySize < 3){
                                    break;
                                }
                                temp.style.fontSize = (mySize-3 )+'px';//No I18N
                            }
                        }
                    }
                }
            }
            //if(eve!=null){
                //fnSetBannerImg(banner,750,500);
                fnSetBannerImg(banner,banner.clientWidth,banner.clientHeight);
            //}
        }
        banner=document.getElementById("carouselImg");
        if(banner){
            if(origOverlayWidth==0){
                origOverlayWidth=banner.clientWidth;
            }
            if(origOverlayHeight==0){
                origOverlayHeight=banner.clientHeight;
            }
            if(tempOrigOverlayWidth==0){
                tempOrigOverlayWidth=banner.clientWidth;
            }
            if(tempOrigOverlayHeight==0){
                tempOrigOverlayHeight=banner.clientHeight;
            }
            var overlay=document.getElementById("overTxt");
            
            /*    tempOverlayHeight=banner.clientHeight;
               tempOverlayWidth=banner.clientWidth;
            var height=(banner.clientHeight/banner.clientWidth);
            banner.parentNode.style.width=banner.parentNode.parentNode.style.width;
            var padWidth = parseInt(navGetStyle(banner.parentNode,'padding-left').replace("px",""));;//No I18N
            banner.style.width=(banner.parentNode.clientWidth-padWidth*2)+"px";
            banner.style.height=Math.floor((banner.parentNode.clientWidth-padWidth*2)*height)+"px";
            banner.parentNode.style.height=banner.clientHeight+"px";
            banner.parentNode.style.width=banner.clientWidth+"px";*/
        }
    banner =document.getElementsByClassName("themeBannerArea")[0];
    if(!banner){
        banner =document.getElementsByClassName("themeBanner")[0];
    }
    if(banner){
        var swfBanner=banner.getElementsByTagName("object")[0];
            if(swfBanner){
                    swfBanner.parentNode.parentNode.style.width=banner.clientWidth+"px";
                    var prevHeight=swfBanner.getAttribute("height");
                    var prevWidth=swfBanner.getAttribute("width");
                    var padWidth = parseInt(navGetStyle(banner,'padding-left').replace("px",""));;//No I18N
                    swfBanner.setAttribute("width",banner.clientWidth-padWidth*2);
            }
    }
        var logo=document.getElementById("zpLogo");
        if(logo && logo.clientWidth>window.innerWidth){
            logo.style.width=window.innerWidth+"px";;
            /*logo.style.width="100%";
            logo.parentNode.style.width="100%";*/
        }
        var imgs=document.getElementsByClassName("zpImage");//NO I18N
        for(i=0;i<imgs.length;i++){
            if(imgs[i].clientWidth>window.innerWidth){
                imgs[i].style.width=window.innerWidth+"px";
            }
        }

        /*
        var gImgs=document.getElementsByClassName("photoContent");//NO I18N
        if(gImgs && window.innerWidth<350){
            for(i=0;i<gImgs.length;i++){
                //gImgs[i].style.width=Math.floor(window.innerWidth/2)+"px";
                //gImgs[i].children[0].style.width=gImgs[i].style.width;
            }
        }*/

        var photoList=document.getElementsByClassName("photoListView");//NO I18N
        var ownG=document.getElementsByClassName("ownGallery");//NO I18N
        if(ownG && photoList.length!=0){
            for(i=0;i<ownG.length;i++){
                var zpAlignGal=ownG[i].children[1];
                var cheight=photoList[0].clientHeight;
                zpAlignGal.style.height=cheight+"px";
                //gImgs[i].style.width=Math.floor(window.innerWidth/2)+"px";
                //gImgs[i].children[0].style.width=gImgs[i].style.width;
            }
        }

        var slideShow=document.getElementsByClassName("carousel");//NO I18N
        for(i=0;i<slideShow.length;i++){
            var zpAlign=slideShow[0].children[0].children[0];
            if(zpAlign){
                var prevHeight=zpAlign.clientHeight;
                var prevWidth=zpAlign.clientWidth;
                zpAlign.style.width=zpAlign.parentNode.clientWidth+"px";
                //zpAlign.style.height=zpAlign.parentNode.clientWidth+"px";
                var calculatedHeight=(prevHeight/prevWidth)*zpAlign.clientWidth;
                zpAlign.style.height=Math.round(calculatedHeight)+"px";
            }
        }
        if(mobile){
            var searchContainer=document.getElementsByClassName("themeSearchContainer");//NO I18N
            if(searchContainer[0]){
                //searchContainer[0].parentNode.removeChild(searchContainer[0]);
                if(!istab || window.innerWidth<640){
                searchContainer[0].style.display="none";
                }else{
                    searchContainer[0].style.display="";
                }
            }
        }
        if(mobile){
            var imageCaption=document.getElementsByClassName("zpImageCaption");//NO I18N
            for(i=0;i<imageCaption.length;i++){
                imageCaption[i].style.width="auto";
            }
        }
    }
}

resizeOverlay=function(){
            var banner=document.getElementById("carouselImg");
            var overlay=document.getElementById("overTxt");
            if(banner.parentNode.style && banner.parentNode.style.height && banner.parentNode.style.width){
                banner.style.height = banner.parentNode.style.height;
                banner.style.width = banner.parentNode.style.width;
            }
            if(overlay){
                var overlayHeight=overlay.clientHeight;
                var overlayWidth=overlay.clientWidth;
                if(tempOrigOverlayWidth==0){
                    tempOrigOverlayWidth=overlayWidth;
                }
                overlay.style.width=banner.clientWidth+"px";
                overlay.style.height=banner.clientHeight+"px";
                var elem=overlay.childNodes;
                for(i=0;i<elem.length;i++){
                    if(overlay.clientWidth<tempOrigOverlayWidth){
                        elem[i].style.left=Math.floor((elem[i].offsetLeft/tempOrigOverlayWidth)*overlay.clientWidth)+"px";
                        elem[i].style.top=Math.floor((elem[i].offsetTop/tempOrigOverlayHeight)*overlay.clientHeight)+"px";
                        var temp;
                        if(elem[i].className.indexOf("bannerBtn")!==-1){
                            temp= elem[i].getElementsByTagName("button")[0];
                            var prevWidth=temp.clientWidth;
                            var prevHeight=temp.clientHeight;
                            /*temp.style.width=Math.floor((temp.offsetWidth/tempOrigOverlayWidth)*overlay.clientWidth)+"px";
                            temp.style.height=Math.floor((prevHeight/prevWidth)*temp.clientWidth)+"px";
                            if(temp.clientWidth<=50){
                                temp.style.width="50px";
                                temp.style.height=Math.floor((prevHeight/prevWidth)*50)+"px";
                            }*/
                        }else{
                            var prevWidth=elem[i].clientWidth;
                            var prevHeight=elem[i].clientHeight;
                            elem[i].style.width=Math.floor((elem[i].offsetWidth/tempOrigOverlayWidth)*overlay.clientWidth)+"px";
                            elem[i].style.height=Math.floor((elem[i].offsetHeight/tempOrigOverlayHeight)*overlay.clientHeight)+"px";
                            elem[i].style.minHeight = "";//No I18N
                            temp=elem[i];
                            var font=temp.getElementsByTagName("font");
                            if(font!==undefined){
                                //var fSize1=font.getAttribute("size");
                                //var sizeArray = [11,13,16,19,24,32,42];
                                // var fSize1 = sizeArray[fSize1-1];
                                for(var z=0; z < font.length ; z++){
                                    var fSize1 = parseInt(navGetStyle(font[z],"font-size").replace("px",""));//No I18N
                                    if(fSize1!=null){
                                        fSize1=Math.floor((fSize1/tempOrigOverlayWidth)*overlay.clientWidth)+"px";//NO I18N
                                        font[z].removeAttribute("size");
                                        font[z].style.fontSize = fSize1;
                                    }
                                }
                            }
                        }
                        var fontSize=parseInt(navGetStyle(temp,"font-size").replace("px",""));//NO I18N
                        temp.style.fontSize=Math.floor((fontSize/tempOrigOverlayWidth)*overlay.clientWidth)+"px";//NO I18N
                        if(parseInt(navGetStyle(temp,"font-size").replace("px",""))<8){
                            temp.style.fontSize="8px";//NO I18N
                            //if(elem[i].className.indexOf("bannerBtn")==-1){
                            //}
                        }
                        if(navGetStyle(temp, 'font-size').replace('px','') < 8 || temp.scrollHeight > temp.offsetHeight || temp.offsetHeight + temp.offsetTop > overlay.offsetHeight){
                            var ratio=(temp.clientHeight/temp.clientWidth);
                            temp.style.minWidth=temp.style.width;
                            temp.style.width = '';//No I18N
                            var myMax = ratio*temp.clientWidth;
                            temp.style.maxHeight = (myMax+temp.offsetTop >= overlay.offsetHeight ? (overlay.offsetHeight-temp.offsetTop-2) : myMax )+'px';//No I18N
                        }
                        var fontElement;
                        for(j=1;j<7;j++){
                            fontElement=temp.getElementsByTagName("h"+j)[0];
                            if(fontElement!=null){
                                break;
                            }
                        }
                        if(fontElement!=null){
                            var fontSize=parseInt(navGetStyle(temp, "font-size").replace('px',''));//Math.floor((parseInt(navGetStyle(fontElement,"font-size").replace("px",""))/tempOrigOverlayWidth)*overlay.clientWidth);//NO I18N
                            if(fontSize<8){
                                fontSize=8;
                            }
                            var fDiv="<div style=\"font-size:"+fontSize*1.2+"px\">"+fontElement.innerHTML+"</div>";
                            fontElement.innerHTML=fDiv;
                        }
 
                        var padding=parseInt(navGetStyle(temp,"padding-left").replace("px",""));//NO I18N
                        temp.style.paddingLeft=temp.style.paddingRight=Math.floor((padding/tempOrigOverlayWidth)*overlay.clientWidth)+"px";//NO I18N
                        var padding1=parseInt(navGetStyle(temp,"padding-top").replace("px",""));//NO I18N
                        temp.style.paddingTop=temp.style.paddingBottom=Math.floor((padding1/tempOrigOverlayWidth)*overlay.clientWidth)+"px";//NO I18N
 
                        var border=parseInt(navGetStyle(temp,"border").replace("px",""));//NO I18N
                        temp.style.border=Math.floor((border/tempOrigOverlayWidth)*overlay.clientWidth)+"px solid";
                        if(border && temp.style.border=="0px solid"){
                            temp.style.border="1px solid";
                        }
                        while((temp.scrollHeight > temp.offsetHeight) && (temp.scrollHeight - temp.offsetHeight > 2) && (temp.offsetLeft-5 > 0)){ //code to resize the overlay bacckground while overflow // the differece should be greater than 5
                            temp.style.left = temp.offsetLeft - 5 + 'px';//No I18N
                        }
                        if((temp.scrollHeight > temp.offsetHeight) && (temp.scrollHeight - temp.offsetHeight > 2)){
                            while((temp.scrollHeight > temp.offsetHeight) && (temp.scrollHeight - temp.offsetHeight > 2)){
                                var mySize = parseInt(navGetStyle(temp, "font-size").replace("px",""));//No I18N
                                if(mySize < 3){
                                    break;
                                }
                                temp.style.fontSize = (mySize-2 )+'px';//No I18N
                            }
                        }
                        temp.style.height="";//No I18N
                    }
                }                
            }
}

fnChangeTab = function(evt){
    var elem = evt.target?evt.target:evt.srcElement;
    if(elem.id=="" && elem.className==""){
        elem=elem.parentNode;
    }
    if(elem.className.indexOf("selected")!==-1){
        return;
    }
    var prevSelected;
    if(elem.className.indexOf("zs-accordion")!=-1){
        prevSelected = getClasses('selected',"div",elem.parentNode)[0];//NO I18N
    }else{
        prevSelected = getClasses('selected',"li",elem.parentNode)[0];//NO I18N
    }
    prevSelected.className=prevSelected.className.replace("selected","");
    var accordionSelected;
    var contents;
    if(prevSelected.className.indexOf("zs-tabs-accordion-header")!=-1){
        contents=getClasses("zs-tabs-accordion-content","span",prevSelected.parentNode.parentNode.parentNode);//NO I18N
    }else{
        contents=getClasses("zs-tabs-accordion-content","span",prevSelected.parentNode);//NO I18N
    }
    var name="content"+prevSelected.getAttribute("name").replace("tab","");//NO I18N
    var newName="content"+elem.getAttribute("name").replace("tab","");//NO I18N
    for(var i=0,length=contents.length;i<length;i++){
        if(contents[i].getAttribute("name")===name){
            contents[i].style.display="none";
        }
        if(contents[i].getAttribute("name")===newName){
            contents[i].style.display="block";
            accordionSelected=contents[i];
        }
    }
    elem.className=elem.className+" selected";
    loadAudioFiles();
    var childElements=accordionSelected.getElementsByClassName("picasa");//NO I18N
    for(var i=0;i<childElements.length;i++){
            if(childElements[i].clientHeight<75){
            if(childElements[i].clientHeight!=0){
                var atitle=childElements[i].getElementsByClassName("zpalbumTitle-container");
                for(j=0;j<atitle.length;j++){
                    atitle[j].parentNode.removeChild(atitle[j]);
                }
                var agallery=document.getElementById('zpg'+childElements[i].id);
            }
            albumCount.push(childElements[i]);  
            galleryElements.push(document.getElementById('zpg'+childElements[i].id));
            }
    }    
    childElements=accordionSelected.getElementsByClassName("flickr");//NO I18N
    for(var i=0;i<childElements.length;i++){
            if(childElements[i].clientHeight<75){
            albumCount.push(childElements[i]);  
            galleryElements.push(document.getElementById('zpg'+childElements[i].id));
            }
    }    
    childElements=accordionSelected.getElementsByClassName("ownGallery");//NO I18N
    for(var i=0;i<childElements.length;i++){
            albumCount.push(childElements[i]);  
            galleryElements.push(document.getElementById('zpg'+childElements[i].id));
    }    
    if(typeof(Gallery.fnShowOwnGalleryImgs)=="undefined"){
        if(albumCount.length>0){
            if(window.ZS_PreviewMode){
                commonLoadScript(assetsUrl+"/js/gallery.js");//NO I18N
                commonLoadScript(assetsUrl+"/js/collage.js");//NO I18N
                loadingAlbumCount = 1;
            }else{
                xml("GET","/gallery.txt",function(res){//NO I18N
                    window.ownGallery = JSON.parse(res);
                    commonLoadScript("/js/gallery.js");//NO I18N
                    commonLoadScript("/js/collage.js");//NO I18N
                    loadingAlbumCount = 1;
                    },"");
            }
        }
    }else{
    if(albumCount.length>0){
        var album = albumCount.pop();
        if(album.className == "zpelement-wrapper picasa"){
            setTimeout(function(){commonLoadScript("//picasaweb.google.com/data/feed/api/user/"+album.getAttribute('albumusername')+"/albumid/"+album.getAttribute('albumid')+"?alt=json-in-script&callback=Gallery.showAlbumPhotos");},10);
        }
        else if(album.className == "zpelement-wrapper flickr"){
            setTimeout(function(){commonLoadScript("ht"+"tps://www.flickr.com/services/rest?method=flickr.photosets.getPhotos&api_key=317b7c22e9394b291b75f6356335d254&photoset_id="+album.getAttribute('albumid')+"&format=json&jsoncallback=Gallery.showAlbumPhotos");},10);// No I18N

        }else{
        if(album.clientHeight<75){
        var func = function(){
          Gallery.fnShowOwnGalleryImgs(album.getAttribute("albumid"),album.getAttribute("albumusername"));
        }
         setTimeout(func,1000);
         }
        }
    }
    }
    var fbElem=accordionSelected.getElementsByClassName("facebook");//NO I18N
    for(var i=0;i<fbElem.length;i++){
        var fbComment=getClasses(".fb-comments","div",fbElem[i]);// No I18N
        if(!fbComment[0]){
            fbComment=getClasses(".facebook","div",fbElem[i]);// No I18N
        }
        if(fbComment[0]){
            fbComment[0].setAttribute("data-width",(window.ZS_PublishMode||window.ZS_PreviewMode)?fbElem[i].offsetWidth:fbElem[i].offsetWidth-2);
        }
        if(fbElem[i].clientHeight==0){
            facebookElem.push(fbElem[i]);
        }
    }
    for(var i=0;i<fbElem.length;i++){
       var wType = fbElem[i].getAttribute("data-fwType");
       facebookElem.push(fbElem[i]);
    }
    if(fbElem.length>0){
            if(window.FB){
                enableFacebookWidget();
                window.FB.XFBML.parse();
            }
    }   
    //for carousel
    var carousel=accordionSelected.getElementsByClassName("carousel");//NO I18N
        if(carousel){
            var func2 = function(res){
                var func3 = function(){
                    if(typeof(ImageRotator) != "undefined"){
                        clearInterval(window.interval1);
                        res = JSON.parse(res);
                        func(res);
                    }
                }
                window.interval1 = setInterval(func3,16);
            }
            var func = function(res){
                for(var c=0;c<carousel.length;c++){
                    var elem=carousel[c];
                    var carouselProp = res[elem.getAttribute("carouselId")];
                    if(carouselProp){
                    var imgs = carouselProp.slideURL;
                    var imgArr=[];
                    for(var i=0;i<imgs.length;i++){
                        if(window.ZS_PreviewMode){
                            imgArr.push(imgs[i]);
                        }else{
                            var imgSrc = imgs[i].split("/");
                            imgArr.push(imgs[i].replace(imgSrc[2]+"/",""));
                        }
                    }
                    var carCont;
                    carCont=getClasses("zpAlignPos","div",elem)[0];// No I18N
                    var cLength=carCont.childNodes.length;
                    if(cLength>0){
                    while(carCont.firstChild){
                        carCont.removeChild(carCont.firstChild);
                     }
                    }
                    carCont = document.createElement("div");
                    carCont.innerHTML="<div style=\"position: absolute; top: 0px; left: 0px; opacity: 1; z-index: 2; overflow:hidden;background:#fff\"></div><div style=\"position: absolute; top: 0px; left: 0px; display: block; z-index: 1; overflow:hidden;\">";//No I18N
                    elem.children[0].appendChild(carCont);
                    var width = carCont.parentNode.clientWidth;
                    if(carouselProp.settings.thumbnail == "on" && (carouselProp.settings.thumbPos == "left" || carouselProp.settings.thumbPos == "right")){
                        width = width-65;
                    }
                    var height=Math.floor((width*9)/16);
                    carCont.style.cssText = "position:relative;width:"+width+"px;height:"+height+"px;overflow:hidden;float:left";//No I18N
                    carCont.id="ir"+Math.floor(Math.random()*1000000000000);
                    if((typeof(responsiveTheme)!="undefined" && responsiveTheme==true && mobile===true && carouselProp.settings.type==="film" )||(typeof(window.parent.responsivePreview)!="undefined" && window.parent.responsivePreview=="true" && carouselProp.settings.type==="film")){
                        carouselProp.settings.type="stripe";
                    }
                     var rotator=new ImageRotator(carCont,imgArr,carouselProp.settings);}
                }
            }
            if(window.ZS_PreviewMode){
                    func2(JSON.stringify(carouselProp));
            }else{
                xml("GET","/carousel.txt",func2,""); //No I18N
            }
        }

}

findParent=function(el2,cls) {
        var el2 = el2.parentNode;
        while(el2) {if(el2.className && el2.className.indexOf(cls)!=-1){ return el2;}el2 = el2.parentNode;}
        return false;
    }

function submitVote(param) {
    var actualPath = location.pathname;
    var path = actualPath.substr(1,actualPath.lastIndexOf(".")-1);
    path = path.replace("mobile/","");
    if(window.location.host.indexOf("sitepreview") !== -1 || window.location.host.indexOf("siteipreview") !== -1  || path.indexOf("preview/") !== -1) {
        alert("Rating will work only on published website");
        return false;
    }
    
    var inp = param.id;
    inpElm = document.getElementById(inp);
    inpElm.checked=true;
    
    var eltCont = findParent(param, "zpelement-wrapper commentbox");//NO I18N
    var elmId = eltCont.id;
    
    var resDiv = document.getElementById(elmId+"_resultsDiv");
    if(resDiv) {
    resDiv.style.display="none";
    }
    
    var voteVal = inp.replace("rating-","");
    var rCont = document.getElementById("cbRatingContainer-"+elmId);
    rCont.setAttribute("curRating",voteVal);
    
    var rFormId= param.parentNode.parentNode;
    rFormId.setAttribute("voted","true");
    
    var actualPath = location.pathname;

    var path = actualPath.substr(1,actualPath.lastIndexOf(".")-1);
    path = path.replace("mobile/","");
    if(path.indexOf("siteapps/protected/") != -1) {
        path = path.replace("siteapps/protected/","");
    }
    var randVal = getBrowserCookie();
    
    var paramsdata = "rateVal="+voteVal+"&path="+path+"&randVal="+randVal;  //No I18N
    
    if(cbDet.CBVisible == "0" && cbDet.RatingVisible == "1") {
        var ie = /*@cc_on!@*/false;
        if(!ie){
            xml("POST","/siteapps/commentbox/addRankVotes",addRankVotesCallback,paramsdata);//No I18N
        }else{
            xml("POST","/siteapps/commentbox/addRankVotes",addRankVotesCallback,paramsdata);//No I18N
        }
    }
}

function addRankVotesCallback(resp) {
    var cbResults = JSON.parse(resp);
    var currRData = cbResults.RESULT.currUsrRatingData;
    var totResData = cbResults.RESULT.usrTotVoteResults;
    var rType = cbDet.RatingTypeId.substring(0,1);
    
    if(document.querySelectorAll){
        var matches = document.querySelectorAll("div.rating-outer-container");  //NO I18N
        for(p=0;p<matches.length;p++){
            if(matches[p].hasAttribute("voted")) {
                var rateCont = matches[p];
                rateCont.removeAttribute("voted");
                if(rType == "1") {
                    var starRatingDiv = rateCont.querySelectorAll("div.star-rating");   //NO I18N
                    starRatingDiv[0].removeAttribute("style");

                    for(var i = 0; i < commentBoxElm.length; i++) {
                        cbElm = commentBoxElm[i];
                        elmId = cbElm.id;
                        
                        if(cbResults.CB_RATING_RESULT) {
                            var resDiv = document.getElementById(elmId+"_resultsDiv");
                            if(resDiv) {
                                resDiv.innerHTML=cbResults.CB_RATING_RESULT;
                            }
                        }
                    }
                    
                    var rateAvgDiv = starRatingDiv[0].parentNode.nextSibling.nextSibling;
                    var vStr = (totResData.votes > 1) ? "Votes" : "Vote";    //NO I18N
                    rateAvgDiv.innerHTML = totResData.average + "/ 5 ("+totResData.votes+" "+vStr+")"; 
                }
                else if(rType == "2") {
                    var thumbUpResDiv = rateCont.querySelectorAll("div.thumb-up-result");   //NO I18N
                    thumbUpResDiv[0].innerHTML=totResData.rateScale1;
                
                    var thumbDownResDiv = rateCont.querySelectorAll("div.thumb-down-result");   //NO I18N
                    thumbDownResDiv[0].innerHTML=totResData.rateScale2;
                }
            }
        }
    }
}

var rateVal;
function generateCookie() {

    var get_as_float=5.5;
  
    var now = new Date().getTime() / 1000;
    var s = parseInt(now, 10);

    var ans = (get_as_float) ? now : (Math.round((now - s) * 1000) / 1000) + ' ' + s;
    
    var v = ans+""+Math.random().toString(36).slice(2);
    var f = v.replace(".","");
    return f;
}

function setBrowserCookie() {
    //if(detectCookie()) {
        var date = new Date();
        date.setTime(date.getTime()+(20*24*60*60*1000)); // cookie will be active for 20 days
        var expires = "; expires="+date.toGMTString();  //NO I18N
        var idVal = generateCookie();
        document.cookie = "rtId="+idVal+expires+"; path=/";
}

function checkCookie() {
    var cookie = document.cookie;
    var cVal = cookie.indexOf("rtId");
//    if(cVal == -1) {
        setBrowserCookie();
//    }
}

function getBrowserCookie() {
    var cs = document.cookie.split(";");
    var cookieVal;
    for(i=0;i<cs.length;i++) {
        var v = cs[i];
        if(v.indexOf("rtId") != -1) {
            val=v.split("=");
            cookieVal = val[1];
        }
    }
    return cookieVal;
}

render_newsletter = function(){
    var site_id = newsletter_elts[0].getAttribute('data_site_id');
    if(window.ZS_PreviewMode){
        xml("GET","/zs-site/api/v0/renderNewsletter?siteId="+site_id,render_newsletter_handler,"");//No I18N
    }else{
        xml("GET","/siteapps/renderNewsletter?siteId="+site_id,render_newsletter_handler,"");//No I18N
    }
}

render_newsletter_handler = function(response){
    for(var i=0; i < newsletter_elts.length; i++){
        var position_elt;
        if(!(newsletter_elts[i].getElementsByClassName)){
            position_elt = (getElementsByClassName_ieFix(newsletter_elts[i], "zpAlignPos"))[0];
        }else{
            position_elt = newsletter_elts[i].getElementsByClassName("zpAlignPos")[0]
        }
        if(response != -1){
            position_elt.innerHTML = response;
        }else{
            position_elt.innerHTML = "Error in loading newsletter";//NO I18N
        }
    }
}

validate_name = function(field){
    var name = field.value;
    var container_elt = findParent(field, "zpelement-wrapper newsletter");//NO I18N
    var error_msg_div = (!container_elt.getElementsByClassName) ? (getElementsByClassName_ieFix(container_elt, "newsletter_response"))[0] : container_elt.getElementsByClassName("newsletter_response")[0];
    var name_pattern = new RegExp("^[a-zA-Z\\.\\-\\s]+$");
    if(name.trim().length == 0){
        error_msg_div.innerHTML = "Please enter your name";//NO I18N
        error_msg_div.style.color = "red";
        error_msg_div.style.display = "block";
        return false;
    }else if(name.trim().length > 50){
        error_msg_div.innerHTML = "Length of the name must not exceed 50 charecters";//NO I18N
        error_msg_div.style.color = "red";
        error_msg_div.style.display = "block";
        return false;
    }/*else if(!name_pattern.exec(name)){
        error_msg_div.innerHTML = "Use only letters and these special charecters .- in name";//NO I18N
        error_msg_div.style.color = "red";
        error_msg_div.style.display = "block";
        return false;
    }*/
    return true;
}

validate_email = function(field){
    var email = field.value;
    var container_elt = findParent(field, "zpelement-wrapper newsletter");//NO I18N
    var error_msg_div = (!container_elt.getElementsByClassName) ? (getElementsByClassName_ieFix(container_elt, "newsletter_response"))[0] : container_elt.getElementsByClassName("newsletter_response")[0];
    if(email.trim().length == 0){
        error_msg_div.innerHTML = "Please enter your email id";//NO I18N
        error_msg_div.style.color = "red";
        error_msg_div.style.display = "block";
        return false;
    }
    var email_pattern= new RegExp("^[\\w]([\\w\\-\\.\\'\\/]*)@([\\w\\-\\.]*)(\\.[a-zA-Z]{2,22}(\\.[a-zA-Z]{2}){0,2})$");
    if(!email_pattern.exec(email)){
        error_msg_div.innerHTML = "Invalid email id";//NO I18N
        error_msg_div.style.color = "red";
        error_msg_div.style.display = "block";
        return false;
    }
    return true;
}

clear_field = function(){
    var error_msg_divs = (!document.getElementsByClassName) ? (getElementsByClassName_ieFix(document, "newsletter_response")) : document.getElementsByClassName("newsletter_response");//NO I18N
    for(var i = 0;i < error_msg_divs.length; i++){
        error_msg_divs[i].innerHTML = "";
        error_msg_divs[i].style.display = "none";
    }
}

subscribe_user = function(field){
    if(window.ZS_PreviewMode){
        alert(parent.i18n("pages.newsletter.preview.Error"));
    }else{
        var container_elt = findParent(field, "zpelement-wrapper newsletter");//NO I18N
        var email_field = (!container_elt.getElementsByClassName) ? (getElementsByClassName_ieFix(container_elt, "newsletter-input-field email_field"))[0] : container_elt.getElementsByClassName("newsletter-input-field email_field")[0];
        var is_valid = validate_email(email_field);
        var name_field = (!container_elt.getElementsByClassName) ? (getElementsByClassName_ieFix(container_elt, "newsletter-input-field name_field"))[0] : container_elt.getElementsByClassName("newsletter-input-field name_field")[0];
        var email = email_field.value;
        email = encodeURIComponent(email);
        if(name_field){
            is_valid = is_valid && validate_name(name_field);
            var name = name_field.value;
        }
        if(!is_valid){
            return;
        }
        var site_id = container_elt.getAttribute("data_site_id");
        var source = 0;//Data from WIDGET
        var params = "siteId="+site_id+"&source="+source+"&email="+email;//No I18N
        if(name_field){
            params = params+"&fname="+name;//No I18N
        }
        xml("POST","/siteapps/subscribeUser",subscribe_user_handler,params,container_elt);//No I18N
    }
}

subscribe_user_handler = function(response,container){
    var response_div = (!container.getElementsByClassName) ? (getElementsByClassName_ieFix(container, "newsletter_response"))[0] : container.getElementsByClassName("newsletter_response")[0];
    try{
        var response_object = JSON.parse(response);
        if(response_object.error_code){
            response_div.innerHTML = "Error in adding data";//NO I18N
            response_div.style.color = "red";
            response_div.style.display="block";
        }else if(response_object.subscriber_added && response_object.subscriber_added == "true"){
            response_div.innerHTML = "You have been successfully added to our mailing list";//No I18N
            response_div.style.color = "green";
            response_div.style.display="block";
            var email_field = (!container.getElementsByClassName) ? (getElementsByClassName_ieFix(container, "newsletter-input-field email_field"))[0] : container.getElementsByClassName("newsletter-input-field email_field")[0];
            email_field.value = "";
            var name_field = (!container.getElementsByClassName) ? (getElementsByClassName_ieFix(container, "newsletter-input-field name_field"))[0] : container.getElementsByClassName("newsletter-input-field name_field")[0];
            if(name_field){
                name_field.value = "";
            }
        }
    }catch(e){
        response_div.innerHTML = "Error in adding data";//NO I18N
        response_div.style.color = "red";
        response_div.style.display="block";
    }
}

getBlogPostCommentsCount = function () {
    var count_elements = document.querySelectorAll('span[data-post-id]');//NO I18N
    if(count_elements.length != 0) {
        var resource_ids = [];
        Object.keys(count_elements).map(function(index){ 
            resource_ids.push(count_elements[index].getAttribute('data-post-id'));
        })
        var request_uri = "/siteapps/commentbox/commentscount?resource_ids=" + resource_ids.join();//NO I18N
        xml("GET", request_uri, getBlogPostCommentsCountHandler, null, count_elements);//No I18N
    }
}

getBlogPostCommentsCountHandler = function (response, count_elements) {
    if(response.trim() != "") {
        var count_map = JSON.parse(response);
        Object.keys(count_elements).map(function(index){ 
            var post_id = count_elements[index].getAttribute('data-post-id');
            var count = count_map[post_id];
            count_elements[index].innerHTML = "Comments("+ count +")";//NO I18N
        })
    }
}

