+++
title = '使用gitee的webhook实现提交代码后自动部署项目'
tags = ['webhook']
series = ["CI/CD"]
series_order = 1
order = 1
categories = ['编程开发']
draft = false
+++
## 业务背景
giteeAction付费，gitPages也停止了服务，没办法想搞一个国内服务器的独立站只能自己动手了，看了gitee提供了webhook👌那一切就很简单了
![在这里插入图片描述](cicd/giteeHookPage.png)

## 部署架构
![在这里插入图片描述](cicd/webHookDeploy.jpg)
- 用户做提交代码等操作时，如果满足webhook条件则会发送webhook请求到固定的服务器路由
- 服务器路由接收到之后就更新git仓库（go 提供服务，真的很方便）
- 然后生成静态文件部署到nginx

## webhook设置
![在这里插入图片描述](cicd/giteeHookDetail.png)
选择触发事件以及服务路由即可，触发对应事件的时候就会走POST调用

## go编写web服务
真的比java快速且方便很多，需要加密自己写即可
```go
package main

import (
    "fmt"
    "log"
    "net/http"
    "os/exec"
)

func webhookHandler(w http.ResponseWriter, r *http.Request) {
    if r.Method == http.MethodPost {
        // 解析 Webhook 请求的内容
        // 这里只是简单的示例，实际情况可能需要更多的处理
        body := r.Body
        defer body.Close()
        // 你可以根据需要解析请求体中的信息

        // 执行 Shell 命令
        cmd := exec.Command("/bin/bash", "-c", "./deploy.sh")
        output, err := cmd.CombinedOutput()
        if err != nil {
            http.Error(w, "Failed to execute command", http.StatusInternalServerError)
            log.Println("Command execution error:", err)
            return
        }

        // 输出执行结果
        fmt.Fprintf(w, "Command executed successfully: %s", output)
    } else {
        http.Error(w, "Invalid request method", http.StatusMethodNotAllowed)
    }
}

func main() {
    http.HandleFunc("/webhook", webhookHandler)
    log.Println("Starting server on :8080")
    if err := http.ListenAndServe(":8080", nil); err != nil {
        log.Fatal("ListenAndServe: ", err)
    }
}
```

## 部署脚本

```powershell
#!/bin/bash

# 定义路径
path="/opt/webhook/blue-whale/"
deployPath="/opt/www/blueWhale/"
gitPath="git@gitee.com:jzwPro/blue-whale.git";

# 检查文件夹是否存在
if [ ! -d "${path}" ]; then
    echo "Directory does not exist. Cloning the repository..."
    # 从 Git 仓库克隆项目
    git clone ${gitPath} "${path}"
else
    echo "Directory exists. Pulling the latest changes..."
fi

cd "${path}" || exit
git pull

# 生成 Hugo 静态文件
/opt/hugo/hugo

# 删除目标目录及其内容（如果存在）
rm -rf "${deployPath}"
mkdir -p "${deployPath}"

# 复制生成的静态文件到目标目录
cp -r ./public/* "${deployPath}"

echo "Deployment completed successfully."
```