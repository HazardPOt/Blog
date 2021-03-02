---
layout:     post
title:     Unity PHP应用 分数排行榜
description:     Unity PHP应用 分数排行榜
date:     2021-03-02
author:     POt
header-img:     img/post-bg-kuaidi.jpg
catalog: true
category: blog
tags:     
    -   
        -   

    -   

---

## 创建数据库

  首先在MySQL中建立一个简单的数据库

- 启动My SQL workbench 连接数据库

- 在导航窗口右键，【Create Schema】创建一个新的数据库

- 选择【Create Table】创建一个数据表名字叫hiscore，添加三条数据

  [![6ibXCt.png](https://s3.ax1x.com/2021/03/01/6ibXCt.png)](https://imgtu.com/i/6ibXCt)

## 创建PHP脚本

  我们要创建两个PHP脚本，一个用来上传用户名和分数，另一个用来下载分数并将其排序后发送给Unity

### 创建UploadSocore.php

  这里读入来自Unity的Username和Score数据，然后打开数据库，使用SQL语句将数据插入到数据库中。

```
<?php
//连接数据库
$myHandler=mysqli_connect("localhost","root","Woshitudou0205","myscoresdb");
if(mysqli_connect_errno())//mysqli_connect_errno() 返回最近调用函数的最后一个错误代码：
{
    echo mysqli_connect_error();
    die();
    exit(0);
}

//保证编码为UTF8
mysqli_query($myHandler,"set names utf8");

//读入Unity传来的用户名和分数，使用mysql_real_escape_string校验用户名的合法性（防止SQL注入）
$UserID=mysqli_real_escape_string($myHandler,$_POST["name"]);
$hiscore=$_POST["score"];

//向数据库插入新的数据
$sql="insert into hiscores value (NULL,'$UserID','$hiscore')";//insert into 插入新的行
mysqli_query($myHandler,$sql) ;//or die("SQL ERROR : ".$requestSQL);

//关闭数据库
mysqli_close($myHandler);

//将结果发给Unity
echo 'upload'.$UserID.":".$hiscore;
?>
```

### 创建DownloadSocore.php

  在数据库中查询分数最高的20个记录，然后在一个循环语句中将用户名和得分保存在数组中，最后将其序列化为JSON字符串发送给Unity，代码如下。

```
<?php
$myHandler=mysqli_connect("localhost","root","Woshitudou0205","myscoresdb");
if(mysqli_connect_errno())
{
    echo mysqli_connect_error();
    die();
    exit(0);
}
mysqli_query($myHandler,"ser names utf8");
$requestSQL="SELECT * FROM hiscores ORDER by score DESC LIMIT 20";//查询分数最高的20个
$result=mysqli_query($myHandler,$requestSQL) or die("SQL ERROR:".$requestSQL);
$num_result=mysqli_num_rows($result);

$arr = array();
for($i=0;$i<$num_result;$i++)
{
    $row=mysqli_fetch_array($result,MYSQLI_ASSOC);//获得一行数据

    $id=$row['id'];//获得ID
    $arr[$id]['id']=$row['id'];//将ID存入数组
    $arr[$id]['name']=$row['name'];//将用户名存入数组
    $arr[$id]['score']=$row['score'];//将分数存入数组

}
mysqli_free_result($result);//释放SQL查询结果
mysqli_close($myHandler);

//向Unity发送JSON格式的数据
echo json_encode($arr);
```

### 上传和下载分数

  在Unity中，可以通过简单的UI来实现这两个功能；一个是上传用户名和得分，另一个是下载排名前20的用户的得分

  创建脚本HiScoreApp.cs

-   添加UploadScore函数，使用POST方式

  ```
  IEnumerator UploadScore(string name, string score)
      {
          string url = "http://127.0.0.1/UploadScore.php";
          WWWForm form = new WWWForm();
          form.AddField("name", name);
          form.AddField("score", score);
  
          WWW www = new WWW(url, form);
          yield return www;
          if (www.error != null)
          {
              Debug.LogError(www.error);
              yield return null;
          }
          else
          {
              Debug.Log(www.text);
          }
      }
  ```

- 在上传分数按钮中执行UploadScore函数

          if (GUI.Button(m_uploadBut, "上传"))
          {
              StartCoroutine(UploadScore(m_name, m_score)); // HTTP请求
              m_name = "";  // 清空UI中的文本输入
              m_score = "";  // 清空UI中的文本输入
          }

- 定义UserData类，用来保存服务器返回的数据。添加DownloadScores函数，下载服务器返回的数据，<u>使用JSON将数据先反序列化为字典，再一一反序列化为用户数据。</u>

      IEnumerator DownloadScores(string name,string score)
      {
          string url = "http://127.0.0.1/DownloadScore.php";
          WWW www = new WWW(url);
          yield return www;
          if (www.error != null)
          {
              Debug.LogError(www.error);
              yield return null;
          }
          else
          {
              try
              {
                  //将PHP返回的数据解析为字典格式
                  var dict = MiniJSON.Json.Deserialize(www.text) as Dictionary<string, object>;
                  int index = 0;
                  foreach(object v in dict.Values)
                  {
                      UserData user = new UserData();
                      MiniJSON.Json.ToObject(user, v); //将字典中的值反序列化为UserData
                      m_hiscores[index] = string.Format("ID:{0:D2} 名字: {1}  分数{2}", user.id, user.name, user.score);
                      index++;
                  }
              }
              catch(System.Exception e)
              {
                  Debug.Log("无数据或者返回的数据不正确");
              }
          }
      }
  ```
  public class UserData
  {
      public int id;
      public string name;
      public int score;
  }
  ```

- 在下载分数按钮中执行DownloadScores函数

          if (GUI.Button(m_downLoadBut, "下载"))
          {
              StartCoroutine(DownloadScores(m_name, m_score));  // MySQL请求
          }

- 最后再用OnGUI实现UI，在实际项目中不推荐使用OnGUI创建UI

  ```
  private void OnGUI()
      {
          GUI.Label(m_nameLablePos, "用户名");
          GUI.Label(m_scoreLablePos, "得分");
          m_name = GUI.TextField(m_nameTxtField, m_name);  // 名字输入框
          m_score = GUI.TextField(m_scoreTxtField, m_score);  // 密码输入框
  
          GUI.skin.button.alignment = TextAnchor.MiddleCenter;  // 设置文字对齐
  
          // 上传分数按钮
          if (GUI.Button(m_uploadBut, "上传"))
          {
              StartCoroutine(UploadScore(m_name, m_score)); // HTTP请求
              m_name = "";  // 清空UI中的文本输入
              m_score = "";  // 清空UI中的文本输入
          }
  
          // 下载分数按钮
          if (GUI.Button(m_downLoadBut, "下载"))
          {
              StartCoroutine(DownloadScores(m_name, m_score));  // MySQL请求
          }
  
  
          GUI.skin.button.alignment = TextAnchor.MiddleLeft;  // 设置文字对齐
  
          m_scrollPos = GUI.BeginScrollView(m_scrollViewPosition, m_scrollPos, m_scrollView);
  
          m_gridPos.height = m_hiscores.Length * 30;
  
          // 显示分数排行榜
          GUI.SelectionGrid(m_gridPos, 0, m_hiscores, 1);
  
          GUI.EndScrollView();
      }
  ```

  

### 最后，实现效果

[![6kccb6.png](https://s3.ax1x.com/2021/03/02/6kccb6.png)](https://imgtu.com/i/6kccb6)

[^]: Unity Game界面

[![6kcRUO.png](https://s3.ax1x.com/2021/03/02/6kcRUO.png)](https://imgtu.com/i/6kcRUO)

[^]: MySQL WorkBench 界面

