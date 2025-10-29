#### 安装 MinIO

- 下载

  ```ini
  #由于需要用到wget命令，所以要先安装wget
  [root@localhost ~]# yum -y install wget
  # 创建minio目录
  [root@localhost ~]# mkdir -p /usr/local/minio/bin
  [root@localhost ~]# mkdir -p /usr/local/minio/data
      
  进入/usr/local/minio/bin执行如下命令:
  [root@localhost ~]#	cd /usr/local/minio/bin
  [root@localhost bin]# wget https://dl.minio.org.cn/server/minio/release/linux-amd64/minio
  	
  赋予它可执行权限: 进入到/usr/local/minio/bin中执行如下命令
  [root@localhost bin]# chmod +x /usr/local/minio/bin/minio
  ```

- 已有安装包

  - 创建minio目录

    ```ini
    # 创建minio目录
    [root@localhost ~]# mkdir -p /usr/local/minio/bin
    [root@localhost ~]# mkdir -p /usr/local/minio/data
    ```

  - 上传minio

    后上传到/usr/local/minio/bin目录中

  - 授权

    ```ini
    #赋予它可执行权限: 进入到/usr/local/minio/bin中执行如下命令
    [root@localhost bin]# chmod +x /usr/local/minio/bin/minio
    ```

- 将 minio 添加成 Linux 的服务

  ```ini
  cat > /etc/systemd/system/minio.service << EOF
  [Unit]
  Description=Minio
  Wants=network-online.target
  After=network-online.target
  AssertFileIsExecutable=/usr/local/minio/bin/minio
  
  [Service]
  WorkingDirectory=/usr/local/minio/
  PermissionsStartOnly=true
  ExecStart=/usr/local/minio/bin/minio server /usr/local/minio/data
  ExecReload=/bin/kill -s HUP $MAINPID
  ExecStop=/bin/kill -s QUIT $MAINPID
  Restart=always
  PrivateTmp=true
  
  [Install]
  WantedBy=multi-user.target
  EOF
  ```

- 重新加载系统服务

  ```ini
  [root@localhost bin]# systemctl daemon-reload
  ```

- 启动minio

  ```ini
  systemctl start minio.service   # 启动
  systemctl stop minio.service    # 停止
  systemctl enable minio.service --now  #设置自动开启
  systemctl disable minio.service #禁用开机启动
  ```

  

  后台启动

  ```
  # 假设已设置环境变量（可选，若修改了凭据）
  nohup ./minio server /data/minio --console-address ":8040" > /home/software/minio/minio.log 2>&1 &
  ```

  

  ```
  # nohup：忽略终端挂断信号，确保终端关闭后程序仍能继续运行
  nohup \
    ./minio \                # 执行当前目录下的minio可执行文件（需确保已赋予执行权限）
    server \                 # minio的核心命令，用于启动对象存储服务
    /data/minio \  # 指定minio的数据存储目录（所有上传的文件会保存在此目录）
    --console-address ":8040" \  # 自定义Web控制台端口为8040（默认9001，避免端口冲突时使用）
    > /home/software/minio/minio.log \  # 将程序的标准输出（stdout，如正常运行日志）重定向到指定文件
    2>&1 \                    # 将标准错误（stderr，如错误信息）重定向到标准输出，与正常日志一起写入上面的文件
    &                         # 将程序放入后台运行，不阻塞当前终端（可继续输入其他命令）
  ```

  

- 访问minio

  地址：http://服务器ip:9000

  帐号：minioadmin

  密码：minioadmin