# Rails 4 Sample App on OpenShift #
Quickstart rails 4 application for openshift.

The easiest way to install this application is to use the [OpenShift
Instant Application][template]. If you'd like to install it
manually, follow [these directions](#manual-installation).

## OpenShift Considerations ##
These are some special considerations you may need to keep in mind when
running your application on OpenShift.

### Database ###
Your application is configured to use your OpenShift database in
Production mode.
Because it addresses these databases based on [OpenShift Environment
Variables](http://red.ht/NvNoXC), you will need to change these if you
want to use your application in Production mode outside of
OpenShift.

By default the development and test environment is configured to use
the sqlite3 database adapter.

You can also speed up the `git push` process by excluding gems you don't need,
based on the database you use in OpenShift. You can use the `BUNDLE_WITHOUT`
environment variable for that:

```
$ rhc env set BUNDLE_WITHOUT="development test postgresql"
```

Use the command above if you don't want to install any development gems and you
are using OpenShift MySQL cartridge.

### Assets ###
Your application is set to precompile the assets every time you push
to OpenShift. Any assets you commit to your repo will be preserved
alongside those which are generated during the build.

By adding `disable_asset_compilation` marker, you will disable asset compilation upon application deployment.

### Security ###
Since these quickstarts are shared code, we had to take special
consideration to ensure that security related configuration variables
was unique across applications.
To accomplish this, we modified some of the configuration files (shown
in the table below).
Now instead of using the same default values, your application will
generate it's own value using the `initialize_secret` function from `lib/openshift_secret_generator.rb`.

This function uses a secure environment variable that only exists on
your deployed application and not in your code anywhere.
You can then use the function to generate any variables you need.
Each of them will be unique so `initialize_secret(:a)` will differ
from `initialize_secret(:b)` but they will also be consistent, so any
time your application uses them (even across reboots), you know they
will be the same.

TLDR: You should copy/link the `.openshift/lib/openshift_secret_generator.rb`
file into `./lib` folder and link the `secret_token.rb` and `session_store.rb`
files into `./config/initializers` folder. Look at this quickstart for an
example.

### Development mode ###
When you develop your Rails application in OpenShift, you can also enable the
'development' environment by setting the `RAILS_ENV` environment variable,
using the `rhc` client, like:

```
$ rhc env set RAILS_ENV=development
```

If you do so, OpenShift will run your application under 'development' mode.
In development mode, your application will:

* Show more detailed errors in browser
* Skip static assets (re)compilation
* Skip web server restart, as the code is reloaded automatically
* Skip `bundle` command if the Gemfile is not modified

Development environment can help you debug problems in your application
in the same way as you do when developing on your local machine.
However, we strong advise you to not run your application in this mode
in production.

##### Modified Files #####

<table>
  <tr>
    <th>File</th>
    <th>Variable</th>
  </tr>
  <tr>
    <td>config/initializers/secret_token.rb</td> 
    <td>Railsapp::Application.config.secret_token</td>
  </tr>
  <tr>
    <td>config/initializers/session_store.rb</td>
    <td>Railsapp::Application.config.session_store</td>
  </tr>
</table>

## Manual Installation ##

1. Create an account at https://www.openshift.com

1. Create a rails application

    ```
    rhc app create railsapp ruby-2.0
    ```

   **Note:** This quickstart will not work with Ruby 1.8


1. Add database support to your application

    ```
    rhc cartridge add -a railsapp -c mysql-5.5
    ```

    or

    ```
    rhc cartridge add -a railsapp -c postgresql-9.2
    ```

1. Add this upstream Rails quickstart repository

    ```
    cd railsapp
    git remote add upstream -m master git://github.com/openshift/rails4-example.git
    git pull -s recursive -X theirs upstream master
    ```

1. Push your new code

    ```
    git push
    ```

1. That's it! Enjoy your new Rails application!


[template]: https://openshift.redhat.com/app/console/application_types

License
-------

This code is dedicated to the public domain to the maximum extent permitted by applicable law, pursuant to CC0 (http://creativecommons.org/publicdomain/zero/1.0/)



#Blog演示页面

##首页

![首页](https://github.com/fxhover/blog/raw/master/doc/index.png)

##博文列表

![文章列表](https://github.com/fxhover/blog/raw/master/doc/articles_list.png)

##写博客页面

![写博客](https://github.com/fxhover/blog/raw/master/doc/new_article.png)

##文章详情页面

![文章详情](https://github.com/fxhover/blog/raw/master/doc/show_article.png)

##设置页面

![设置头像](https://github.com/fxhover/blog/raw/master/doc/set1.png)

![设置博客](https://github.com/fxhover/blog/raw/master/doc/set2.png)

#Blog部署文档#

##环境
    git、ruby 2.1.2、rails 4.1.5、nginx 1.2+、mysql 5.0+

##下载代码##
    
    git clone git@github.com:fxhover/blog.git

##安装ruby依赖类库##

    bundle exec bundle install

##配置文件##

    cd blog

    cp config/database.yml.example config/database.yml 修改数据库配置信息

    cp config/secrets.yml.example config/secrets.yml
 
    cp config/blog.yml.example config/blog.yml 修改好博客配置信息

    cp config/unicorn.rb.example config/unicorn.rb 修改unicorn配置文件，配置应用目录等信息

##执行数据脚本##

    RAILS_ENV=production bundle exec rake db:create  创建数据库
   
    RAILS_ENV=production bundle exec rake db:migrate 创建表及索引

    RAILS_ENV=production bundle exec rake db:seed 创建默认分类和管理员账号，编辑 db/seeds.rb可以修改默认分类和管理员账号密码，然后再执行

##预编译前端资源##

    RAILS_ENV=production bundle exec rake assets:clean
    
    RAILS_ENV=production bundle exec rake assets:precompile

##启动##

###创建服务用到的文件夹###
    mkdir -p tmp/pids

    mkdir -p log 

###拷贝启动服务文件###

    sudo cp deployment/init.d/blog /etc/init.d 编辑文件中的app目录和sudo user的配置

###启动blog服务###
    
    sudo service blog start
    
    sudo service blog stop/restart 停止或者重启服务

###nginx配置###

    以下列出部分配置，完整配置见 deployment/nginx/nginx.conf文件

    upstream blog {
        server unix:/home/fangxiang/blog/tmp/sockets/blog.socket;
    }
    server {
        listen 80;
        server_name blog.com;
        root /home/fangxiang/blog/public;
        try_files $uri/index.html $uri.html $uri @user1;
        location @user1 {
            proxy_redirect off;
            proxy_set_header Host $host;
            proxy_set_header X-Forwarded-Host $host;
            proxy_set_header X-Forwarded-Server $host;
            proxy_set_header X-Real-Ip $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_buffering on;
            proxy_pass http://blog;
        }
        location ~ ^(/assets) {
            access_log off;
            expires max;
        }

    }

###重启nginx###
    
    sudo service nginx restart
###绑定hosts
   
    127.0.0.1  blog.com 

###访问###

    http://blog.com

