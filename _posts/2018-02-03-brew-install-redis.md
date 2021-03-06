---
layout: post
title: 'Ruby On Rails 異步任務處理'
date: 2018-02-03 10:04
comments: true
categories: Redis
tags: Redis
---

使用[redis-rb](https://github.com/redis/redis-rb) 數據庫gem
請先在本機安裝無法透過Gemfile安裝
```rb
gem install redis
```
接下來請開啟redis所需的server跑起來看看是否有安裝成功
```rb
redis-server /usr/local/etc/redis.conf
```
應該會長這樣

![scrollbar.PNG](https://s3-ap-northeast-1.amazonaws.com/gregningpublic/a8COh8vT0mKC41ymsY83.png)
接下來安裝[sidekiq](https://github.com/mperham/sidekiq)
sidekiq 搭配 redis效果非常突出
請在Gemfile安裝
```rb
gem 'sidekiq'
```
新增以下指令編輯 `config/environments/development.rb` 和 `config/environments/production.rb` 告訴 Rails 要用 sidekiq 来做異步處理
```rb
config.active_job.queue_adapter = :sidekiq
```
新增 `config\sidekiq.yml`(請使用空格)
```yaml
  :queues:
    default
    mailers
```
新增異步處理任務`rails g job import_worker`
編輯`app/jobs/import_worker_job.rb`
```rb
def perform(import_id)
  #do somethings
end
```
Controller加入以下代碼就可以呼叫了
```rb
ImportWorkerJob.perform_later(@import.id)
```
在專案底下執行下面指令就可以看到是誰在幫你做執行了
`bundle exec sidekiq`

在Sidekiq有專用的dashboard可以查看瀏覽狀態紀錄
如果有設定權限請記得給予
```rb
require 'sidekiq/web'
authenticate :user, lambda { |u| u.is_admin? } do
  mount Sidekiq::Web => '/sidekiq'
end
```
瀏覽網址`http://localhost:3000/sidekiq`