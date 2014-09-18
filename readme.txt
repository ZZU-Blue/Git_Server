Apache与Python的相关配置


以下所有安装包要注意适用的操作系统版本（32位和64位）
1、安装Python2.7
2、安装Apache服务，版本2.2
3、安装PostgreSQL数据库版本9.3
4、安装pgadmin，操作PostgreSQL数据库的客户端程序版本不限
5、根据使用Python2.7，和Windows是64位下载Python使用PostgreSQL数据的第三方库，然后安装。下载地址是
	http://www.stickpeople.com/projects/python/win-psycopg/psycopg2-2.5.4.win-amd64-py2.7-pg9.3.5-release
6、Apache配置注意事项

a.
在C:\Program Files\Apache Software Foundation\Apache2.2\conf\httpd.conf文件中LoadModule区域添加下面一句话

LoadModule wsgi_module modules/mod_wsgi.so
mod_wsgi.so是需要下载，下载地址是：http://www.lfd.uci.edu/~gohlke/pythonlibs/#mod_wsgi/mod_wsgi-3.5.ap22.win-amd64-py2.7.zip
上面的版本是针对apache是版本2.2,WinOS64位，Python2.7版本对应的库。
b.
修改DocumentRoot的路径为html页面存放目录
#
# DocumentRoot: The directory out of which you will serve your
# documents. By default, all requests are taken from this directory, but
# symbolic links and aliases may be used to point to other locations.
#
DocumentRoot "C:/website/htdocs"
c.
修改默认目录为html页面存放的目录位置
#
# This should be changed to whatever you set DocumentRoot to.
#
<Directory "C:/website/htdocs">
    #
    # Possible values for the Options directive are "None", "All",
    # or any combination of:
    #   Indexes Includes FollowSymLinks SymLinksifOwnerMatch ExecCGI MultiViews
    #
    # Note that "MultiViews" must be named *explicitly* --- "Options All"
    # doesn't give it to you.
    #
    # The Options directive is both complicated and important.  Please see
    # http://httpd.apache.org/docs/2.2/mod/core.html#options
    # for more information.
    #
    Options Indexes FollowSymLinks

    #
    # AllowOverride controls what directives may be placed in .htaccess files.
    # It can be "All", "None", or any combination of the keywords:
    #   Options FileInfo AuthConfig Limit
    #
    AllowOverride None

    #
    # Controls who can get stuff from this server.
    #
    Order allow,deny
    Allow from all

</Directory>
d.
放开使用下面的配置文件
# Virtual hosts
Include conf/extra/httpd-vhosts.conf

7修改httpd-vhosts.conf的配置文件
a.修改VirtualHost内容 DocumentRoot 为存放html页面的路径 ServerName为机器的IP地址
WSGIScriptAlias为提供的服务类型对应的服务器程序，Directory目录是存放服务器程序的路径，设置路径属性有访问权限
#
# VirtualHost example:
# Almost any Apache directive may go into a VirtualHost container.
# The first VirtualHost section is used for all requests that do not
# match a ServerName or ServerAlias in any <VirtualHost> block.
#
<VirtualHost *:80>
    ServerAdmin webmaster@dummy-host.local.com
    DocumentRoot "C:/website/htdocs/"
    ServerName 192.168.1.225
    ServerAlias www.dummy-host.local.com
    ErrorLog "logs/dummy-host.local.com-error.log"
    CustomLog "logs/dummy-host.local.com-access.log" common
    WSGIScriptAlias /insert_tab_project C:/WebServerPython/server_project_form.py
    WSGIScriptAlias /insert_tab_project_expense_item C:/WebServerPython/server_project_expense_item_form.py
    WSGIScriptAlias /retrieve_project_id C:/WebServerPython/server_retrieve_project_id.py
    WSGIScriptAlias /retrieve_project_name C:/WebServerPython/server_retrieve_project_name.py
    WSGIScriptAlias /retrieve_all_project_name C:/WebServerPython/server_retrieve_all_project_name.py
    WSGIScriptAlias /test C:/WebServerPython/test1.py
    <Directory C:/WebServerPython/>
            Order deny,allow
            Allow from all
    </Directory>
</VirtualHost>
