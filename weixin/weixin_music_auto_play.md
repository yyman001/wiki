###微信音乐自动播放
大部分iPhone都能正常播放,少部分奇葩不行.

```css
/*css*/
.music-box { position: absolute; top: 1.1rem; right: 0; z-index: 9; margin: .15rem; width: .62rem; height: .62rem; }
.music-box .music-btn { display: block; width: 100%; height: 100%; background-image: url(data:image/png;base64,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); background-position: 0 0; background-size: 200% 100%; }
.music-box .music-btn.close { -webkit-animation-play-state: paused; animation-play-state: paused; background-position: -100% 0; }
.music-rotate-ani { -webkit-animation: music-rotate-ani 3s linear infinite normal; animation: music-rotate-ani 3s linear infinite normal; }

@-webkit-keyframes music-rotate-ani { 0% { -webkit-transform: rotate(0deg); transform: rotate(0deg); }
  100% { -webkit-transform: rotate(360deg); transform: rotate(360deg); } }
@keyframes music-rotate-ani { 0% { -webkit-transform: rotate(0deg); transform: rotate(0deg); }
  100% { -webkit-transform: rotate(360deg); transform: rotate(360deg); } }

/*sass源码*/
.music-box {
  position: absolute;
  top: 1.1rem;
  right: 0;
  z-index: 9;
  margin: .15rem;
  width: .62rem;
  height: .62rem;
  .music-btn {
	display: block;
	width: 100%;
	height: 100%;
	//background: url(../images/music-on.png) no-repeat 50%;
	//background-size: contain;
	background-image: url(data:image/png;base64,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);
	background-position: 0 0;
	background-size: 200% 100%;
	&.close {
	  -webkit-animation-play-state: paused;
	  animation-play-state: paused;
	  background-position:-100% 0;
	}
  }
}

//音乐图标转动
.music-rotate-ani{
  animation: music-rotate-ani 3s linear infinite normal;
}
@keyframes music-rotate-ani{
  0% {
	transform: rotate(0deg);
  }
  100%{
	transform: rotate(360deg);
  }
}
```

```html
<!--音乐按钮-->
<div class="music-box">
    <a id="musicBtn" class="music-btn music-rotate-ani close"></a>
</div>
<!-- 如果页面中有其他音效,可能会无法播放 -->
<audio id="Jaudio" class="media-audio" src="/media/bg_C32kbps.mp3" preload loop="loop" style="position: absolute;left: -9999px;opacity: 0;"></audio >

```

```javascript
//依赖微信js
<script src="http://res.wx.qq.com/open/js/jweixin-1.0.0.js"></script>
<script src="http://h5.static.myappgame.com/common/WeixinApi.js"></script>

//////////背景音

var musicIco = document.getElementById('musicBtn');

	function audioAutoPlay(id){
		var audio = document.getElementById(id);
		audio.play();
		setTimeout(function(){
			setMusicIco();
		},500);
		document.addEventListener("WeixinJSBridgeReady", function () {
			audio.play();
			setTimeout(function(){
				setMusicIco();
			},500);
		}, false);
	}
	audioAutoPlay('Jaudio');

	function playMusic(target){
		var hasClass = target.className;
		if(hasClass.indexOf('close') > -1){
			target.className = 'music-btn music-rotate-ani';
			document.getElementById('Jaudio').play();
		}else{
			target.className = 'music-btn music-rotate-ani close';
			document.getElementById('Jaudio').pause();
		}
	}

	function setMusicIco(){
		if(!document.getElementById('Jaudio').paused){
			musicIco.className = 'music-btn music-rotate-ani';
		}else{
			musicIco.className = 'music-btn music-rotate-ani close';
		}
	}

	//音乐按钮点击
	musicIco.addEventListener('touchstart',function(e){
		playMusic(this);
		return false;
	},false);

//////////
```



