---
layout: post
title: 'MVC Use ELMAH監控程式'
date: 2017-10-03 16:13
comments: true
categories: MVC
tags: MVC Csharp
reference:
  name:
    - aspnet-mvc-elmah
    - elmah
  link:
    - http://kevintsengtw.blogspot.tw/2011/10/aspnet-mvc-elmah-4.html
    - https://jeffprogrammer.wordpress.com/2015/12/05/asp-net-mvc-%E7%9B%A3%E6%8E%A7%E7%B3%BB%E7%B5%B1%E5%81%A5%E5%BA%B7-elmah-%E4%BD%BF%E7%94%A8%E7%B4%80%E9%8C%84/
---
#### 使用Elmah監控程式若出現錯誤會寄信或是寫log進資料庫，大部分的動作直接寫到`web.config`裡面。
```conf
PM > Install-Package elmah
PM > Install-Package elmah.xml (安裝 ELMAH on XML Log，若要使用 DB，不須安裝這個)
PM > Install-Package elmah.mvc
# 下面這是inserts sql server需要
PM > Install-Package ELMAH on MS SQLServer
# 當你安裝完成他會出現以下
<configSections>
此區為elmah紀錄錯誤log地帶 ErrorMail
<sectionGroup name="elmah">
<section name="security" requirePermission="false" type="Elmah.SecuritySectionHandler, Elmah" />
<section name="errorLog" requirePermission="false" type="Elmah.ErrorLogSectionHandler, Elmah" />
<section name="errorMail" requirePermission="false" type="Elmah.ErrorMailSectionHandler, Elmah" />
<section name="errorFilter" requirePermission="false" type="Elmah.ErrorFilterSectionHandler, Elmah" />
<sectionGroup>
<configSections>
<add key="aspnet:UseTaskFriendlySynchronizationContext" value="true" />
```
##### 設定是否停用 ELMAH.MVC Handler (就是顯示 Elmah 錯誤訊息的頁面)
```conf
<add key="elmah.mvc.disableHandler" value="false" />
```
##### 設定是否停用預設的 HandleErrorAttribute 例外動作過濾器
```conf
<add key="elmah.mvc.disableHandleErrorFilter" value="false" />
```
##### 是否啟用 ELMAH 的身分驗證功能，如果啟用的話，進入 ELMAH 頁面則必須先登入網站
在此有相關的安全性問題，必須將下兩個設定為true。
```conf
<add key="elmah.mvc.requiresAuthentication" value="true" />
<add key="elmah.mvc.IgnoreDefaultRoute" value="true" />
```
##### 設定有哪些 ASP.NET 角色才能夠開啟 ELMAH 頁面 (基本授權機制)
預設的`*`代表所有角色，也就只要有登入網站(FormsAuthentication)就可以存取。
```conf
<add key="elmah.mvc.allowedRoles" value="*" />
<add key="elmah.mvc.allowedUsers" value="*" />
```
##### 設定預設對外的 ELMAH Handler 會用哪一個路由名稱
預設的 elmah 就代表 ELMAH Handler 的 URL 是：
`http://localhost/elmah`
##### 強烈建議修改這個參數值，變更為一個不容易記憶的網址
##### 例如：`ThatIsMyErrorHandler`那麼`ELMAH Handler`的URL就會變成
`http://localhost/ThatIsMyErrorHandler`
#### 在這要注意還要在`App_Start -> RouteConfig.cs`設定，安全性問題最好禁止。
```conf
routes.IgnoreRoute("Elmah");
```
在此是設定路由可由此進入
```conf
<add key="elmah.mvc.route" value="elmah" />
<add key="elmah.mvc.UserAuthCaseSensitive" value="true" />
```
##### LMAH ErrorMail Settings 以下是要使用smtp寄信
```conf
<httpModules>
<add name="ApplicationInsightsWebTracking" type="Microsoft.ApplicationInsights.Web.ApplicationInsightsHttpModule, Microsoft.AI.Web" />
<add name="ErrorLog" type="Elmah.ErrorLogModule, Elmah" />
<add name="ErrorMail" type="Elmah.ErrorMailModule, Elmah" />
<add name="ErrorFilter" type="Elmah.ErrorFilterModule, Elmah" />
</httpModules>
```
###### 需要註記當mode = On 或是 RemoteOnly 可以讓外界人看不到詳細錯誤訊息在此要先設定好倒到錯誤頁面
```conf
<customErrors mode="On" redirectMode="ResponseRewrite" defaultRedirect="~\Views\Shared\Error.cshtml" />
```
下面這段一樣是安裝完成後會出現大概看下就可以
```conf
<modules>
<remove name="ApplicationInsightsWebTracking" />
<add name="ApplicationInsightsWebTracking" type="Microsoft.ApplicationInsights.Web.ApplicationInsightsHttpModule, Microsoft.AI.Web" preCondition="managedHandler" />
<add name="ErrorLog" type="Elmah.ErrorLogModule, Elmah" preCondition="managedHandler" />
<add name="ErrorMail" type="Elmah.ErrorMailModule, Elmah" preCondition="managedHandler" />
<add name="ErrorFilter" type="Elmah.ErrorFilterModule, Elmah" preCondition="managedHandler" />
</modules>
<httpHandlers>
<add verb="POST,GET,HEAD" path="elmah.axd" type="Elmah.ErrorLogPageFactory, Elmah" />
</httpHandlers>
```
#### 設定使用smtp寄信功能
```conf
<mailSettings>
  <smtp deliveryMethod="Network">
    <network host="smtp.gmail.com" port="587" userName="使用者帳號" password="使用者帳號密碼" />
  </smtp>
</mailSettings>
<elmah>
下面這段可以不理直接刪掉都可以但請用Nuget將elmah.xml刪除比較乾淨
errorLog type="Elmah.XmlFileErrorLog, Elmah" logPath="~/App_Data/Elmah.Errors" />
寫入log 進資料庫-->\
  <errorLog type="Elmah.SqlErrorLog, Elmah" connectionStringName="elmah-sqlserver" />
  使用smtp發信收信 有關錯誤訊息 useSsl 有關SSL設定在此需設定
  <errorMail form="使用者帳號" to="要寄送的使用者帳號" smtpPort="587" subject="ELMAH- Error" async="true" useSsl="true" />
  <errorFilter>
    <test>
      不理會 log favicon.ico 以及404的錯誤 在此的and 是 sql and的意思
      <and>
        <equal binding="HttpStatusCode" value="404" type="Int32" />
        <regex binding="Context.Request.ServerVariables['URL']" pattern="/favicon\.ico(\z|\?)" />
       </and>
    </test>
  </errorFilter>
  不准許遠端存取 請使用 false
  <security allowRemoteAccess="false" />
</elmah>
```
新增到`Sql server`請使用 nuget 安裝 `ELMAH on MS SQLServer`在專案有新增`App_Readme`目錄，目錄下有`「Elmah.SqlServer.sql」`檔案，使用裡面的sql語法安裝`ELAMG`所需的store以及table。
