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
// 方法1(废弃，但原理相同)
let curTop = 0//用来记录保存滚动高度
$('input').focusin(function(){
    $(window).scrollTop(curTop)
})
$('input').focusout(function(){
    curTop = $(window).scrollTop()
    $(window).scrollTop(0)
})

// 方法2
var flag;
var myFunction;
document.body.addEventListener('focusin', () => {  //软键盘弹起事件
    flag=true;
    clearTimeout(myFunction);
})
document.body.addEventListener('focusout', () => { //软键盘关闭事件
    flag=false;
    if(!flag){
        myFunction = setTimeout(function(){
            window.scrollTo({top:0,left:0,behavior:"smooth"})//重点  =======当键盘收起的时候让页面回到原始位置(这里的top可以根据你们个人的需求改变，并不一定要回到页面顶部)

        },200);
    }else{
        return
    }
})
```