/*$Id$*/
fnPausePlay = function(){
    var audio = this.parentNode.parentNode;
    if(this.className == "playBtn"){
        var loop = false;
        if(audio.native){
            if(audio.audio.getAttribute('loop')){
                loop = true;
                audio.audio.removeAttribute('loop');
            }
        }
        audio.audio.play();
        if(audio.native){
            if(loop){
                audio.audio.setAttribute('loop', 'true');
            }
        }
        if(!audio.native){
            audio.timer = setInterval(function(){
                var move = audio.audio.getCurrentTime()/1000;
                var duration = audio.audio.getDuration()/1000;
                var mins = Math.floor(move/60);
                var secs = Math.round(move%60);
                fnGetElem(".timer",audio).innerHTML=(mins>9?mins:"0"+mins)+":"+(secs>9?secs:"0"+secs);//NO I18N
                fnGetElem(".sliderThumb",audio).style.left=(155/duration*move)+"px";
                fnGetElem(".sliderBar",audio).style.width=(155/duration*move)+"px";
                },100);
        }
        this.className = "pauseBtn";
    }else{
        audio.audio.pause();
        if(!audio.native){
            clearInterval(audio.timer);
        }
        this.className = "playBtn";
    }
}

function fnPlayPrev(){
    var audio = this.parentNode.parentNode;
    fnEndAudio(audio, true);
}

function fnPlayNext(){
    var audio = this.parentNode.parentNode;
    fnEndAudio(audio);
    /*var repeat = fnGetElem("#repeat", audio).className;
    if(repeat !== "repeatIcon-one"){
        if(audio.curPlayItem == audio.playList.length-1){
            audio.curPlayItem = 0;
        }else{
            audio.curPlayItem+=1;
        }
    }
    fnSetupAudio(audio,true,false);*/
}

function fnShowHidePL(){
    var audio = this.parentNode.parentNode.parentNode;
    var audHei = audio.clientHeight;
    var listHei = fnGetElem(".playlistContainer",audio).clientHeight;//NO I18N
    var plState = this.getAttribute("data-show");
    if(plState === "true"){
        audio.style.height = audHei-listHei+"px";
        this.setAttribute("data-show","false");
    }else{
        audio.style.height = audHei+listHei+"px";
        this.setAttribute("data-show","true");
    }
}

function fnRepeatPlayList(){
    var aud = this.parentNode.parentNode;
    if(this.className === "repeatIcon-off"){
        if(aud.playList){
            aud.repeat = "all";//NO I18N
        }else{
            if(aud.native){
                aud.audio.setAttribute("loop", "true");
            }else{
                aud.loop = true;
            }
        }
        this.className = "repeatIcon-on";
    }else if(this.className === "repeatIcon-on"){
        if(!aud.playList){
            if(aud.native){
                aud.audio.removeAttribute("loop");
            }else{
                aud.loop = false;
            }
            this.className = "repeatIcon-off";
        }else{
            aud.repeat = "one";//NO I18N
            this.className = "repeatIcon-one";
        }
    }else if(this.className === "repeatIcon-one"){
        aud.repeat = false;//NO I18N
        this.className = "repeatIcon-off";
    }
}

function fnShufflePlaylist(){
    var aud = this.parentNode;
    if(this.className === "shuffleIcon-off"){
        this.className = "shuffleIcon-on";
        aud.shuffle = true;
    }else{
        aud.shuffle = false;
        this.className = "shuffleIcon-off";
    }
}

function fnPlayThis(){
    var audio = this.parentNode.parentNode;
    var item = this.getAttribute("data-item");
    audio.curPlayItem = parseInt(item);
    fnSetupAudio(audio,false,false);
}

fnChangeAudioPos = function(e){
    if((e.srcElement?e.srcElement:e.target).className == "sliderThumb")return;
    var left = e.layerX;//e.clientX-(this.offsetLeft);
    if(this.parentNode.parentNode.native){
        try{
            this.parentNode.parentNode.audio.currentTime=this.parentNode.parentNode.audio.duration/155*left;
        }catch(e){
            fnPausePlay.call(fnGetElem(".playBtn",this.parentNode.parentNode));//NO I18N
            var src = this;
            setTimeout(function(){src.parentNode.parentNode.audio.currentTime=src.parentNode.parentNode.audio.duration/155*left;},300);
        }
    }else{
        if(this.parentNode.parentNode.audio.isPlaying()){
            this.parentNode.parentNode.audio.play(this.parentNode.parentNode.audio.getDuration()/155*left); 
        }else{
            var move = this.parentNode.parentNode.audio.getDuration()/155*left;
            this.parentNode.parentNode.audio.setSeekTime(move);
            fnGetElem(".timer",this.parentNode.parentNode).innerHTML=(Math.floor((move/1000)/60)<9?"0"+Math.floor((move/1000)/60):Math.floor((move/1000)/60))+":"+(Math.floor((move/1000)%60)<9?"0"+Math.floor((move/1000)%60):Math.floor((move/1000)%60));//NO I18N
            fnGetElem(".sliderThumb",this.parentNode.parentNode).style.left=((155/this.parentNode.parentNode.audio.getDuration()*move)-7)+"px";
            fnGetElem(".sliderBar",this.parentNode.parentNode).style.width=((155/this.parentNode.parentNode.audio.getDuration()*move)-7)+"px";
        }
    }
}
fnAudioSeekDown = function(e){
    fnAudioSeekDown.left = this.parentNode.offsetLeft;
    fnAudioSeekDown.currentLeft = e.clientX;
    fnAudioSeekDown.seekBar = this;
    fnAudioSeekDown.duration = this.parentNode.parentNode.parentNode.native?this.parentNode.parentNode.parentNode.audio.duration:this.parentNode.parentNode.parentNode.audio.getDuration()/1000;
    fnAudioSeekDown.paused = this.parentNode.parentNode.parentNode.native?this.parentNode.parentNode.parentNode.audio.paused:!this.parentNode.parentNode.parentNode.audio.isPlaying();
    this.parentNode.parentNode.parentNode.audio.pause();
    if(!this.parentNode.parentNode.parentNode.native){
        clearInterval(this.parentNode.parentNode.parentNode.timer);
    }
    document.addEventListener("mousemove",fnMoveSeekBar,false);
    document.addEventListener("mouseup",fnReleaseSeekBar,false);
}
fnMoveSeekBar = function(e){
    var left = e.clientX;
    var diffX = fnAudioSeekDown.currentLeft-left;
    var elemLeft = fnAudioSeekDown.seekBar.offsetLeft;
    var audio = fnAudioSeekDown.seekBar.parentNode.parentNode.parentNode;
    if(audio.audio.duration || audio.audio.getDuration()){
        var dur = audio.audio.duration || audio.audio.getDuration();
        if(elemLeft-diffX >= 0 && elemLeft-diffX <= 155){
            var toMove = elemLeft-diffX;
            var move = fnAudioSeekDown.duration/155*toMove;
            var mins = Math.floor(move/60);
            var secs = Math.round(move%60);
            fnGetElem(".timer",audio).innerHTML=(mins<10?("0"+mins):mins)+":"+(secs<10?("0"+secs):secs);//NO I18N
            fnAudioSeekDown.seekBar.style.left=(155/fnAudioSeekDown.duration*move)+"px";
            fnGetElem(".sliderBar",audio).style.width=(155/fnAudioSeekDown.duration*move)+"px";
            fnAudioSeekDown.curTime = move;
        }
    }
    fnAudioSeekDown.currentLeft=left;
}
fnReleaseSeekBar = function(e){
    var audio = fnAudioSeekDown.seekBar.parentNode.parentNode.parentNode;
    if(audio.native){
        try{
            audio.audio.currentTime=fnAudioSeekDown.curTime;
        }catch(exp){
        }
        if(!fnAudioSeekDown.paused)audio.audio.play();
    }else{
        if(!fnAudioSeekDown.paused){
            audio.audio.play(Math.floor(fnAudioSeekDown.curTime)*1000);
            audio.timer = setInterval(function(){
            var move = audio.audio.getCurrentTime()/1000;
            var duration = audio.audio.getDuration()/1000;
            var mins = Math.floor(move/60);
            var secs = Math.round(move%60);
            fnGetElem(".timer",audio).innerHTML=(mins>9?mins:"0"+mins)+":"+(secs>9?secs:"0"+secs);//NO I18N
            fnGetElem(".sliderThumb",audio).style.left=(155/duration*move)+"px";
            fnGetElem(".sliderBar",audio).style.width=(155/duration*move)+"px";
        },100);

        }else{
            fnPausePlay.call(fnGetElem(".playBtn",audio));//NO I18N
            fnAudioSeekDown.seekBar.parentNode.parentNode.parentNode.audio.setSeekTime(fnAudioSeekDown.curTime*1000);
        }
    }
    document.removeEventListener("mousemove",fnMoveSeekBar,false);
    document.removeEventListener("mouseup",fnReleaseSeekBar,false);
}
fnChgVolume = function(e){
    if((e.srcElement?e.srcElement:e.target).className == "volumeThumb")return;
    var left = e.layerX;
    var vol = (left/50*100)/100;
    var audio = this.parentNode;
    if(this.parentNode.native)
        this.parentNode.audio.volume=vol;
    else{
        this.parentNode.audio.setVolume(vol.toFixed(1));
    }
    if(vol>0.1 && vol < .6){fnGetElem(".volumeContainer",audio).children[0].className="volumeIcon50"}else if(vol > 0.6){fnGetElem(".volumeContainer",audio).children[0].className="volumeIcon100"}else{fnGetElem(".volumeContainer",audio).children[0].className="volumeIcon0"};fnGetElem(".volumeThumb",audio).style.left=(45/1*vol)+"px";fnGetElem(".volumeSliderBar",audio).style.width=(45/1*vol)+"px";
}
fnMuteUnmute = function(){
    var vol;
    var audio = this.parentNode;
    if(audio.native){
        if(audio.audio.muted){
            vol=audio.audio.volume;
            audio.audio.muted = false;
        }else{
            vol=0;
            audio.audio.muted = true;
        }
    }else{
        var vol = this.parentNode.audio.getVolume(); 
        if(vol != 0){
            audio.volume = vol;
            vol=0
        }else{
            vol=audio.volume;
        }
        audio.audio.setVolume(vol);
    }
    if(vol>0.1 && vol < .6){fnGetElem(".volumeContainer",audio).children[0].className="volumeIcon50"}else if(vol > 0.6){fnGetElem(".volumeContainer",audio).children[0].className="volumeIcon100"}else{fnGetElem(".volumeContainer",audio).children[0].className="volumeIcon0"};fnGetElem(".volumeThumb",audio).style.left=(45/1*vol)+"px";fnGetElem(".volumeSliderBar",audio).style.width=(45/1*vol)+"px";
}
fnVolBtnDown = function(e){
    fnVolBtnDown.left = this.parentNode.offsetLeft;
    fnVolBtnDown.leftTot = fnVolBtnDown.left+45;
    fnVolBtnDown.currentLeft = e.clientX;
    fnVolBtnDown.volBtn = this;
    document.addEventListener("mousemove",fnMoveVolBtn,false);
    document.addEventListener("mouseup",fnReleaseVolBtn,false);
}
fnMoveVolBtn = function(e){
    var left = e.clientX;
    var elemLeft = fnVolBtnDown.volBtn.offsetLeft;
    var diffX = left-fnVolBtnDown.currentLeft;
    if(elemLeft+diffX >= 0 && elemLeft+diffX <= 45){
        var audio = fnVolBtnDown.volBtn.parentNode.parentNode;
        var toMove = elemLeft+diffX;
        var vol = (toMove/45*100)/100;
        if(audio.native){
            audio.audio.volume=vol;
        }else{
            audio.audio.setVolume(vol);
        }
        if(vol>=0.1 && vol < .6){fnGetElem(".volumeContainer",audio).children[0].className="volumeIcon50"}else if(vol >= 0.6){fnGetElem(".volumeContainer",audio).children[0].className="volumeIcon100"}else{fnGetElem(".volumeContainer",audio).children[0].className="volumeIcon0"};fnGetElem(".volumeThumb",audio).style.left=(45/1*vol)+"px";fnGetElem(".volumeSliderBar",audio).style.width=(45/1*vol)+"px";
    }
    fnVolBtnDown.currentLeft=left;
}
fnReleaseVolBtn = function(){
    document.removeEventListener("mousemove",fnMoveVolBtn,false);
    document.removeEventListener("mouseup",fnReleaseVolBtn,false);
}
fnSetupAudio = function(audio,autoplay,loop,playlist){
    var repeatBtn = fnGetElem("#repeat", audio);//NO I18N
    var audioTag = document.createElement("audio");
    if(audio.getAttribute('data-url') || audio.tagName.toLowerCase() == "audio"){
        audio.playList = false;
        audioTag.setAttribute("src",audio.getAttribute("data-url"));
        if(repeatBtn){
            repeatBtn.onclick = fnRepeatPlayList;
            if(loop === "true"){
                repeatBtn.className = "repeatIcon-on";
            }
        }
    }else{
        audioTag.setAttribute("src",audio.playList[audio.curPlayItem]);
        var plCont = fnGetElem(".playlistContainer", audio);//NO I18N
        if(playlist){
            plCont.innerHTML = "";
            var prevBtn = fnGetElem(".prevBtn", audio);//NO I18N
            var nextBtn = fnGetElem(".nextBtn", audio);//NO I18N
            fnGetElem("#shuffle", audio).onclick=fnShufflePlaylist;//NO I18N
            prevBtn.onclick = fnPlayPrev;
            nextBtn.onclick = fnPlayNext;
            repeatBtn.onclick = fnRepeatPlayList;
            audio.shuffle = false;
            audio.repeat = false;
            audio.stack = [];
            var playList = audio.playList;
            var funcAud = function(){
                var mins = Math.floor(this.duration / 60);
                var secs = Math.round(this.duration % 60);
                this.durationCont.innerHTML=(mins>9?mins:"0"+mins)+":"+(secs>9?secs:"0"+9)
            }
            for(var i = 0,item;item = playList[i];i++){
                var meta = document.createElement("audio");
                meta.setAttribute("src",item);
                meta.setAttribute("preload","meta");
                var itemCont = document.createElement("div");
                itemCont.setAttribute("data-item",i);
                itemCont.className = "playlist-audiotime-container"+(i == 0?" selected":"");
                var songName = document.createElement("div");
                songName.className = "playlist-audioname ";
                songName.innerHTML = decodeURIComponent(item.split("/").pop().replace(".mp3",""));
                var songDuration = document.createElement("div");
                songDuration.className = "playlist-audiotime";
                songDuration.innerHTML = "--:--";
                itemCont.appendChild(songName);
                itemCont.appendChild(songDuration);
                plCont.appendChild(itemCont);
                if(meta.canPlayType && (audioTag.canPlayType("audio/mpeg") == "maybe" || audioTag.canPlayType("audio/mpeg") == "probably")){
                    meta.durationCont=songDuration;
                    meta.addEventListener('loadedmetadata', funcAud, false);
                }
                //plCont.appendChild(itemCont);
                itemCont.onclick = fnPlayThis;
            }
            var hei = plCont.clientHeight;
            var contHei = audio.clientHeight;
            audio.parentNode.parentNode.style.height = hei+contHei+"px";
            fnGetElem(".playlistIcon", audio).onclick=fnShowHidePL;//NO I18N
        }else{
            fnGetElem(".selected", plCont).className = "playlist-audiotime-container";
            plCont.children[audio.curPlayItem].className = "playlist-audiotime-container selected";
        }
    }
    if(audio.playList){
        if(audio.stack.indexOf(audio.curPlayItem)==-1){
            audio.stack.push(audio.curPlayItem);
        }
    }
    audio.native=false;
    var seekbar = fnGetElem(".sliderThumb",audio);//NO I18N
    var slideBar = fnGetElem(".sliderBar",audio);//NO I18N
    if(audio.audio){
        if(audio.audio.pause){
            audio.audio.pause();
        }
        var btn = fnGetElem(".pauseBtn",audio);//NO I18N
        if(btn){
            btn.className="playBtn";
        }
        if(audio.native){
            audio.audio.currentTime=0;
        }else{
            clearInterval(audio.timer);
            fnGetElem(".timer",audio).innerHTML="00:00";//NO I18N
            seekbar.style.left="0px";
            slideBar.style.width="0px";
        }
    }
    var playBtn=fnGetElem(".playBtn",audio)||fnGetElem(".pauseBtn",audio);//NO I18N
    if(!playBtn)return;
    playBtn.onclick=fnPausePlay;
    fnGetElem(".timer",audio).innerHTML="00:00";//NO I18N
    if(audioTag.canPlayType && (audioTag.canPlayType("audio/mpeg") == "maybe" || audioTag.canPlayType("audio/mpeg") == "probably")){
        audio.native=true;
        audio.audio=audioTag;
        if(autoplay == "true"){
            playBtn.className="pauseBtn";
            audioTag.setAttribute("autoplay","true");
        //}else{
            //audioTag.setAttribute("preload","none");
           }
        if(loop == "true")
            audioTag.setAttribute("loop","true");
        if(audio.playList && !playlist){
            playBtn.click();
        }
        audioTag.addEventListener("timeupdate",function(){var secs = Math.floor(this.currentTime%60);fnGetElem(".timer",audio).innerHTML=(Math.floor(this.currentTime/60)<9?"0"+Math.floor(this.currentTime/60):Math.floor(this.currentTime/60))+":"+(secs>9?secs:"0"+secs);seekbar.style.left=(155/this.duration*this.currentTime)+"px";fnGetElem(".sliderBar",audio).style.width=(155/this.duration*this.currentTime)+"px";},false);
        audioTag.addEventListener('ended',function(){fnEndAudio(audio)},false);
        //audioTag.addEventListener('loadedmetadata', function(){fnGetElem(".timer",audio).innerHTML=Math.floor(this.duration/60)+"."+Math.floor(this.duration%60)}, false);

    }else{
         var flObj = FlashAudio.init(audio,audio.getAttribute("data-url")?audio.getAttribute("data-url"):audio.playList[audio.curPlayItem]);
         audio.audio =flObj;
         addEvent(flObj,"ended",function(){//NO I18N
                var aud = this.parentNode;
                clearInterval(aud.timer);
                fnEndAudio(aud);
                });
         if(autoplay == "true"){
            setTimeout(function(){playBtn.click()},600);
         }
         if(audio.playList && !playlist){
            setTimeout(function(){playBtn.click()},600);
         }
         audio.loop = loop;
    }
    var seeker = fnGetElem(".sliderBGCont",audio);//NO I18N
    seeker.onclick=fnChangeAudioPos;
    seekbar.onmousedown = fnAudioSeekDown;
    var volCont = fnGetElem(".volumeSliderContainer",audio);//NO I18N
    volCont.onclick=fnChgVolume;
    fnGetElem(".volumeThumb",audio).onmousedown = fnVolBtnDown;//NO I18N
    fnGetElem(".volumeContainer",audio).onclick = fnMuteUnmute;//NO I18N
}
function fnEndAudio(aud,prev){
    if(fnGetElem(".pauseBtn",aud)){
        //fnGetElem(".pauseBtn",aud).className="playBtn";
        fnPausePlay.call(fnGetElem(".pauseBtn",aud));//NO I18N
    }
    if(aud.playList){
        var repeat = fnGetElem("#repeat", aud).className;//NO I18N
        if(repeat === "repeatIcon-off"){
            if(aud.shuffle){
                if(aud.stack.length !== aud.playList.length){
                    var rand = Math.floor(Math.random()*aud.playList.length);
                    var t = -1;
                    while(t < 0){
                        if(aud.stack.indexOf(rand) == -1){
                            t = 0;
                            aud.stack.push(rand);
                        }else{
                            rand = Math.floor(Math.random()*aud.playList.length);
                            t = -1;
                        }
                    }
                    aud.curPlayItem = rand;
                    fnSetupAudio(aud,false,false);
                }
            }else{
                if(!prev && aud.curPlayItem<aud.playList.length-1){
                    aud.curPlayItem+=1;
                    fnSetupAudio(aud,false,false);
                }else if(prev && aud.curPlayItem > 0){
                    aud.curPlayItem-=1;
                    fnSetupAudio(aud,false,false);
                }
            }
        }else if(repeat === "repeatIcon-on"){
            if(aud.stack.length === aud.playList.length){
                aud.stack = [];
            }
            if(aud.shuffle){
                var rand = Math.floor(Math.random()*aud.playList.length);
                var t = -1;
                while(t < 0){
                    if(aud.stack.indexOf(rand) == -1){
                        t = 0;
                        aud.stack.push(rand);
                    }else{
                        rand = Math.floor(Math.random()*aud.playList.length);
                        t = -1;
                    }
                }
                aud.curPlayItem = rand;
            }else{
                if(!prev){
                    if(aud.curPlayItem<aud.playList.length-1){
                        aud.curPlayItem+=1;
                    }else{
                        aud.curPlayItem=0;
                    }
                }else{
                    if(aud.curPlayItem>0){
                        aud.curPlayItem-=1;
                    }else{
                        aud.curPlayItem=aud.playList.length-1;
                    }
                }
            }
            fnSetupAudio(aud,false,false);
        }else if(repeat === "repeatIcon-one"){
            if(!aud.native){
                aud.audio.setSeekTime(0);
            }
            setTimeout(function(){fnGetElem(".playBtn",aud).click()},600);
        }
    }else{
        if(!aud.native){
            if(aud.loop == true){
                aud.audio.setSeekTime(0);
                setTimeout(function(){fnGetElem(".playBtn",aud).click()},600);

            }
        }
    }
}
FlashAudio = {}
FlashAudio.init = function (domelement,src) {
    var faid = "fa"+(Math.ceil(Math.random()*11111111111111));//NO I18N
        var html = '<object id="'+faid+'" data="/swf/mp3player.swf" type="application/x-shockwave-flash" allowscriptaccess="sameDomain" allowfullscreen="true" width="1" height="1" ><param name="movie" value="mp3player.swf"></param><param name="flashvars" value="src='+src+'&faid='+faid+'"></param><param name="menu" value="false"></param><param name="align" value="middle"></param><param name="allowScriptAccess" value="sameDomain"></param><param name="quality" value="high"></param><param name="wmode" value="opaque"></param></object>'
        var docfrag = document.createElement('div');
    docfrag.innerHTML = html;
    domelement.appendChild(docfrag);
    var flashobject = docfrag.firstChild;
    domelement.insertBefore(flashobject,docfrag);
    domelement.removeChild(docfrag);
    domelement.setAttribute("data-faid",faid); 
    return flashobject;
}
FlashAudio.postMessage = function(arg_message) {
    var message = JSON.parse(arg_message);
    switch (message.cmd) {
        case "event"://NO I18N
            var flashobject = document.getElementById(message.faid);
            fireEvent.call(flashobject, message.type);
            break;
        case "log"://NO I18N
            
            break;
    }
}
function fireEvent(evnt){
    if (document.createEventObject){
        var evt = document.createEventObject();
        return this.fireEvent('on'+evnt,evt);//NO I18N
    } else {
        var evt = document.createEvent("HTMLEvents");//NO I18N
        evt.initEvent(evnt, true, true );
        return !this.dispatchEvent(evt);
    }
}
function addEvent(obj, evnt, func) {
    if (obj.addEventListener) { // W3C DOM
        obj.addEventListener(evnt,func,false);
    } else if (obj.attachEvent) { // IE DOM
        obj.attachEvent("on"+evnt, func);
    } else { // No much to do
        obj[evnt] = func;
    }
}
function fnGetElem(elem,cont){
    var children = cont.childNodes;
    var nodes = [];
    for(var j=0;j<children.length;j++){
        getAllChild(children[j],nodes);
    }
    for(var i=0,child;child = nodes[i];i++){
        if(elem[0] == "." && (","+child.className.split(" ").join(",")+",").indexOf(","+elem.substring(1,elem.length)+",")>-1){
            return child;
        }else if(elem[0] == "#" && child.id.indexOf(elem.substring(1,elem.length))>-1){
            return child;
        }
    }
}
function getAllChild(elem,arr){
    if(elem.nodeType == 1){
        arr.push(elem);
    }
    if(elem.childNodes){
        for(var k=0;k<elem.childNodes.length;k++){
            getAllChild(elem.childNodes[k],arr);
        }
    }
}
