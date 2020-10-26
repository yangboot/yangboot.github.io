---
title:  jenkins触发器
linktitle:  jenkins触发器
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
 jenkins-study:
    parent: jenkins持续集成
    weight: 6
---
## 前提条件
开启邮箱服务器的SMTP功能


## 安装插件


Email Extension Template

## 配置

![](/img/jenkins/20.jpg)
![](/img/jenkins/21.jpg)
![](/img/jenkins/22.jpg)
![](/img/jenkins/23.jpg)
## 测试成功
![](/img/jenkins/24.jpg)

## 添加邮件模板

项目根目录下添加邮件模板`email.html`

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>${ENV, var="JOB_NAME"}-第${BUILD_NUMBER}次构建日志</title>
</head>
<body leftmargin="8" marginwidth="0" topmargin="8" marginheight="4" offset="0">
<table width="95%" cellpadding="0" cellspacing="0" style="font-size: 11pt; font-family: Tahoma, Arial, Helvetica, sans-serif">
<tr>
   <td>(本邮件是程序自动下发的，请勿回复！)</td>
</tr>
<tr>
   <td><h2><font color="#0000FF">构建结果 - ${BUILD_STATUS}</font></h2></td>
</tr>
<tr>
   <td><br /> <b><font color="#0B610B">构建信息</font></b> <hr size="2" width="100%" align="center" /></td> 
</tr> 
<tr>
<td> 
     <ul> 
          <li>项目名称&nbsp;：&nbsp;${PROJECT_NAME}</li>
          <li>构建编号&nbsp;：&nbsp;第${BUILD_NUMBER}次构建</li>
          <li>触发原因：&nbsp;${CAUSE}</li>
          <li>构建日志：&nbsp;<a href="${BUILD_URL}console">${BUILD_URL}console</a></li>
          <li>构建&nbsp;&nbsp;Url&nbsp;：&nbsp;<a href="${BUILD_URL}">${BUILD_URL}</a></li>
          <li>工作目录&nbsp;：&nbsp;<a href="${PROJECT_URL}ws">${PROJECT_URL}ws</a></li>
          <li>项目&nbsp;&nbsp;Url&nbsp;：&nbsp;<a href="${PROJECT_URL}">${PROJECT_URL}</a></li>
     </ul> 
</td> 
</tr>
<tr>
     <td>
          <b><font color="#0B610B">Changes Since Last Successful Build:</font></b>
          <hr size="2" width="100%" align="center" />
     </td> 
</tr> 
<tr> 
     <td>
         <ul> 
              <li>历史变更记录 : <a href="${PROJECT_URL}changes">${PROJECT_URL}changes</a></li>
         </ul></td> 
</tr>
<tr>
     <td>
          <b>Failed Test Results</b> <hr size="2" width="100%" align="center" />
     </td> 
</tr>
<tr>
     <td>
          <pre style="font-size: 11pt; font-family: Tahoma, Arial, Helvetica, sans-serif">$FAILED_TESTS</pre> <br />
     </td>
</tr>
<tr>
     <td><b><font color="#0B610B">构建日志 (最后 100行):</font></b> <hr size="2" width="100%" align="center" /></td>
</tr> <!-- <tr> <td>Test Logs (if test has ran): <a href="${PROJECT_URL}ws/TestResult/archive_logs/Log-Build-${BUILD_NUMBER}.zip">${PROJECT_URL}/ws/TestResult/archive_logs/Log-Build-${BUILD_NUMBER}.zip</a> <br /> <br /> </td> </tr> --> 
<tr>
<td>
     <textarea cols="80" rows="30" readonly="readonly" style="font-family: Courier New">${BUILD_LOG, maxLines=100}</textarea> 
</td> 
</tr>
</table>
</body>
</html>
```

## 修改jenkinsfile

```javascript

        post {
             always {
               emailext(
               		subject: '构建通知: ${PROJECT_NAME} - Build  # ${BUILD_NUMBER} - ${BUILD_STATUS)!',
                    body: '${FILE,path="email.html"}',
                    to: '1785868089@qq.com'
               )  
            }
            
        }
    
```

立即构建

![](/img/jenkins/25.jpg)