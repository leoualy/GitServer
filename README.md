# GitServer
记录搭建局域网git服务的过程
1、操作系统linux
2、web服务器apache2.x
3、git 需要高于1.6.6

1、只能读取版本库的apache配置：
  SetEnv GIT_PROJECT_ROOT /home/git
  SetEnv GIT_HTTP_EXPORT_ALL
  ScriptAlias /git/ /usr/lib/git-core/git-http-backend/
  
  <Location />
    Options +ExecCGI
    Require all granted
  </Location>
  
2、写操作授权的apache配置：
    <LocationMatch "^/git/.*/git-receive-pack$">
      AuthType Basic
      AuthName "Git Access"
      AuthBasicProvider file
      AuthUserFile /var/www/git/git-team
    </LocationMath>
 
 版本库的所属分组需要 通过 chown -R www-data:www-data /home/git/*.git 命令改变其所属用户组为www-data ，
 这样apache才有操作该版本库的权限
 在每个版本库中的config中需要添加如下配置：
 [http]
      receivepack=true
