---
title: 【学习笔记】之jQuery
tags: [学习笔记, jQuery]
categories: [学习笔记]
---
### 点击选中
  ``` js
  $(function() {
    $(".eui-btn a").click(function() {
      if ($(this).hasClass("active")) {
        $(this).removeClass("active");
      } else {
        $(this).addClass("active");
      }
    });
  });
  ```
### 滚动更换头部
  ``` js
  $(window).on('scroll', function() {
    if ($(window).scrollTop() > 30) {
      $('.eui-header').addClass('on');
    } else {
      $('.eui-header').removeClass('on');
    }
  });
  ```
### 滚动视觉差
  ``` js
  $("#goTop").click(function() {
    $('html, body').animate({scrollTop: 0}, 300);
    return false;
  })
  ```
### 手风琴切换js
  ``` js
  $(function() {
    $(".am-menu-nav li").click(function() {
      if ($(this).children("div").is(":hidden")) {
        $(this).children("div").stop(false,true).slideDown();
        $(this).siblings("li").children("div").stop(false, true).slideUp();
      } else {
        $(this).children("div").stop(false, true).slideUp();
      }
    });
  });
  ```
### 点击右侧导航页面平滑滚动至相应位置
  ``` html
  <div id="menu"class="smart-city-nav">
    <a href="#item1"><span>智慧城市</span></a>
    <a href="#item2"><span>智慧应急</span></a>
    <a href="#item3"><span>智慧政务</span></a>
    <a href="#item4"><span>智慧云</span></a>
    <a href="#item5"><span>城市运营</span></a>
    <a href="#item6"><span>云服务</span></a>
  </div>
  <div class="eui-main">
    <div class="item" id="item1"></div>
    <div class="item" id="item2"></div>
    <div class="item" id="item3"></div>
    <div class="item" id="item4"></div>
    <div class="item" id="item5"></div>
    <div class="item" id="item6"></div>
  </div>
  ```
  ``` js
  $(document).ready(function() {
    $(window).scroll(function() {
      var top = $(document).scrollTop(); // 定义变量，获取滚动条的高度
      var menu = $("#menu"); // 定义变量，获取侧边导航id
      var items = $(".eui-main").find(".item"); // 定义变量，查找.item
      varcurId="";
      vartopVal = $(this).scrollTop();
      if (topVal > 200) {
        menu.fadeIn(150);
      } else {
        menu.fadeOut(150);
      }
      // 定义变量，当前所在的楼层item #id 
      items.each(function() {
        var m = $(this); // 定义变量，获取当前类
        varitemsTop = m.offset().top; // 定义变量，获取当前类的top偏移量
        if (top > itemsTop-200) {
          curId ="#" + m.attr("id");
        } else {
          return false;
        }
      });
      //给相应的楼层设置active,取消其他楼层的active
      varcurLink = menu.find(".active");
        if (curId && curLink.attr("href") != curId) {
          curLink.removeClass("active");
          menu.find("[href=" + curId + "]").addClass("active");
        }
        // console.log(top);
    });
    $("#menu a").click(function() {
      varhref = $(this).attr("href");
      var pos = $(href).offset().top;
      $("html,body").animate({scrollTop: pos},500);
      return false;
    });
  });
  ```
### jQuery打开页面数字数值滚动代码
  ``` html
  <script type="text/javascript"src="js/jquery.min.js"></script>
  <script type="text/javascript"src="js/numroll.js"></script>
  ....
  <span class="timer" data-to="300000" data-speed="2000">300000</span>
  ```
  ``` js
  $('.timer').each(count)
  ```
### H5视频播放暂停
  ``` js
  varmyVideo = document.getElementById("eveVideo");
  $(".play-pause").click(function() {
    if (myVideo.paused) {
      myVideo.play();
      $(this).removeClass("on");  
    } else {
      myVideo.pause();
      $(this).addClass("on");
    }
  });
  ```
### 鼠标经过显示span文字为title
  ``` js
  $(".eui-activity-btn a").hover(function() {
    var tit = $(this).find("span").text();
    $(this).attr("title", tit);
  });
  ```
### 鼠标经过淡入淡出显示隐藏效果
  ``` js
  $(".show-hide").hover(function() {
    $(this).find(".sub").fadeIn(150);
  }, function() {
    $(this).find(".sub").fadeOut(150);
  });
  ```
### 百度分享
  ``` html
  <div class="bdsharebuttonbox"><a href="#" class="bds_more" data-cmd="more">分享</a></div>
  ```
  ``` js
  window._bd_share_config = {
    "common": {
      "bdSnsKey": {},
      "bdText": "",
      "bdMini":"2",
      "bdMiniList": false,
      "bdPic": "",
      "bdStyle": "1",
      "bdSize": "24"
    }, "share": {}
  };
  with(document)0[(getElementsByTagName('head')[0]||body).appendChild(createElement('script')).src='http://bdimg.share.baidu.com/static/api/js/share.js?v=89860593.js?cdnversion='+~(-newDate()/36e5)];
  ```