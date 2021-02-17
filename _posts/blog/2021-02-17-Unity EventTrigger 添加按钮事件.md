---
layout:     post
title:     Unity EventTrigger 添加按钮事件
description:     Unity EventTrigger 添加按钮事件
date:     2021-02-17
author:     POt
header-img:     img/post-bg-kuaidi.jpg
catalog: true
category: blog
tags:     
    -   
        -   

    -   
---  

# Unity EventTrigger 添加按钮事件

- 在Start()函数创建UnityAction，在OnButCreateDefenderDown函数种相应按钮按下时发生的事件

  ```
  UnityAction<BaseEventData> downAction = new UnityAction<BaseEventData>(OnButCreateDefenderDown);
  UnityAction<BaseEventData> upAction = new UnityAction<BaseEventData>(OnButCreateDefenderUp);
  ```

- 添加按下 "创建防守单位按钮"

  ```
  void OnButCreateDefenderDown(BaseEventData data)
      {
          m_isSeletedButton = true;
      }
  ```

- 添加抬起"创建防守单位按钮"

  ```
  void OnButCreateDenferUp(BaseEventData data)
      {
          GameObject go = data.selectedObject;
      }
  ```

- 添加EventTrigger动态监听“按下”事件

  ```
  EventTrigger.Entry down = new EventTrigger.Entry();//按钮按下事件
  down.eventID = EventTriggerType.PointerDown;
  down.callback.AddListener(downAction);
  ```

- 添加EventTrigger动态监听“抬起”事件

  ```
  EventTrigger.Entry up = new EventTrigger.Entry();
  up.eventID = EventTriggerType.PointerUp;
  up.callback.AddListener(upAction);
  ```

- 在实际情况中给Transform添加按钮事件

  ```
  if (t.name.Contains("but_player"))
  {
       EventTrigger trigger = t.gameObject.GetComponent<EventTrigger>();
       trigger.triggers = new List<EventTrigger.Entry>();
       trigger.triggers.Add(down);
       trigger.triggers.Add(up);
  }
  ```

