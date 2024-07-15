# 开发效能与DevOps

## 开发效能

code-server
网站版vscode，和gitlab提供的Web IDE相比，可以直接访问服务器环境，可以根据开发习惯自行选择。
每个code-server实例需要占用一个端口，自身占用较小的资源。
每个开发同事在68环境有一个账号，一种合理的用法是每个同事分别在自己的账号下安装code-server，并通过nginx转接端口。
假设端口8080-8089，最多可分给10位同事使用：

`http://10.144.66.68/code-server/huaisy`
对应配置为
```
server {
  listen 80;
  server_name 10.144.66.68;

  location /code-server/huaisy {
    proxy_pass http://127.0.0.1:8081/;
    proxy_set_header Host $host;
    ...
  }
}
```

`http://10.144.66.68/code-server/zhuangmz`
对应配置为
```
server {
  listen 80;
  server_name 10.144.66.68;

  location /code-server/huaisy {
    proxy_pass http://127.0.0.1:8082/;
    ...
  }
}
```

以此类推

已经安装好的效果：
```
# 在服务器中启动code-server，可以通过systemctl保持后台运行
nohup ~/code-server/bin/code-server --config ~/code-server/config/config.yaml > ~/code-server/code-server.log 2>&1 &

# 在浏览器输入IP+Port访问
http://10.144.66.68:8081/?folder=/home/huaisy
```



## DevOps
### GitLab CI

100服务器的GitLab是怎么装的？
- rpm包安装
- docker容器化
- k8s

区别与优缺点：
- rpm包安装最方便，但是服务器不好管理
- docker适合小规模轻量级，需要手动管理，可能发生单点故障
- k8s适合多节点管理，资源效率高，但是配置较为复杂

1. GitLab Runner
* 概述
  * 运行作业并将结果返回GitLab，因此需要与GitLab版本同步。
  * 部署Runner需要使用Docker，可以配置任意数量的Runner。
* 类型
  * shared，整个GitLab平台所有项目可用
  * group，特定group下的项目可用
  * specific，指定项目可用
 





