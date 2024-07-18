- 问题: docker build 命令显示docker hub无法链接上

- 解决方法:

  编辑daemon.json

  ```bash
  sudo vim /etc/docker/daemon.json
  ```

  更改网络代理:

  ```bash
  {
      "proxies": {        
          "http-proxy": "http://127.0.0.1:8889",
          "https-proxy": "http://127.0.0.1:8889",
          "no-proxy": "localhost,127.0.0.0/8"
      }
  }
  ```

  重新启动docker

  ```bash
  sudo systemctl restart docker
  ```

  

