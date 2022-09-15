---
title: touch-and-click-event
date: 2022-09-14 09:38:48
tags:
categories:
---

製作完一個 swipe 專案之後發覺 click 事件會同時觸發 touch 事件，造成綁定同一個 dom 時觸發兩次，解決方式有幾種：

# 針對 click 與 touch 綁定不同的 dom
例如輪播可以針對箭頭按鈕綁定 click，輪播對象綁定 touch。

# 改用 mousedown & up 取代 click 事件
可以區分滑鼠的點擊事件使用 mouse 系列，與手指的點擊使用 touch 事件。

# 改用 pointer event
該事件可以適用於所有觸控裝置，包括觸控筆與手指，可以更敏銳的偵測觸控的方式，有幾種形式可以使用：

### pointerover
Fired when a pointer is moved into an element's hit test boundaries.
當觸控進入指定區域時觸發。

### ponterenter
Fired when a pointer is moved into the hit test boundaries of an element or one of its descendants,
including as a result of a pointerdown event from a device that does not support hover
當觸控進入指定區域或是子元素區域，及不支援 hover 的裝置上按壓時觸發．

### pointerdown
Fired when a pointer becomes active buttons state.
按壓時觸發．

### pointermove
Fired when a pointer changes coordinates.
This event is also used if the change in pointer state can not be reported by other events.
當手指按壓並移動時觸發，亦會使用在 pointer 狀態不為其他事件所察覺時．

### pointerup
Fired when a pointer is no longer active buttons state.
當手指接觸面積離開裝置時觸發．

### pointercancel
A browser fires this event if it concludes the pointer will no longer be able to generate events
(for example the related device is deactivated).
當裝置因設置關閉觸控等原因無法觸發 pointer 事件時，瀏覽器會偵測並觸發．