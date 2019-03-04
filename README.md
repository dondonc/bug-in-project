# bug-in-project
日常项目遇到的bug，以及修复方法

### iOS 12的input框bug
iOS 12的input框输入完成后页面不会滚回正常位置，可以通过监听input失焦时让页面滚动回顶部解决。
```javascript
$('input').focusout(function(){
    $(window).scrollTop(0)
})
```
上面的问题在只有1个input框的时候能解决问题，当页面出现复数个input框时，会发现在不收起软键盘的情况下点击下一个input框时，页面不会定位到当前input的位置，而是直接滚动到顶部了。原因就是因为上面做了失焦时的处理导致的，解决方法如下
```javascript
let curTop = 0//用来记录保存滚动高度
$('input').focusin(function(){
    $(window).scrollTop(curTop)
})
$('input').focusout(function(){
    curTop = $(window).scrollTop()
    $(window).scrollTop(0)
})
```