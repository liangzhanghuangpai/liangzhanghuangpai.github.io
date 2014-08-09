---
layout: page
title: 
tagline: 
---
{% include JB/setup %}
    
<div id="posts">
  <script type="text/javascript">
    var data = {};
  </script>
  {% for post in site.posts %}
  {% assign item = site.data.games[post.title] %}
  <div class="post">
    <div class="clearfix">
      <a href="{{ BASE_PATH }} {{ post.url }}"> {{ post.title }} </a>
      <button class="btn btn-mini btn-success" data-id="{{ item.pid }}">分享</button>
    </div>

    <div class="msg-clone">
      <img src="{{item.imgUrl}}" width="50" height="50">
      <a href="{{ BASE_PATH }} {{ post.url }}"> {{ item.score_prefix }}{{ item.score }}{{ item.score_suffix }} </a>    
    </div>
    <script type="text/javascript">
      data["{{ item.pid }}"] = {
        "appId": "{{ item.appId }}",
        "imgUrl": "{{ item.imgUrl }}",
        "url": "{{ item.url }}",
        "score_prefix": "{{ item.score_prefix }}",
        "score_suffix": "{{ item.score_suffix }}",
        "score": "{{ item.score }}"
      };
    </script>
  </div>
  {% endfor %}

  <script type="text/javascript">
    var selected_id;

    document.addEventListener('WeixinJSBridgeReady', function onBridgeReady() {
      WeixinJSBridge.on('menu:share:appmessage', function(argv) {
        var item = data[selected_id]
          , title = item.score_prefix + item.score + item.score_suffix;
        document.title = title;
        WeixinJSBridge.invoke('sendAppMessage', {
          "appId": item.appId,
          "img_url": item.imgUrl,
          "link": item.url,
          "desc": "",
          "title": title
        });
      });

      WeixinJSBridge.on('menu:share:timeline', function(argv) {
        var item = data[selected_id]
          , title = item.score_prefix + item.score + item.score_suffix;
        WeixinJSBridge.invoke('shareTimeline', {
          "appid": item.appId,
          "img_url": item.imgUrl,
          "img_width": "640",
          "img_height": "640",
          "link": item.url,
          "desc": "",
          "title": title
        });
      });
    }, false);

    var buttons = document.getElementsByClassName("btn-success");
    for (var i = 0, j=buttons.length; i<j; i++) {
      buttons[i].addEventListener("click", function(e){
        selected_id = e.target.getAttribute("data-id");
      });
    }
  </script>
</div>
