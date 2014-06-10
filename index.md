# Web Animations

[@GeckoTang](http://twitter.com/GeckoTang)

---

## Web Animationsという仕様

- [Web Animations 1.0](http://dev.w3.org/fxtf/web-animations/)
- CSS AnimationsをJavaScript側で定義し、実行するための仕様。
- 途中でキャンセルしたり、アニメーション終了のイベントも取れる。
- Chrome 36から実装される。  
2014/06/10時点だと、Canaryで動く
- Chrome 36以外でも使いたい場合は[Polyfill](https://github.com/web-animations/web-animations-js)を使う。

---

## とりあえず使う

こんな感じ。

[fiddle](http://jsfiddle.net/68J2J/1/)  

```javascript
var block = document.querySelector('.block');
block.animate([
    { left: "0" },
    { left: "100px" }
], 1500);
```

```html
<div class="block"></div>
```

```css
.block {
    position: relative;
    width: 100px; height: 100px;
    background: tomato;
}
```

jQueryライクで、非常に簡単である。

---

### element.animate(keyframe, option)

animateメソッドの引数について

------

## 第一引数

第一引数には、@keyframes的な感じの配列を指定する。

```javascript
element.animate([
  {cssProperty: value},
  {
    cssProperty: value,
    cssProperty: value
  },
  {cssProperty: value}
], option);
```

------

## 第二引数

第二引数に**数値**を指定した場合は、animation-durationの値になる。

```javascript
element.animate([
  {cssProperty: value},
  {cssProperty: value},
  {cssProperty: value}
], 1000);
```

より詳細に指定するためには、第二引数にオブジェクトを指定する

```javascript
element.animate([
  {cssProperty: value},
  {cssProperty: value},
  {cssProperty: value}
], {
    duration: 3000,
    delay: 1000,
    iterations: 3,
    direction: "alternate",
    easing: "steps(10)",
    fill: "none"
});
```

------

オプションでは、以下を指定できる。

- duration
- delay
- endDelay
- fill
- iterations
- iterationStart
- duration
- playbackRate
- direction
- easing

お気付きの通り、大体はCSS Animationのものを指定できる。  
詳しくは、[5.4.1 Attributes](http://dev.w3.org/fxtf/web-animations/#attributes-3)を参考にしてね。

---

## AnimationPlayer

``element.animate()``は、``AnimationPlayer``オブジェクトを返す。
それを使えばアニメーションを止めたり終了を検知したりができる。

---

### アニメーションの終了を検知する

3秒後にアラートされる。

[fiddle](http://jsfiddle.net/68J2J/2/)  

```javascript
var block = document.querySelector('.block');
var player = block.animate([
    { left: "0" },
    { left: "100px" }
], {
    duration: 3000,
    easing: "ease-in"
});
player.onfinish = function(){
    alert('done!');
}
```

------

ちなみにonfinishの部分は

```javascript
player.addEventListener('finish', function(){
    alert('done!');
}, false);
```

addEventListenerでもよい

---

### アニメーションを停止、終了、再生、一時停止、反転させる

仕様上は、

- player.cancel()
- player.play()
- player.pause()
- player.finish()
- player.reverse()

これだけできるはずなのですが、2014/06/10時点のCanaryではcancelしかできない。

---

### とりあえず止めてみた

キャンセルボタンをクリックするとアニメーションが終了する。  

[fiddle](http://jsfiddle.net/68J2J/3/)  

```html
<div class="block"></div>
<button id="animate">アニメーションを作成する</button>
<button id="cancel">player.cancel()</button>
```

```javascript
var block, player;
document.querySelector('#animate').addEventListener('click', function(){
    block = document.querySelector('.block');
    player = block.animate([
        { left: "0" },
        { left: "100px" }
    ], {
        duration: 3000,
        easing: "ease-in"
    });
});
document.querySelector('#cancel').addEventListener('click', function(){
    player.cancel();
});
```

cancelすると、アニメーション完了後の状態になる

------

cancelした時にも、onfinishが呼ばれる。  

[fiddle](http://jsfiddle.net/68J2J/5/)

```javascript
var block, player;
document.querySelector('#animate').addEventListener('click', function(){
    block = document.querySelector('.block');
    player = block.animate([
        { left: "0" },
        { left: "100px" }
    ], {
        duration: 3000,
        iterations: 1,
        easing: "ease-in",
        fill: "forwards"
    });
    player.addEventListener('finish', function(){
        alert('done!');
    }, false);
});

document.querySelector('#cancel').addEventListener('click', function(){
    player.cancel();
});
```

---

## 色々やってみたリンク

- []()
- []()
- []()
- []()
- []()

---

## 参考リンク

- [Web Animations 1.0](http://dev.w3.org/fxtf/web-animations/)
- [Web Animations - element.animate() is now in Chrome 36](http://updates.html5rocks.com/2014/05/Web-Animations---element-animate-is-now-in-Chrome-36)
- [Coming to a Screen Near You: CSS3 Animations and The New JavaScript Method Animate()](http://www.noupe.com/javascript/coming-to-a-screen-near-you-css3-animations-and-the-new-javascript-method-animate-82409.html)
- [Polyfill](https://github.com/web-animations/web-animations-js)

---

## ご清聴ありがとうございました。
