---
layout: post
title:  Capybara drag and drop summary
categories: automation
---


最近的项目用到了drag and drop， 记载一些

origin_ele = find(...)
target_ele = find(...)

- 1.  drag_to
origin_ele.drag_to target_ele
最简单的一种了，把一个元素拽到另一个元素
缺点是，如果浏览器不能同时显示两个元素，那么有可能失败

- 2.  drag_and_drop_by
page.driver.browser.action.drag_and_drop_by(origin_ele.native,200,-800).perform
把一个元素按照像素位置拖动，后面两个像素的参数， 第一个是x，表示该元素横向拖动多少，如果200就是该元素向右拖动200，如果是负数就是向左拖动。
第二个是y，表示该元素纵向拖动多少，如果200就是该元素向下拖动200，如果是负数就是向上拖动。
所以现在这个表示把origin_ele向左上拖动


- 3.  move_to
上面两个都不管用的时候，可以考虑直接移动鼠标来达到目的
page.driver.browser.mouse.down(origin_ele.native)
page.driver.browser.mouse.move_to(target_ele.native, x, y)
page.driver.browser.mouse.up
鼠标按下-拖动-抬起，其中的x,y和上面的意思一致

- 4.  javascript
page.execute_script %{
   $.getScript("http://your.bucket.s3.amazonaws.com/jquery.simulate.drag-sortable.js", function() {
   $("#ele_id").simulateDragSortable({ move: 100});
  });
}
使用js来移动目标，没有用过，其中move的似乎是上下移动，同样正数向下，负数向上，暂不知道怎样左右移动

关于怎么解决页面太长导致无法准确拖拽的问题，没有找到什么好方法，不过可以考虑在拖拽之前将页面定位到target ele的位置
origin_ele.native.send_keys :page_up


