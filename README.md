# zoom_demo
zoom demo

##  demo  

[click here to view demo](https://fuxingzhang.github.io/zoom_demo/index.html)

## description  

* Each div can be dragged, hold down the left mouse button and let go, slide up and down, left and right. Release the left mouse button to end
* 4 sides and 4 corners of each div can be scaled, the mouse arrow changes shape to indicate selection, hold down the left mouse button and  let go, slide up and down, left and right. Release the left mouse button to end

* 每个div可以拖拽，按住鼠标左键不放手，上下左右滑动，松开鼠标左键结束    
* 每个div的4个边和4个角可以缩放，鼠标箭头变形状表示选中，按住鼠标左键不放手，上下左右滑动，松开鼠标左键结束  

## screenShots  

![1](https://github.com/fuxingZhang/zoom_demo/blob/master/screenshots/1.jpg)   
![2](https://github.com/fuxingZhang/zoom_demo/blob/master/screenshots/2.jpg)   

## code  

```  
<!DOCTYPE html>
<html lang="zh-cn">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>cursor</title>
  <style>
    * {
      margin: 0;
      border: 0;
    }

    .move {
      background: #ccc;
      position: absolute;
      cursor: move;
      font-size: 30px;
      text-align: center;
      vertical-align: middle;
      color: black;
    }

    .top,
    .left,
    .right,
    .bottom {
      position: absolute;
      background-color: darkorchid;
    }

    .left {
      left: 0;

    }

    .right {
      right: 0;

    }

    .left,
    .right {
      top: 8px;
      bottom: 8px;
      width: 8px;
      cursor: e-resize;
    }

    .top {
      top: 0;
    }

    .bottom {
      bottom: 0;
    }

    .bottom,
    .top {
      left: 8px;
      right: 8px;
      height: 8px;
      cursor: n-resize;
    }

    .lt,
    .lb,
    .rt,
    .rb {
      width: 8px;
      height: 8px;
      position: absolute;
      background-color: red;
    }

    .lt {
      left: 0;
      top: 0;
      cursor: se-resize;
    }

    .lb {
      left: 0;
      bottom: 0;
      cursor: sw-resize;
    }

    .rt {
      top: 0;
      right: 0;
      cursor: sw-resize;
    }

    .rb {
      right: 0;
      bottom: 0;
      cursor: se-resize;
    }
  </style>
</head>

<body>
</body>

<script>
  window.onload = function () {
    // 兼容手机
    let eventStart, eventMove, eventEnd, getX, getY, isPc = false, base;

    if ((navigator.userAgent.match(/(iPhone|iPod|Android|ios|iOS|iPad|Backerry|WebOS|Symbian|Windows Phone|Phone)/i))) {
      eventStart = 'touchstart';
      eventMove = 'touchmove';
      eventEnd = 'touchend';
      getX = e => e.changedTouches[0].clientX;    // targetTouches, touches
      getY = e => e.changedTouches[0].clientY;
      base = '100px';
    } else {
      eventStart = 'mousedown';
      eventMove = 'mousemove';
      eventEnd = 'mouseup';
      getX = e => e.clientX;
      getY = e => e.clientY;
      isPc = true;
      base = '200px';
    }

    // 随机生成5个可拖拽的div
    generateRandomDiv(5);

    let left, top, width, height, x, y, target, action;



    document.addEventListener(eventStart, e => {
      // document.addEventListener(eventMove, handlePosition, { passive: false });
      document.addEventListener(eventMove, handlePosition);
      handleStart(e);
    })

    document.addEventListener(eventEnd, e => {
      document.removeEventListener(eventMove, handlePosition);
      handlePosition(e);
      action = '';
    })

    // 防止mouseup丢失
    function preventDefault(e) {
      if (e.stopPropagation) e.stopPropagation();
      if (e.preventDefault) e.preventDefault();
      e.cancelBubble = true;
      e.returnValue = false;
      return false;
    }

    function handleStart(e) {
      if (e.target.className == 'move') {
        target = e.target;
      } else if (e.target.parentNode.className == 'move') {
        target = e.target.parentNode;
      } else {
        return
      }
      action = e.target.className;
      x = getX(e);
      y = getY(e);
      left = target.offsetLeft;
      top = target.offsetTop;
      width = target.offsetWidth;
      height = target.offsetHeight;
    }

    function handlePosition(e) {
      if (isPc) preventDefault(e);

      switch (action) {
        case 'move':
          target.style.left = left + getX(e) - x + 'px';
          target.style.top = top + getY(e) - y + 'px';
          break;
        case 'left':
          target.style.left = left + getX(e) - x + 'px';
          target.style.width = width - getX(e) + x + 'px';
          break;
        case 'right':
          target.style.width = width + getX(e) - x + 'px';
          break;
        case 'top':
          target.style.top = top + getY(e) - y + 'px';
          target.style.height = height - getY(e) + y + 'px';
          break;
        case 'bottom':
          target.style.height = height + getY(e) - y + 'px';
          break;
        case 'lt':
          target.style.left = left + getX(e) - x + 'px';
          target.style.width = width - getX(e) + x + 'px';
          target.style.top = top + getY(e) - y + 'px';
          target.style.height = height - getY(e) + y + 'px';
          break;
        case 'lb':
          target.style.left = left + getX(e) - x + 'px';
          target.style.width = width - getX(e) + x + 'px';
          target.style.height = height + getY(e) - y + 'px';
          break;
        case 'rt':
          target.style.top = top + getY(e) - y + 'px';
          target.style.height = height - getY(e) + y + 'px';
          target.style.width = width + getX(e) - x + 'px';
          break;
        case 'rb':
          target.style.width = width + getX(e) - x + 'px';
          target.style.height = height + getY(e) - y + 'px';
          break;
      }
    }

    function generateRandomDiv(n) {
      const height = document.documentElement.clientHeight - 200;
      const width = document.documentElement.clientWidth - 200;

      let generateOneDiv = n => {
        const left = parseInt(Math.random() * width) + 'px';
        const top = parseInt(Math.random() * height) + 'px';

        return `
          <div class="move" style="left:${left};top:${top};width:${base};height:${base};line-height:${base};">
            <div class="left"></div>
            <div class="right"></div>
            <div class="top"></div>
            <div class="bottom"></div>
            <div class="lt"></div>
            <div class="lb"></div>
            <div class="rt"></div>
            <div class="rb"></div>
            ${n}
          </div>
        `
      }

      let html = '';

      for (let i = 0; i < n; i++) {
        html += generateOneDiv(i + 1);
      }

      document.body.innerHTML = html;
    }
  }
</script>

</html>
```  


