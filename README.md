1.打开以下网页，选择右上角Sign Up注册账号

[https://huggingface.co/](https://)

<br/>

2.注册完成后选择顶部导航栏的Spaces->Create new Space

Space name随便填，License填MIT，点击下方Docker图标，翻到最下面点击Create Space

此时将会提示你邮箱未验证，去邮箱验证一下再返回这个界面，若邮箱未收到验证信息则点击界面上方的提示信息再次发送

<br/>

3.验证完成并点击了Create Space后，请翻到页面中间找到“Create”，在浏览器中创建Dockerfile文件，填写的文件内容如下：

注意：Go_Proxy_BingAI_USER_TOKEN_1字段您可以随便填写

<br/>

```dockerfile
# Build Stage
# 使用 golang:alpine 作为构建阶段的基础镜像
FROM golang:alpine AS builder

# 添加 git，以便之后能从GitHub克隆项目
RUN apk --no-cache add git

# 从 GitHub 克隆 go-proxy-bingai 项目到 /workspace/app 目录下
RUN git clone https://github.com/Harry-zklcdc/go-proxy-bingai.git /workspace/app

# 设置工作目录为之前克隆的项目目录
WORKDIR /workspace/app

# 编译 go 项目。-ldflags="-s -w" 是为了减少编译后的二进制大小
RUN go build -ldflags="-s -w" -tags netgo -trimpath -o go-proxy-bingai main.go

# Runtime Stage
# 使用轻量级的 alpine 镜像作为运行时的基础镜像
FROM alpine

# 设置工作目录
WORKDIR /workspace/app

# 从构建阶段复制编译后的二进制文件到运行时镜像中
COPY --from=builder /workspace/app/go-proxy-bingai .

# 设置环境变量，此处为随机字符
ENV Go_Proxy_BingAI_USER_TOKEN_1="hdagsbhenjouinqvoabsvvovbgre"

# 暴露8080端口
EXPOSE 8080

# 容器启动时运行的命令
CMD ["/workspace/app/go-proxy-bingai"]
```

4.填写完毕后翻到最后点击Commit new file to main，观察到正在Building，在Building同行点击App查看构造进度，等待少许时间返回Files发现已生成三个文件，点击README.md文件，在编辑工具栏找到并点击edit，在license: mit这一行回车，键入以下代码：

```markdown
app_port: 8080
```

<br/>

5.然后翻到最后点击Commit changes to main，稍等一会发现之前的Building变成了Running就代表创建成功了，再次返回到App界面发现已进入NewBing聊天窗口，叉掉聊天服务器设置就可以开始使用NewBing了
