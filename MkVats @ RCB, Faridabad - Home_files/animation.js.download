/*$Id:$*/
window.blurred = true;
(function() {
    var tid = setInterval(function() {
        if ( !window.blurred ) {     
            window.blurred = true;
            var support = transSupport();
            if(support["transitionEnd"]){
                for(var key in ImageRotator.objs){
                    if(!ImageRotator.objs[key].type || (ImageRotator.objs[key].type && ImageRotator.objs[key].type == "carousel")){
                        clearTimeout(ImageRotator.objs[key].timer);
                        ImageRotator.objs[key].start(ImageRotator.objs[key].transition);
                    }
                }
            }
       }
    }, 1000);
    if(window.ZS_PublishMode && !window.ZS_PreviewMode){ 
       window.onblur = function() { window.blurred = true; };
       window.onfocus = function() { window.blurred = false; };
    }else{
       try {
           parent.window.onblur = function() { window.blurred = true; };
           parent.window.onfocus = function() { window.blurred = false; };
       } catch(e) {clearInterval(tid)}
    }
})();


var ImageRotator = function(el, images, transProps, cssProp, bgColors) {
    if(el.id!=="carouselImg") {
        el.style.zIndex=1;
    }else{
        if((typeof(responsiveTheme)!="undefined" && responsiveTheme==true && window.innerWidth<350 ) ||( typeof(parent.responsivePreview)!="undefined" && parent.responsivePreview=="true" && window.innerWidth<350 )){
           transProps.transition.transition="diffuse";//NO I18N
           transProps.transition.type="diffuse";//NO I18N
        }
    }
    var elemChild = el.children;
    this.frontCont=elemChild[0];
    this.frontCont.style.backgroundImage=assetsUrl+'/images/progressbarsmall.gif';//NO I18N
    this.frontCont.style.backgroundPosition="center center";//NO I18N
    this.frontCont.style.backgroundRepeat="no-repeat";//NO I18N
    this.elem=el;
    this.overlay = elemChild[2];
    this.maxW=el.clientWidth;
    this.maxH=el.clientHeight;
    this.bannerwid=el.clientWidth;
    if(el.id==="carouselImg" && (typeof(responsiveTheme)!="undefined" && responsiveTheme==true))
    {
    el.style.width="100%";
    el.style.height=Math.floor(el.clientWidth*(this.maxH/this.maxW))+"px";
    }
    this.type=transProps.type;
    this.belowBgColor = '';
    this.slideCss = cssProp;
    this.slideBg  = bgColors;
    var imgs = [];
    var called=this;
    var imgStripCont = document.createElement("div");
    this.stripCont = imgStripCont;
    imgStripCont.className=(this.type && this.type == "stripe"?"stripe":"film");
    el.appendChild(imgStripCont); 
    var wid = Math.floor(this.maxW/4);
    var totWid=0;
    if(window.location.href.indexOf("/mobile/") !== -1 || ((typeof(responsiveTheme)!="undefined" && responsiveTheme==true && mobile==true) ||(typeof(parent.responsivePreview)!="undefined" && parent.responsivePreview=="true")) || (window.location.href.indexOf("mobile=true") !== -1 &&  window.ZS_PreviewMode) || window.ZS_MobileVer ){
        transProps.control = "off"; //NO I18N
        if(document.addEventListener){
            document.body.addEventListener('orientationchange', function(){return called.changeOrientation1(called)},false);
            document.body.addEventListener('resize', function(){return called.changeOrientation1(called)},false);
        }

    }
   if(window.addEventListener && (typeof(responsiveTheme)!="undefined" && responsiveTheme==true ) ){
            var evnt=window.addEventListener('resize', function(){return called.changeOrientation1(called)},false);
               window.onload=called.changeOrientation1(called); 
   }
fnTouchStart=function(){
    var touchobj = event.changedTouches[0]; // reference first touch point (ie: first finger)
    startx = parseInt(touchobj.clientX) // get x position of touch point relative to left edge of browser
    touchdown=true;
}
fnTouchMove=function(){
            el=event.target;
            var touchobj = event.changedTouches[0]; // reference first touch point (ie: first finger)
            endx = parseInt(touchobj.clientX); // get x position of touch point relative to left edge of browser
            var diff=startx-endx;
            if( el.id=="carouselImg"){
                return;
            }
            if(diff>15){
                if(touchdown==false){
                    return;
                }
                event.stopPropagation();
                event.preventDefault();
                if (event.cancelBubble!=null){
                    event.cancelBubble = true;
                }
                touchdown=false;
                if(called.type=="stripe"){
                    called.next(called);
                }
            }else if(diff<-15){
                if(touchdown==false){
                    return;
                }
                event.stopPropagation();
                event.preventDefault();
                called.previous(called);
                touchdown=false;
            }
        }

    for(var i=0,loaded=0,len=images.length;i<len;i++){
        var img = new Image();
        img.pos = i;
        img.onload=  function(){
            this.onload=null;
            if(called.type && (called.type == "stripe" || called.type == "film")){
                this.height=called.maxH;
                this.setAttribute("style","float:left");
                totWid+=this.clientWidth;
            }
            loaded++;
            if(loaded == len){
                if(called.type && called.type == "stripe"){
                    imgStripCont.style.width=totWid+"px";
                }
            }
            if(this.pos == 0){
                if(!called.type || (called.type && called.type == "carousel")){
                    called.start();
                }
            }

        }
        img.onerror= function(){
            this.onerror=null;
            loaded++;
            if(loaded == len){
                if(!called.type || (called.type && called.type == "carousel")){
                    called.start();
                }
            }
        }
        img.ontouchstart=fnTouchStart;
        img.ontouchmove=fnTouchMove;

        if(this.type && this.type == "stripe")
            imgStripCont.appendChild(img);
        if(this.type && this.type == "film"){
            var div = document.createElement("div");
            div.style.width=wid+"px";
            div.style.height=wid+"px";
            if(i == 0){
                div.style.transition="margin .2s linear"; //NO I18N
            }
            div.className="filmDiv";
            var imgDiv = document.createElement("div");
            imgDiv.style.cssText="width:100%;height:100%;background-image:url('"+images[i]+"');background-position:center center;background-size:150%;background-repeat:no-repeat";//NO I18N
            div.appendChild(imgDiv);
           imgStripCont.appendChild(div); 
        }
        img.src=images[i].url?images[i].url:images[i];
        imgs.push(img);
    }
    if(this.type && this.type == "film"){
        el.style.height=wid+"px";
        this.filmWidth = imgStripCont.firstChild.offsetWidth+14;
    }

    this.images=imgs;
    this.frontCont.style.height=this.maxH+"px";
    this.frontCont.style.width=this.maxW+"px";
        //this.frontCont.style.cssText = CurrentSiteData.siteData.bannerProp.slide.slideCss[this.curPos + 1];

    this.backCont=elemChild[1];
    this.backCont.style.height=this.maxH+"px";
    this.backCont.style.width=this.maxW+"px";
    if(this.type && (this.type == "stripe" || this.type == "film")){
        this.frontCont.style.display="none";
        this.backCont.style.display="none";
    }
    this.curPos=0;
    this.slideURL = images;
    if(el.id == "carouselImg"){
        this.overlay = true;
    }
    this.transSupport=false;
    this.transition = transProps.transition;
    this.delay=transProps.delay*1000;
    this.repeat = transProps.repeat;
    this.control = transProps.control;
    this.transitionStart = false;
    this.action = "next";
    this.background=transProps.background;
    this.box=transProps.box;
    var support = transSupport();
    if(transProps.control == "on"){
        var controls = document.getElementById("carouselCont").cloneNode("true").children;
        var control;
        for(var k=0;control = controls[k];k++){
            var navigation = el.appendChild(control.cloneNode(true));
            if(navigation.className == "zs-slideshow-right-arrow"){
                bindEvent(navigation,'click',function(){return called.next(called)});//No I18N
            }else if(navigation.className == "zs-slideshow-left-arrow"){
                bindEvent(navigation,'click',function(){return called.previous(called)});//No I18N
            }else if(navigation.className == "themeSlideshowOuterContainer"){
                var navCont = navigation;
                for(var m=0,inChd;inChd=navCont.children[m];m++){
                    if(inChd.className == "themeSlideshowInnerContainer"){
                        var ctrlCont = inChd.cloneNode(true);   
                        navCont.innerHTML="";
                        navCont.appendChild(ctrlCont);
                        this.navCont = ctrlCont;
                        for(var n=0,ctrl;ctrl = ctrlCont.children[n];n++){
                            if(ctrl.className == "zs-slideshow-control"){
                                var sampleCtrl = ctrl.cloneNode(true);
                                ctrlCont.innerHTML="";
                                var len = images.length;
                                for(var g=0;g<len;g++){ 
                                    var nav = sampleCtrl.cloneNode(true);
                                    if(g == 0){         
                                        nav.className = "zs-slideshow-control-active";
                                    }                   
                                    ctrlCont.appendChild(nav);
                                    bindEvent(nav,'click',function(){called.navigate(called,this)});//No I18N
                                }
                            }
                        }
                    }
                }
            }
        }
    }
    if(transProps.thumbnail && transProps.thumbnail == "on" && (!this.type || (this.type && this.type == "carousel"))){
        if(transProps.control === "on"){
            this.navCont.style.display='none';
        }
        var thumbCont = document.createElement("div");
        if(transProps.thumbPos == "top" || transProps.thumbPos == "bottom"){
            thumbCont.style.cssText = "z-index: 300;width: 100%;height: 65px;background: transparent;cursor:pointer;float:left;overflow:hidden";//NO I18N
        }else{
            el.style.float="left";//NO I18N
            thumbCont.style.cssText = "z-index: 300;width: 64px;height: "+this.maxH+"px;background: transparent;cursor:pointer;float:left;overflow:hidden";//NO I18N
        }
        this.thumbnail = transProps.thumbnail;
        this.thumbPos = transProps.thumbPos;
        thumbCont.onmousemove=function(event){
            var top1 = document.body.scrollTop?document.body.scrollTop:document.documentElement.scrollTop;
            var top = (event.clientY+top1)-(thumbCont.parentNode.offsetTop);
            var left = event.clientX-thumbCont.parentNode.offsetLeft;
            if(transProps.thumbPos == "top" || transProps.thumbPos == "bottom"){
                var contWidth = thumbCont.clientWidth;
                var scrContWidth = thumbCont.firstChild.clientWidth;
                if(scrContWidth > contWidth){
                    var scroll = ((scrContWidth-contWidth)/contWidth)*left;
                    thumbCont.firstChild.style.left=-scroll+"px";
                }
            }else{
                var contHeight = thumbCont.clientHeight;
                var scrContHeight = thumbCont.firstChild.clientHeight;
                if(scrContHeight > contHeight){
                    var scroll = ((scrContHeight-contHeight)/contHeight)*top;
                    thumbCont.firstChild.style.top=-scroll+"px";
                }
            }
        };
        var thumbs = document.createElement("div");
        this.thumbCont = thumbs;
        if(transProps.thumbPos == "top" || transProps.thumbPos == "bottom"){
            thumbs.style.cssText = "position:relative;height:100%;width:"+((len*58)+8)+"px";//NO I18N
            this.perRoll = Math.floor(el.clientWidth/58);
        }else{
            thumbs.style.cssText = "position:relative;height:"+((len*61)+11)+"px;width:100%";//NO I18N
            this.perRoll = Math.floor(el.clientHeight/61);
        }
        thumbCont.appendChild(thumbs);
        function callThumbNavigate(){
          called.thumbNavigate(called,this); 
        }
        for(var thumb=0;thumb<len;thumb++){
            var div = document.createElement("div");
            div.className=(thumb==0?"withMaskPrev withoutMaskPrev":"withMaskPrev");
            var thumbImg = (images[thumb].url?images[thumb].url:images[thumb]).split("/");
            thumbImg[thumbImg.length-1]="."+thumbImg[thumbImg.length-1]+"_s.jpg";//NO I18N
            thumbImg = thumbImg.join("/");
            div.innerHTML='<img src="'+thumbImg+'" width="50px" height="50px">';
            thumbs.appendChild(div);
            bindEvent(div,'click',function(){called.thumbNavigate(called,this)});//No I18N
            bindEvent(div,'touchstart',callThumbNavigate);//No I18N
        }
        if(transProps.thumbPos == "top" || transProps.thumbPos == "left"){
            el.parentNode.insertBefore(thumbCont,el); 
        }else{
            el.parentNode.appendChild(thumbCont);
        }
    }
    if(this.type && this.type == "film" &&!(typeof(mobile)!="undefined" && mobile)){
   this.navCont.style.display="none"; 
    }
    if(support["transitionEnd"]){
        this.transSupport = support;
    }
    ImageRotator.objs[el.id]=this;
}
ImageRotator.objs={};
transSupport=function(){
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

ImageRotator.prototype={
    resize:function(img,elem,pos){
        var maxH=this.maxH;
        var maxW=this.maxW;
        var ratio = maxH/maxW;
        var percentHeight = Math.floor(img.height*maxW/img.width);
        var percentWidth = Math.floor(img.width*maxH/img.height);
        var presentWid=this.elem.clientWidth;
        var ratio = presentWid/this.bannerwid;        
        this.ratio = ratio;    
        img.style.position="absolute";
        if((typeof(this.slideCss)!="undefined")&&(this.slideCss[pos] == "null" || this.slideCss[pos] == null)&&((this.elem.id==="carouselImg")))
            {
            // img.width = maxW;
            // img.height = percentHeight;
            // if(img.height < maxH){
            //     img.width  = percentWidth;
            //     img.height  = maxH;            
            // }
            img.style.position="absolute";
            img.style.width = maxW + "px";
            img.style.height = "initial";            
        }
        else if( window.location.href.indexOf("/mobile/") === -1 && !(window.location.href.indexOf("mobile=true")!== -1 &&  window.ZS_PreviewMode) && !window.ZS_MobileVer && ((typeof(responsiveTheme)!="undefined" && responsiveTheme==true) || (typeof(responsiveTheme)!="undefined" && responsiveTheme==true && mobile==true))){            
            img.width = maxW;
            img.height = percentHeight;
            if(img.height < maxH){
                img.width  = percentWidth;
                img.height  = maxH;            
            }
            img.style.position="absolute";
            if(this.elem.id==="carouselImg"){  //for banner slideshow
                if((this.bannerwid==presentWid)&&(typeof(parent.responsivePreview)!="undefined" && parent.responsivePreview=="true")){   //for mobile preview 
                    this.ratio=presentWid/((img.style.width).replace("px",""));
                    img.style.width=((img.style.width).replace("px","")*this.ratio)+"px";
                    img.style.left="0px";
                    img.style.top="0px";
                    img.style.height='auto';
                }
                else{
                    img.style.width=((img.style.width).replace("px","")*this.ratio)+"px";
                    img.style.left=((img.style.left).replace("px","")*this.ratio)+"px";
                    img.style.top=((img.style.top).replace("px","")*this.ratio)+"px";
                    img.style.height='auto';
                }
            }
            else          //for slideshow
            {
            img.style.width = img.width + "px";
            img.style.height = "initial";
            }
        }else{
                if(this.elem.id==="carouselImg"){
                    img.style.width=maxW+"px"; 
                    img.style.height="auto";
                }else{
                    if(img.width > maxW && img.height < maxH){
                        img.width = percentWidth;
                        img.height = maxH;
                }else if(img.height > maxH && img.width < maxW){
                    img.height = percentHeight;
                    img.width = maxW;
                }else if((img.width > maxW && img.height > maxH) || (img.width < maxW && img.height < maxH)){
                    if(img.width>img.height){
                        img.width = maxW;
                        img.height = percentHeight;
                    }else{
                        img.width = percentWidth;
                        img.height = maxH;
                    }
                }    
                }
            
        }
        return img;
    },
    start:function(trans){
       this.frontCont.innerHTML="";
        var curImg = this.images[this.curPos];
        curImg.style.position="absolute";
        if(this.background)
            curImg.style.background=this.background[this.curPos];
        if((this.elem.id==="carouselImg")&&(typeof(responsiveTheme)!="undefined" && responsiveTheme==true))
        {
            this.fnPosition(curImg, 'start'); //NO I18N
            this.resize(curImg,this.elem,this.curPos);
        }
        else
        {
        this.resize(curImg,this.elem,this.curPos);
        this.fnPosition(curImg, 'start'); //NO I18N
        }
        if(this.slideURL[this.curPos].link && this.slideURL[this.curPos].link.url && this.slideURL[this.curPos].link.url != ""){
            var anchor = document.createElement("a");
            var link = this.slideURL[this.curPos].link.url;
            anchor.setAttribute("title",this.slideURL[this.curPos].link.title);
            anchor.setAttribute("target",this.slideURL[this.curPos].link.target);
            if(window.ZS_PublishMode && !window.ZS_PreviewMode){
                var urlPat = new RegExp("/files/(\\d+)/"); //NOI18N
                link = link.replace(urlPat,"/files/"); //NOI18N
                anchor.setAttribute("href",link);
            }else if(window.ZS_PreviewMode == true){
                anchor.setAttribute("href",link);
                bindEvent(anchor,'click', fnPreviewClickInfoMsg);   //No I18N
            }else{
                anchor.setAttribute("href",link);
                anchor.setAttribute("onclick","return false;");
            }
            anchor.appendChild(curImg);
            curImg = anchor;
        }
        this.frontCont.appendChild(curImg);
        var dim = this.slideURL[this.curPos].dimension;
        /*if(dim ){
            var wid = this.frontCont.clientWidth;
            var hei = this.frontCont.clientHeight;
            if(wid != dim.width || hei != dim.height ||(typeof(mobile)!="undefined" && mobile)){
                var diffWid = wid-dim.width;
                var widApply = curImg.clientWidth+(diffWid);
             if((typeof(responsiveTheme)!="undefined" && responsiveTheme==true) || (typeof(parent.responsivePreview)!="undefined" && parent.responsivePreview=="true")){
                curImg.style.width=widApply+"px";
                curImg.style.height='initial';
                if((hei-curImg.clientHeight)>curImg.offsetTop){
                    curImg.style.top = (hei-curImg.clientHeight)/2+'px'
                }
            
            }
        }*/
        var called=this;
        this.fnSetOriginal(trans);
        var belowId=this.curPos+1>this.images.length-1?0:this.curPos+1;    
        var overlay="";
        var overlayElem = document.getElementById("overTxt");
        if(this.overlay && this.slideURL[this.curPos].overlay && this.slideURL[this.curPos].overlay.length && window.location.href.indexOf("/mobile/") == -1 && !(window.location.href.indexOf("mobile=true") !== -1 &&  window.ZS_PreviewMode) && !window.ZS_MobileVer ){
            if(!overlayElem){
                overlayElem = document.createElement("div");
                overlayElem.id = "overTxt";
                overlayElem.style.cssText = "position:absolute;top:0;left:0;width:100%;height:100%;opacity:0;transition:opacity .2s linear;-webskit-transition:opacity .2s linear;-moz-transition:opacity .2s linear;";//NO I18N
                this.elem.appendChild(overlayElem);
            }
            if(this.slideURL[this.curPos].link && this.slideURL[this.curPos].link.url){
                overlayElem.setAttribute("title",this.slideURL[this.curPos].link.title);
                var linkUrl = this.slideURL[this.curPos].link.url;
                if(window.ZS_PublishMode && !window.ZS_PreviewMode){
                    var urlPat = new RegExp("/files/(\\d+)/","g"); //NOI18N
                    linkUrl = linkUrl.replace(urlPat,"/files/"); //NOI18N
                    linkUrl = linkUrl.replace(/'/g,"\\'"); //NOI18N
                    overlayElem.setAttribute("onclick","window.open('"+linkUrl+"','"+this.slideURL[this.curPos].link.target+"')");
                }else if(window.ZS_PreviewMode == true){
                    overlayElem.href = linkUrl;
                    bindEvent(overlayElem,'click', fnPreviewClickInfoMsg);   //No I18N
                }
                overlayElem.style.cursor="pointer";
            }else{
                overlayElem.setAttribute("title","");
                overlayElem.setAttribute("onclick","");
                overlayElem.style.cursor="auto";
            }
            overlayElem.style.display="block";
            var elems = this.slideURL[this.curPos].overlay;
            for(var i = 0,elem;elem = elems[i];i++){
                if(elem.type == "text"){
                    var content = elem.content;
                    if(window.ZS_PublishMode && !window.ZS_PreviewMode){
                        var urlPat = new RegExp("href=\"/files/(\\d+)/","g"); //NOI18N
                        content = content.replace(urlPat,"href=\"/files/"); //NOI18N
                        var patt = new RegExp("<a","g");
                        content = content.replace(patt,"<a".concat(" onclick=\"event.stopPropagation();\""));
                    }
                    if(!window.ZS_PublishMode && window.ZS_PreviewMode){
                        var pattern = new RegExp("<a","g");
                        content = content.replace(pattern,"<a".concat(" onclick=\"fnPreviewClickInfoMsg(event);\""));
                    }
                    overlay+="<div style='position:absolute;cursor:default;top:"+elem.top+"px;left:"+elem.left+"px;background:"+elem.background+";color:"+elem.color+";width:"+elem.width+"px;min-height:"+elem.height+"px;padding:10px;font-size:16px;z-index:1' class='zpelement-wrapper bannerText' ";   //NO I18N
           if(window.ZS_PreviewMode || window.ZS_PublishMode)
                    {
                        overlay+='onclick=\"event.stopPropagation()\"';   //NO I18N
                    }
                    overlay+= ">" + content+"</div>";
                 }else{
                    overlay+="<div class=\"zpelement-wrapper bannerBtn\" style=\"position:absolute;top:"+elem.top+"px;left:"+elem.left+"px;z-index:2\">";//NO I18N
                    var buttonStyleProp = elem.buttonStyleProperty;
                    overlay+="<div class=\"zpAlignPos\" style=\"text-align:"+elem.textAlign+";\"><button type=\"button\" style=\"";//NO I18N
                    if(buttonStyleProp.color && buttonStyleProp.color != ""){
                        overlay+="color:"+buttonStyleProp.color+";";//NO I18N
                    }
                    if(buttonStyleProp.fontFamily && buttonStyleProp.fontFamily != ""){
                        overlay+="font-family:"+buttonStyleProp.fontFamily+";";//NO I18N
                    }
                    if(buttonStyleProp.fontSize && buttonStyleProp.fontSize != ""){
                        overlay+="font-size:"+buttonStyleProp.fontSize+";";//NO I18N
                    }
                    if(buttonStyleProp.styleBold && buttonStyleProp.styleBold != ""){
                        overlay+="font-weight:"+buttonStyleProp.styleBold+";";//NO I18N
                    }
                    if(buttonStyleProp.styleItalic && buttonStyleProp.styleItalic != ""){
                        overlay+="font-style:"+buttonStyleProp.styleItalic+";";//NO I18N
                    }
                    if(buttonStyleProp.styleUnderline && buttonStyleProp.styleUnderline != ""){
                        overlay+="text-decoration:"+buttonStyleProp.styleUnderline+";";//NO I18N
                    }
                    if(buttonStyleProp.background && buttonStyleProp.background != ""){
                        overlay+="background:"+buttonStyleProp.background+";";//NO I18N
                    }
                    if(buttonStyleProp.borderWidth){
                        overlay+="border:"+buttonStyleProp.borderWidth+" solid "+buttonStyleProp.borderColor+";";//NO I18N
                    }
                    overlay+="\" ";
                    if(!buttonStyleProp.background)
                        overlay+="data_editbackground=\"default\" ";//No I18N
                    if(!buttonStyleProp.color)
                        overlay+="data_editcolor=\"default\"";//No I18N
                    var buttonLink = elem.buttonLink;                    
                    if(window.ZS_PublishMode && !window.ZS_PreviewMode){
                        var urlPat = new RegExp("/files/(\\d+)/"); //NOI18N
                        buttonLink = buttonLink.replace(urlPat,"/files/"); //NOI18N
                        overlay+=" onclick = \"window.open('"+buttonLink+"','"+elem.linkTarget+"');event.stopPropagation();\" >"+elem.name+"</button></div></div>";//NO I18N
                    }else if(window.ZS_PreviewMode == true){
                            overlay+=" onclick = \"fnPreviewClickInfoMsg(event)\" >"+elem.name+"</button></div></div>";//NO I18N                            
                    }else{
                        overlay+=" blink=\""+buttonLink+"\" btarget='"+elem.linkTarget+"'>"+elem.name+"</button></div></div>";//NO I18N                        
                    }
                }
            }
            overlayElem.innerHTML=overlay;
            overlayElem.style.opacity=1;
            if((typeof(responsiveTheme)!="undefined" && responsiveTheme==true) || (typeof(parent.responsivePreview)!="undefined" && parent.responsivePreview=="true")){
                resizeOverlay();
            }


        }else{
            if(this.overlay && overlayElem){
                overlayElem.style.display="none";
                overlayElem.innerHTML="";
                //overlayElem.style.display=0;
            }
        }
        if(this.images.length == 1){this.transitionStart = true;return;}
        var img = new Image();
        img.onload=function(){
        if(called.timer)clearTimeout(called.timer);
        called.timer = setTimeout(function(){if(called.slideURL[called.curPos].overlay && overlayElem){overlayElem.style.opacity=0;}called.fnTransEffect(100);},called.delay);
        called.transitionStart = false;
        }
        img.src=this.images[belowId].src;
    },
    fnPosition:function(img, calledFrom){
        var pos = 0;
        var belowId=this.curPos+1>this.images.length-1?0:this.curPos+1; 
        if(calledFrom === 'start') {
            pos = this.curPos;
        } else {
            pos = belowId;
            if(this.slideBg && this.slideBg[ this.curPos ] != "null") {
                this.backCont.style.backgroundColor = this.slideBg[ belowId ];
            }
        }
        if((typeof this.slideCss !== 'undefined') && this.slideCss[ pos ]  && window.location.href.indexOf("/mobile/") === -1 && !(window.location.href.indexOf("mobile=true") !== -1 &&  window.ZS_PreviewMode) && !window.ZS_MobileVer && (this.slideCss[pos] != "null" || this.slideCss[pos] == null ) ) {
            img.style.cssText = this.slideCss[ pos ];

        } else {
             if(this.elem.id==="carouselImg"){
                img.style.left=((img.style.left).replace("px","")*this.ratio)+"px";
                img.style.top=((img.style.top).replace("px","")*this.ratio)+"px";
            }else{
                img.style.top=(this.maxH-(img.height))/2+"px";
                img.style.left=(this.maxW-(img.width))/2+"px";
            }
        }

    },
    fnTransEffect:function(dur){
        this.transitionStart = true;
        var params={};
        params.elem=this.frontCont;
        params.swap=this.backCont;
        params.callBack=this.callBack;
        params.called=this
        params.duration=dur;
        params.chg=0;
        var swpImg = this.fnSetSwap();
        if(!this.transSupport){ 
            params.elem.style.filter="alpha(opacity=100)";
            params.swap.style.filter="alpha(opacity=0)";
            params.elem.style.zIndex=2;
            params.swap.style.display="block";
            params.max=1;
            params.each=params.max/params.duration;
            params.trans="diffuse";//No I18N
            var func=function(){fnFade(params)};
            this.timer=setInterval(func,1);
            return;
        }
        var transition=this.fnGenTrans();
        //params.type=transition.type;
        params.transition=transition;
        var trans = this.transSupport['transitionEnd'];
        if(transition.transition == "slide"){
            if(transition.type == "top"){
                    this.backCont.style.top=this.maxH+"px";
            }else if(transition.type == "down"){
                    this.backCont.style.top=-this.maxH+"px";
            }else if(transition.type == "left"){
                    this.backCont.style.left=this.maxW+"px";
            }else if(transition.type == "right"){
                    this.backCont.style.left=-this.maxW+"px";
            }
            params.swap.style.display="block";


            if(transition.type == "top" || transition.type == "down"){
                params.swap.style.left = "0px";
                params.elem.style[this.transSupport['transition']] = "top 800ms cubic-bezier(0.770, 0.000, 0.075, 1.000)";//No I18N
                params.swap.style[this.transSupport['transition']] = "top 800ms cubic-bezier(0.770, 0.000, 0.075, 1.000)";//No I18N
                if(transition.type == "down"){
                    params.elem.style.top=this.maxH+"px";
                    var func=function(){params.swap.style.top=0+"px";}
                    setTimeout(func,20);
                }else{
                    params.elem.style.top=-this.maxH+"px";
                    var func=function(){params.swap.style.top=0+"px";}
                    setTimeout(func,20);
                }
            }
            else{
                this.backCont.style.top="0px";
                params.elem.style[this.transSupport['transition']] = "left 800ms cubic-bezier(0.770, 0.000, 0.175, 1.000)";//No I18N
                params.swap.style[this.transSupport['transition']] = "left 800ms cubic-bezier(0.770, 0.000, 0.075, 1.000)";//No I18N
                if(transition.type == "left"){
                    params.elem.style.left=-this.maxW+"px";
                    var func=function(){params.swap.style.left=0;}
                    setTimeout(func,20);
                }else{
                    params.elem.style.left=this.maxW+"px";
                    var func=function(){params.swap.style.left=0+"px";}
                    setTimeout(func,20);
                }
            }
            bindEvent(params.swap,trans,function(){unbindEvent(params.swap,trans,arguments.callee);return params.callBack(params.called,params.transition)});
        }
        else if(transition.transition == "diffuse"){
            params.max=1;
            params.elem.style[this.transSupport['transition']] = "opacity 800ms ease-in";//No I18N
            var elem;
            //params.swap.style.cssText="position:absolute;top:0px;left:0px;opacity:0;display:block;width:"+this.maxW+"px;height:"+this.maxH+"px;overflow:hidden;z-index:5;background:#fff";//No I18N
            params.swap.style.cssText="position:absolute;top:0px;left:0px;opacity:0;display:block;width:"+this.maxW+"px;height:"+this.maxH+"px;overflow:hidden;z-index:5;";//No I18N
            if(transition.type == "diffuse"){
                params.swap.style[this.transSupport['transition']] = "opacity 500ms ease-in";//No I18N
                params.elem.style.opacity=0;
                var func=function(){params.swap.style.opacity=1;}
                setTimeout(func,20);
                bindEvent(params.swap,trans,function(){unbindEvent(params.swap,trans,arguments.callee);return params.callBack(params.called,params.transition)});

            }else{
                var tileNo = 10;
                var tileWidth = Math.ceil(this.maxW/tileNo);
                this.no = tileNo;
                this.boxList=[];
                for(var i=0;i<tileNo;i++){
                    var img=this.backCont.cloneNode(true);
                    img.style.clip = "rect(0px,"+((i*tileWidth)+tileWidth)+"px,"+this.maxH+"px,"+(i*tileWidth)+"px)"//No I18N
                    img.style[this.transSupport['transition']] = "opacity 400ms ease-in";//No I18N
                    this.elem.appendChild(img);
                    if(transition.type == "left"){
                        if(i == tileNo-1){
                            elem = img;
                        }
                    }else{
                        if(i == 0){
                            elem = img;
                        }

                    }
                    this.boxList.push(img);
                }
                bindEvent(elem,trans,function(){unbindEvent(elem,trans,arguments.callee);return params.callBack(params.called,params.transition)});
                params.swap.style.display="none";
                for(var j=0;j<tileNo;j++){
                    if(transition.type == "left"){
                        var timer=new this.fnSetTimeOut(70);
                        var totalClip=timer(this.boxList[j],j);
                    }else{
                        var timer=new this.fnSetTimeOut(70);
                        var totalClip=timer(this.boxList[tileNo-(j+1)],j);
                    }
                 }
                
            }
        }else if(transition.transition == "splice"){
            this.backCont.style.top=0;
            this.backCont.style.left=0;
            var no = 10;
            var spliceWidth=Math.ceil(this.maxW/10);
            this.no=no;
            this.boxList=[];
            params.max=this.maxH;
            for(var i=0;i<no;i++){
                var img=this.backCont.cloneNode(true);
                if(transition.type == "zigzag"){
                    if(i%2 == 0){
                        img.style.clip="rect(0px,"+((i*spliceWidth)+spliceWidth)+"px,0px,"+(i*spliceWidth)+"px)"//No I18N
                    }else{
                        img.style.clip="rect("+this.maxH+"px,"+((i*spliceWidth)+spliceWidth)+"px,"+this.maxH+"px,"+(i*spliceWidth)+"px)"//No I18N
                    }
                }else if(transition.type == "top"){
                    img.style.clip="rect(0px,"+((i*spliceWidth)+spliceWidth)+"px,0px,"+(i*spliceWidth)+"px)"//No I18N
                }else{
                    img.style.clip="rect("+this.maxH+"px,"+((i*spliceWidth)+spliceWidth)+"px,"+this.maxH+"px,"+(i*spliceWidth)+"px)"//No I18N
                }
                img.style.display="block";
                img.style.opacity=1;
                this.elem.appendChild(img);
                img.style[this.transSupport['transition']] = "all 300ms ease-in";//No I18N
                this.boxList.push(img);
            }
            bindEvent(img,trans,function(){unbindEvent(img,trans,arguments.callee);return params.callBack(params.called,params.transition)});
            params.duration=Math.ceil(params.duration/2);
            var called=this;
            for(var j=0;j<no;j++){
                var elem=this.boxList[j];
                (function(elem,pos){
                    var func = function(){
                    elem.style.clip="rect(0px,"+((pos*spliceWidth)+spliceWidth)+"px,"+called.maxH+"px,"+(pos*spliceWidth)+"px)"//No I18N
                    elem.style.opacity=1;}
                    setTimeout(func,(pos+1)*50);
                }(elem,j));
            }
        }else if(transition.transition == "tile"){
            var maxW=90;
            var maxH=90;
            if(this.box){
                maxW = this.box.maxW;
                maxH = this.box.maxH;
            }
            var maxRows=Math.ceil(this.maxW/maxW);
            var maxCols=Math.ceil(this.maxH/maxH);
            var totTiles=maxRows*maxCols;
            this.no=totTiles;
            this.boxList=[];
            var maxGrow=20;
            for(var i=0;i<totTiles;i++){
                  var img=this.backCont.cloneNode(true);
                  img.style.zIndex=3;
                  img.style.top="0px";
                  var top=Math.floor(i/maxRows);
                  var left=(i%maxRows);
                  if(transition.type == "fadeInTop" || transition.type == "fadeInBot" || transition.type == "fadeInRandom"){
                      img.style.clip="rect("+(top*maxH)+"px,"+((left*maxW)+maxW)+"px,"+((top*maxH)+maxH)+"px,"+(left*maxW)+"px)";//No I18N
                  }
                  else if(transition.type =="growTopLeft"){
                        img.style.clip="rect("+(top*maxH)+"px,"+(left*maxW+maxW-maxGrow)+"px,"+(top*maxH+maxH-maxGrow)+"px,"+(left*maxW)+"px)";//No I18N

                  }else if(transition.type == "growBotRight"){
                        img.style.clip="rect("+((top*maxH)+maxGrow)+"px,"+(left*maxW+maxW)+"px,"+(top*maxH+maxH)+"px,"+((left*maxW)+maxGrow)+"px)";//No I18N
                  }
                  img.style.opacity=0;
                  img.style[this.transSupport['transition']] = "all 350ms ease-in";//No I18N
                  img.style.left="0px";
                  img.style.display="block";
                  this.elem.appendChild(img);
                  this.boxList.push(img);
            }
            params.max=1;
            params.duration=dur-20;
            var delay=100;
            if(transition.type == "fadeInTop" || transition.type == "growTopLeft"){
                for(var j=0;j<maxRows;j++){
                    for(var i=0;i<j+1;i++){
                        
                        if(transition.type == "fadeInTop"){
                            var timer=new this.fnSetTimeOut(80);
                            var totalClip=timer(this.boxList[j+(i*(maxRows-1))],j);
                        }else{
                            var timer=new this.fnSetTimeOut(80,true,transition.type);
                            var totalClip=timer(this.boxList[j+(i*(maxRows-1))],j,maxGrow);
                        }
                    }
                }
                for(var x=1;x<maxCols+1;x++){
                    for(var z=x;z<maxCols;z++){
                        if(transition.type == "fadeInTop"){
                            var timer=new this.fnSetTimeOut(80);
                            var totalClip=timer(this.boxList[(((z*maxRows)+(maxRows-1))-(z-x))],maxRows+x);
                        }else{
                            var timer=new this.fnSetTimeOut(80,true,transition.type);
                            var totalClip=timer(this.boxList[(((z*maxRows)+(maxRows-1))-(z-x))],maxRows+x,maxGrow);
                        }
                    }
                }
            }
            else if(transition.type == "fadeInBot" || transition.type == "growBotRight"){
                for(var x=maxCols;x>0;x--){
                    for(var z=x;z<maxCols;z++){
                        if(transition.type == "fadeInBot"){
                            var timer=new this.fnSetTimeOut(80);
                            var totalClip=timer(this.boxList[(((z*maxRows)+(maxRows-1))-(z-x))],maxCols-x)
                        }else{
                            var timer=new this.fnSetTimeOut(80,true,transition.type);
                            var totalClip=timer(this.boxList[(((z*maxRows)+(maxRows-1))-(z-x))],maxCols-x,maxGrow);
                        }
                    }
                }
                for(var j=maxRows-1;j>=0;j--){
                    for(var i=0;i<j+1;i++){
                        if(transition.type == "fadeInBot"){
                            var timer=new this.fnSetTimeOut(80);
                            var totalClip=timer(this.boxList[(j+(i*(maxRows-1)))],maxCols+(maxRows-j))
                        }else{
                            var timer=new this.fnSetTimeOut(80,true,transition.type);
                            var totalClip=timer(this.boxList[(j+(i*(maxRows-1)))],maxCols+(maxRows-j),maxGrow);
                        }
                    }
                }
            }else if(transition.type == "fadeInRandom"){
                var nos=[];
                for(var i=0;i<totTiles;i++){
                    nos.push(i);
                }
                var nos=this.fnSetRandNos(nos);
                var len=nos.length;
                for(var k=0;k<len;k++){
                     var timer=new this.fnSetTimeOut(20);
                var totalClip=timer(this.boxList[nos[k]],k);
                }
                delay=35;
            }
            var func1=function(){return params.callBack(params.called,params.transition)};
            setTimeout(func1,delay*totalClip);
        }else if(transition.transition == "slice"){
                var tileNo=10;
                var elem;
                var spliceWidth=Math.ceil(this.maxW/tileNo);
                this.no=tileNo;
                this.boxList=[]
                for(var i=0;i<tileNo;i++){
                    var img=this.backCont.cloneNode(true);
                    img.style.top="0px";
                    if(transition.type == "rollRight"){
                        if(i==0){
                            elem=img;
                        }
                        img.style.clip="rect(0px,"+((i+1)*spliceWidth)+"px,"+this.maxH+"px,"+((i+1)*spliceWidth)+"px)";//No I18N
                    }else{
                        if(i == tileNo-1){
                            elem=img;
                        }
                        img.style.clip="rect(0px,"+(i*spliceWidth)+"px,"+this.maxH+"px,"+(i*spliceWidth)+"px)";//No I18N
                    }
                   img.style.zIndex=55;
                    img.style.left="0px";
                    img.style.display="block";
                    img.style.opacity=0;
                    img.style[this.transSupport['transition']]="all 400ms ease-in";//No I18N
                    this.elem.appendChild(img);
                    this.boxList.push(img);
                }
                bindEvent(elem,trans,function(){unbindEvent(elem,trans,arguments.callee);return params.callBack(params.called,params.transition)});
                params.max=spliceWidth;
                if(transition.type == "rollLeft" || transition.type == "rollSimul"){
                    for(var j=0;j<tileNo;j++){
                        var timer=new this.fnSetTimeOut(50,true,transition.type);
                        if(transition.type == "rollLeft"){
                            var totalClip=timer(this.boxList[j],j,spliceWidth);
                        }else{
                            var totalClip=timer(this.boxList[j],0,spliceWidth);
                        }
                    }
                }else if(transition.type == "rollRight"){
                    for(var j=tileNo-1;j>=0;j--){
                        var timer=new this.fnSetTimeOut(50,true,transition.type);
                        var totalClip=timer(this.boxList[j],tileNo-j,spliceWidth);
                    }
                }
        }

    },
    fnSetTimeOut:function(timer,clip,transition){
        return function(i,j,width){
            setTimeout((function(){return function(){
                var elem=i;
                if(!elem)return;
                elem.style.opacity=1;
                elem.style.top=0;
                if(clip){
                    var Clip=elem.style.clip.split(", ");
                    (Clip.length==4)?Clip=Clip:Clip=elem.style.clip.split(" ");
                    Clip=fnClip(Clip);
                    var clipping;
                    if(transition == "rollRight"){
                        clipping="rect("+Clip.top+"px,"+Clip.width+"px,"+Clip.height+"px,"+(Clip.left-width)+"px)";//No I18N
                    }else if(transition == "rollLeft" || transition == "rollSimul"){
                        clipping="rect("+Clip.top+"px,"+(Clip.width+width)+"px,"+Clip.height+"px,"+Clip.left+"px)";//No I18N
                    }else if(transition == "growTopLeft"){
                        clipping="rect("+Clip.top+"px,"+(Clip.width+width)+"px,"+(Clip.height+width)+"px,"+Clip.left+"px)";//No I18N
                    }else if(transition == "growBotRight"){
                        clipping="rect("+(Clip.top-width)+"px,"+Clip.width+"px,"+Clip.height+"px,"+(Clip.left-width)+"px)";//No I18N
                    }
                    elem.style.clip=clipping;
                }
            }})(),timer*(j+1));
         return j;
        }
       
    },
    fnSetRandNos:function(noArr){
        var retunArr=[];
        var len=noArr.length;
        for(var j=len;j>0;j--){
            var temp=Math.floor(Math.random()*j);
            var no=noArr.splice(temp,1);
            retunArr.push(no[0]);
        }
        return retunArr;
    },
    fnRemoveTiles:function(no){
        for(var i=0;i<no;i++){
            if(this.boxList[i] && this.boxList[i].parentNode == this.elem){
                this.elem.removeChild(this.boxList[i]);
            }
        }
    },
    fnSetSwap:function(){
        var belowId;
        if(this.action == "next"){
            belowId=this.curPos+1>this.images.length-1?0:this.curPos+1;
        }else if(this.action == "previous"){
            belowId=this.curPos-1<0?this.images.length-1:this.curPos-1;
        }else{
            belowId = this.action;
        }
        var swpImg = this.images[belowId];
        var img = this.resize(swpImg,this.elem,belowId);
        swpImg.style.position="absolute";
        if(this.background){
            swpImg.style.background=this.background[belowId];
        }
        this.fnPosition(swpImg,'setSwap',belowId); //NO I18N
        this.backCont.innerHTML="";
        this.backCont.appendChild(swpImg);
        var dim = this.slideURL[belowId].dimension;
        if(dim){
            var wid = this.frontCont.clientWidth;
            var hei = this.frontCont.clientHeight;
            if(wid != dim.width || hei != dim.height){
                this.backCont.style.opacity = 0;
                this.backCont.style.display = "block";
                var diffWid = wid-dim.width; 
                var widApply = swpImg.clientWidth+(diffWid);
                if( !((window.location.href.indexOf("mobile=true") !== -1 &&  window.ZS_PreviewMode)|| !(typeof(responsiveTheme)!="undefined" && responsiveTheme==true)) ){
                //swpImg.style.width=widApply+"px";
                swpImg.style.height='initial';
                }
                // if((hei-swpImg.clientHeight)>swpImg.offsetTop){
                //     swpImg.style.top = (hei-swpImg.clientHeight)/2+'px'
                // }
                this.backCont.style.display = "none";
                this.backCont.style.opacity ="";
            }      
        }    
        this.fnSetCtrl(belowId);
        if(this.thumbnail && this.thumbnail == "on"){
            var children = this.thumbCont.children;
            var child;
            for(var i=0;child=children[i];i++){
                child.className = "withMaskPrev";
            }
            children[belowId].className = "withMaskPrev withoutMaskPrev";
            if(belowId%this.perRoll == 0){
                if(this.thumbPos == "top" || this.thumbPos == "bottom"){
                    this.thumbCont.style.left=-children[belowId].offsetLeft+"px";
                }else{
                    this.thumbCont.style.top=-children[belowId].offsetTop+"px";
                }
            }
        }
        // if(this.elem.id==="carouselImg")
        // {
        //     var img = this.resize(swpImg,this.elem,this.curPos+1);   
        //     this.fnPosition(swpImg,'setSwap',belowId);      //NO I18N
        //     var img = this.resize(swpImg,this.elem,this.curPos+1);
        // }

        return swpImg;
    },
    fnSetCtrl:function(id){
        if(this.control == "on"){
            var children = this.navCont.children;
            var child;
            for(var i=0;child=children[i];i++){
                child.className = "zs-slideshow-control";
            }
            children[id].className = "zs-slideshow-control-active";
        }
    },
    fnSetOriginal:function(trans){
        this.frontCont.style.cssText="position:absolute;top:0px;left:0px;opacity:1;filter:alpha(opacity=100);display:block;width:"+this.maxW+"px;height:"+this.maxH+"px;overflow:hidden;-webkit-backface-visibility:hidden";//No I18N
        if(this.slideBg && this.slideBg[ this.curPos ]) {
            this.frontCont.style.backgroundColor = this.slideBg[ this.curPos ];
        }/*else if(this.slideBg && this.slideBg[ this.curPos ]) {
            this.backCont.style.backgroundColor = this.slideBg[ this.curPos ];
        }*/
        this.backCont.style.display="none";
        if(!trans)return;
        var transition = trans.transition;
        if(transition == "splice" || transition == "tile" || transition == "slice" || (transition == "diffuse" && (trans.type == "left" || trans.type == "right"))){
            this.fnRemoveTiles(this.no);
        }
    },
    callBack:function(called,trans){
        if(called.repeat == "on"){
            if(called.action == "next"){
                called.curPos=called.curPos+1>called.images.length-1?0:called.curPos+1;
            }else if(called.action == "previous"){
                called.action="next";
                called.curPos=called.curPos-1<0?called.images.length-1:called.curPos-1;
            }else{
                called.curPos=called.action;
                called.action="next";
            }
            called.start(trans);
        }else{
            if((called.curPos+1) < called.images.length-1){
                if(called.action == "next"){
                    called.curPos=called.curPos+1>called.images.length-1?0:called.curPos+1;
                }else if(called.action == "previous"){
                    called.curPos=called.curPos-1<0?called.images.length-1:called.curPos-1;
                }else{
                    called.curPos=called.action;
                    called.action="next";
                }
                called.start(trans);
            }
        }
    },
    fnGenTrans:function(){
        if(this.transition && this.transition.transition != "random")return this.transition;
        var transitions=["slide","diffuse","splice","tile","slice"];//No I18N
        var transEff=transitions[Math.floor(Math.random()*5)];
        var transType;
        if(transEff == "slide"){    
            transType=["top","left","right","down"];//No I18N
            transType=transType[Math.floor(Math.random()*4)];
        }else if(transEff == "diffuse"){
            transType=["diffuse","left","right"];//NO I18N
            transType=transType[Math.floor(Math.random()*3)];
        }else if(transEff == "splice"){
            transType=["zigzag","top","bot"];//No I18N
            transType=transType[Math.floor(Math.random()*3)];
        }else if(transEff == "tile"){
            transType=["fadeInTop","fadeInBot","fadeInRandom","growTopLeft","growBotRight"];//No I18N
            transType=transType[Math.floor(Math.random()*5)];
        }else if(transEff == "slice"){
            transType=["rollLeft","rollRight","rollSimul"];//No I18N
            transType=transType[Math.floor(Math.random()*3)];
        }
        
        return {transition:transEff,type:transType}
    },
    fnChangeProps:function(params){
        this.transition=params.transition;
        this.delay=params.delay*1000;
        this.repeat = params.repeat;
    },
    changeOrientation1:function(called){
        var banner=document.getElementById("carouselImg");
        if(this.elem.id!=="carouselImg")
        {
          this.maxW = this.elem.clientWidth;
          this.maxH = this.elem.clientHeight;  
        }
        
        if(typeof(responsiveTheme)!="undefined" && responsiveTheme==true ){
            var padWidth=0;
           if(this.elem.id==="carouselImg"){
                padWidth = parseInt(navGetStyle(this.elem.parentNode,'padding-left').replace("px",""));;//No I18N
                banner.style.height=Math.floor((this.maxH/this.maxW)*(this.elem.parentNode.clientWidth-padWidth*2))+"px";
                banner.style.width=Math.floor(this.elem.parentNode.clientWidth-padWidth*2)+"px";
                banner.parentNode.style.height=banner.clientHeight+"px";
                this.maxH=Math.floor((this.maxH/this.maxW)*(this.elem.parentNode.clientWidth-padWidth*2));
                this.maxW=Math.floor(this.elem.parentNode.clientWidth-padWidth*2);

            }
           // var currImg = this.images[this.curPos];
            //if(this.elem.id==="carouselImg"){
              //  this.fnPosition(currImg,'start');       //NO I18N    
            //}
            //this.resize(currImg,this.elem,this.curPos);
            if(this.type!="stripe" && this.type!="film"){
                //called.fnTransEffect(100);
            }else if(this.type=="film"){
                var childNode=getClasses("film","div",this.elem)[0].children;//NO I18N
                var len=childNode.length;
                var wid=this.maxW/4;
                for(i=0;i<len;i++){
                    childNode[i].style.width=wid+"px";
                    childNode[i].style.height=wid+"px";
                }
            }else if(this.type=="stripe"){
                    var childNode=getClasses("stripe","div",this.elem)[0].children;//NO I18N
                    var len=childNode.length;
                    var wid=this.maxW/4;
                    for(i=0;i<len;i++){
                        childNode[i].style.width=Math.floor((childNode[i].clientWidth/childNode[i].clientHeight)*this.maxH)+"px";
                        childNode[i].style.height=this.maxH+"px";
                    }

            }

        }
    },
    next:function(called){
        if(!called.transitionStart){
            called.action = "next";
            clearTimeout(called.timer);
            if(this.type && (this.type == "stripe" || this.type == "film")){
                if(this.curPos < this.images.length-1){
                    if(this.type == "stripe"){
                        var el=this.stripCont.children[this.curPos+1];   
                        var left=(this.maxW-el.clientWidth)/2;
                        if(this.curPos == this.images.length-2){
                            if(this.stripCont.clientWidth > this.maxW){
                                this.stripCont.style.left=-el.offsetLeft+(left*2)+"px";
                            }
                        }else{
                            if(-this.stripCont.offsetLeft+left<el.offsetLeft && this.stripCont.clientWidth-el.offsetLeft > this.maxW-left ){
                                this.stripCont.style.left=-el.offsetLeft+left+"px";
                            }
                        }   
                    }else{
                        this.stripCont.children[0].style.marginLeft = -(this.curPos+1)*this.filmWidth+"px";
                    }
                    this.curPos+=1;
                }else{
                    if(this.type == "stripe"){
                        this.stripCont.style.left="0px";
                    }else{
                        this.stripCont.children[0].style.marginLeft = "7px";
                    }
                    this.curPos=0;
                }
                this.fnSetCtrl(this.curPos);
            }else{
                var overlayElem = document.getElementById("overTxt");
                if(overlayElem && this.overlay){
                    overlayElem.style.display="none";
                }
                called.fnTransEffect(100);
            }
        }
    },
    previous:function(called){
        if(!called.transitionStart){
            called.action = "previous";
            clearTimeout(called.timer);
            if(this.type && (this.type == "stripe" || this.type == "film")){
                if(this.curPos > 0){
                    if(this.type == "stripe"){
                        var el=this.stripCont.children[this.curPos-1];
                        var left=(this.maxW-el.clientWidth)/2;
                        if(this.curPos == 1){
                            this.stripCont.style.left="0px";
                        }else{
                            if(this.stripCont.clientWidth-el.offsetLeft > this.maxW-left && el.offsetLeft > left){
                            this.stripCont.style.left=-el.offsetLeft+left+"px";}
                        }
                    }else{
                        if(this.curPos == 1){
                            this.stripCont.children[0].style.marginLeft = "7px";
                        }else{
                            this.stripCont.children[0].style.marginLeft = -(this.curPos-1)*this.filmWidth+"px";
                        }
                    }
                    this.curPos-=1;
                }else{
                    if(this.type == "stripe"){
                        var el=this.stripCont.children[this.images.length-1];
                        var left=(this.maxW-el.clientWidth);
                        if(this.stripCont.clientWidth >this.maxW){
                        this.stripCont.style.left=-el.offsetLeft+left+"px";}
                    }else{
                       this.stripCont.children[0].style.marginLeft = -(this.images.length-1)*this.filmWidth+"px"; 
                    }
                    this.curPos=this.images.length-1;
                }
                this.fnSetCtrl(this.curPos);
            }else{
                var overlayElem = document.getElementById("overTxt");
                if(overlayElem && this.overlay){
                    overlayElem.style.display="none";
                }  
                called.fnTransEffect(100);
            }
        }
    },
    navigate:function(called,src){
        var id = called.fnGetIndex(src,this.navCont); 
        if(!called.transitionStart && id != called.curPos){
            called.action = parseInt(id,10);
            clearTimeout(called.timer);
            if(this.type && this.type == "stripe"){
                var el=called.stripCont.children[called.action];
                var left=(called.maxW-el.clientWidth)/2;
                if(called.action == 0 || called.action == called.images.length-1){
                    called.stripCont.style.left=(called.action == 0?"0":-el.offsetLeft+(left*2))+"px";    
                }else{
                    called.stripCont.style.left=-el.offsetLeft+left+"px";
                }
                called.curPos=called.action;
                called.fnSetCtrl(called.curPos);
            }else if(this.type && this.type == "film"){
                this.stripCont.children[0].style.marginLeft = -called.action*this.filmWidth+"px";
                called.curPos=called.action;
                called.fnSetCtrl(called.curPos);
            }else{
                var overlayElem = document.getElementById("overTxt");
                if(overlayElem && this.overlay){
                    overlayElem.style.display="none";
                }  
                called.fnTransEffect(100);
            }
        }
    },
    thumbNavigate:function(called,src){
        var id = called.fnGetIndex(src,this.thumbCont);
        if(!called.transitionStart && id != called.curPos){
            called.action = parseInt(id,10);
            clearTimeout(called.timer);
            called.fnTransEffect(100);
        }
    },
    fnGetIndex:function(elem,cont){
        var children = cont.children;
        for(var i=0,child;child=children[i];i++){
            if(child == elem){
                return i;
            }
        }
    }
}
fnToNum=function(pix){
    return parseFloat(pix.replace("px",""));
}
fnClip=function(str){
    var top=parseFloat(str[0].substring(str[0].indexOf("(")+1,str[0].length-2));
    var width=parseFloat(str[1].substring(0,str[1].length-2));
    var height=parseFloat(str[2].substring(0,str[2].length-2));
    var left=parseFloat(str[3].substring(0,str[3].length-3));
    return {top:top,width:width,height:height,left:left};
}
fnFade=function(params){
    params.chg+=params.each;
    if(params.chg >= params.max){
        params.chg=params.max;
        clearInterval(params.called.timer)
        params.called.timer=0;
        params.called.callBack(params.called,params.trans);
        return;
    }
    params.elem.style.opacity=1-params.chg;
    params.elem.style.filter="alpha(opacity="+(100-(params.chg*100))+")";
    params.swap.style.opacity=params.chg;
    params.swap.style.filter="alpha(opacity="+(params.chg*100)+")";
}
bindEvent=function(el,type,func){
    if(el.addEventListener){
        el.addEventListener(type,func,false);
    }else if(el.attachEvent){
        el.attachEvent('on'+type,func);
    }
}
unbindEvent=function(el,type,handler){
    if(handler){
        if(el.removeEventListener){
            el.removeEventListener(type,handler,false);    
        }else if(el.detachEvent){
            el.detachEvent('on'+type,handler);
        }
    }
}
function getInternetExplorerVersion()
    // Returns the version of Internet Explorer or a -1
{
    var rv = -1; // Return value assumes failure.
        var ua = navigator.userAgent;
        var re  = new RegExp("MSIE ([0-9]{1,}[\.0-9]{0,})");
        if (re.exec(ua) !== null){
            rv = parseFloat( RegExp.$1 );
        }
    return rv;
}
