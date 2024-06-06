# MyJdCOOKIE

## 介绍
- 用来自动化更新青龙面板的失效JD_COOKIE, 主要有三步
    - 自动化获取青龙面板的失效JD_COOKIE
    - 基于失效JD_COOKIE, 自动化登录JD,包括滑块验证, 拿到key
    - 基于key, 自动化更新青龙面板的失效JD_COOKIE
- python >= 3.9 (playwright依赖的typing，在3.7和3.8会报错typing.NoReturn的BUG)
- 基于windows
- 整体效果如下图

![GIF](./img/main.gif)


## TODOLIST
- 批量更新多账号(已实现)
- selenium加载太慢，用playwright改写(已实现)
- 自动识别拖动验证码(已实现)
- 加日志(已实现)
- 写使用文档(已实现)
- 加一些通知如钉钉等(已实现)
- 添加获取滑块x,y坐标的工具(已实现)
- 加入了自动验证形状码的方法(已实现)

## 使用文档
### 安装依赖
```commandline
pip install -r requirements.txt
```

### 安装chromium插件
```commandline
playwright install chromium
```

### 获取二次形状验证图左上角的坐标
```commandline
python locate_tool4shape.py
```
- 运行脚本后，等待浏览器自动滑块验证后，进入二次形状验证码后，点击左上角触发脚本捕获到backend_top_left_x, backend_top_left_y

- 如下图

![PNG](./img/sharp_click.png)

- 运行情况如下图
![PNG](./img/run_loate4shape.png)

### 添加配置config.py
- 复制config_example.py, 重命名为config.py, 我们基于这个config.py运行程序;
- user_datas及qinglong_data为用户数据,按照实际信息填写;
- auto_move为自动识别并移动滑块验证码的开关, 有时不准就关了;
- slide_difference为滑块验证码的偏差, 如果一直滑过了, 或滑不到, 需要调节下;
- auto_shape_recognition为二次图形状验证码的开关;
- backend_top_left_x, backend_top_left_y 为形状图的左上角坐标,通过locate_tool4shape.py脚本获得
- cron_expression基于cron的表达式，用于schedule_main.py定期进行更新任务
- 消息类的配置下面会说明


### 配置消息通知
#### 1、如果不需要发消息，请关掉消息开关，忽略消息配置
```commandline
# 是否开启发消息
is_send_msg = False
```
#### 2、成功消息和失败消息也可以开关
```commandline
# 更新成功后是否发消息的开关
is_send_success_msg = True
# 更新失败后是否发消息的开关
is_send_fail_msg = True
```

#### 3、可以发企微、钉钉、飞书机器人，其它的就自写webhook
```commandline
# 配置发送地址
send_info = {
    "send_wecom": [
        "https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key="
    ],
    "send_webhook": [
        "http://127.0.0.1:3000/webhook",
        "http://127.0.0.1:4000/webhook"
    ],
    "send_dingtalk": [
    ],
    "send_feishu": [
    ]
}
```


### 运行脚本
#### 1、单次手动执行
```commandline
python main.py
```

#### 2、常驻进程
进程会读取config.py里的cron_expression,定期进行更新任务
```commandline
python schedule_main.py
```

### 特别感谢
- 感谢 **https://github.com/sml2h3/ddddocr** 项目，牛逼项目