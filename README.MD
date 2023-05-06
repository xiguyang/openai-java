## OpenAi-Java

一个基于Spring boot 2.6.4，thymeleaf，gpt-3.5模型的Openai-Java-Web项目

前端界面基于`Hostloc`论坛流出的源码修改而来，目前支持的功能如下

- 支持余额查询
- 支持随机获取Key，支持自定义Key
- 支持生成图片，连续对话(基于gpt-3.5模型)

### 项目截图

![](https://s2.loli.net/2023/02/17/o93NLaA2d5YwClJ.png)

### Docker一键运行项目

docker镜像地址与教程：https://hub.docker.com/r/dqjdda/openai-java

```bash
# 替换 KEYS 即可
# 参数教程见：https://hub.docker.com/r/dqjdda/openai-java
# Arm 机器实用 dqjdda/openai-java:arm64 镜像
# TIPS 是网站首页的提示信息，不支持html语法
docker run -d \
--name openai --restart always \
-p 18080:18080 \
-e MAX_TOKENS=512 \
-e TEMPERATURE=0.6 \
-e KEYS=sk-xxxx,sk-xxxx \
-e BD_TONG_JI_TOKEN= \
-e TIPS="ChatGPT 是一个超强的人工智能语言模型，它会创作、写论文、答辩、编程，绘画等" \
dqjdda/openai-java
```

### 运行项目

1. 修改 application.yml 中的 apiKey
2. 运行 OpenAiRun 类
3. 访问 http://localhost:18080

### 部署项目

1. 打包为 war 包后，上传到服务器
2. 运行项目 nohup java -jar openai-1.0.0.war > nohup.out 2>&1 &
3. 访问 http://IP:18080

### nginx 反代
```bash
server
{
    listen 80;
    server_name yourdomain;

    location / {
        proxy_read_timeout 120; 
        proxy_pass http://127.0.0.1:18080;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Port $server_port;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

##fyc运行测试
无论是gpt3.5  4.0的账号，生成的api_key,在访问api时 HttpRequest.post，都无法访问。提示ConnectException: Connection timed out: connect
账号在官网上https://chat.openai.com/ 可以使用的，都是在vp加速环境下进行的。
经官网查询，有如下这段话：
ChatGPT Plus 订阅中是否包含 ChatGPT API？
不，ChatGPT API 和 ChatGPT Plus 订阅是分开计费的。API 有自己的定价，可以在https://openai.com/pricing找到。ChatGPT Plus 订阅仅涵盖在 chat.openai.com 上的使用，每月收费 20 美元。


