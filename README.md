<div align="center">
  <a href="https://v2.nonebot.dev/store"><img src="https://raw.githubusercontent.com/A-kirami/nonebot-plugin-template/resources/nbp_logo.png" width="180" height="180" alt="NoneBotPluginLogo"></a>
  <br>
  <p><img src="https://raw.githubusercontent.com/A-kirami/nonebot-plugin-template/resources/NoneBotPlugin.svg" width="240" alt="NoneBotPluginText"></p>
</div>

<div align="center">

# 多功能ChatGPT插件
✨基于chatGPT-3.5-turboAPI的nonebot插件✨  
<a href="https://pypi.python.org/pypi/nonebot-plugin-chatgpt-on-qq">
    <img src="https://img.shields.io/pypi/v/nonebot-plugin-chatgpt-on-qq.svg" alt="pypi">
</a>
<img src="https://img.shields.io/badge/python-3.8+-blue.svg" alt="python">

</div>


## 安装  
推荐使用 `nb plugin install nonebot-plugin-chatgpt-on-qq` 一键安装
## 使用前须知    
1. 在env中添加你的api_key(必选)
2. 可自行选择合适的GPT模型，如`gpt-3.5-turbo-0301`，一般无需更改，默认为`gpt-3.5-turbo`，具体可参考[官方文档](https://platform.openai.com/docs/guides/chat/instructing-chat-models)
3. 可设置使用gpt的理智值(temperature)，介于0~2之间，较高值如`0.8`会使对话更加随机，较低值如`0.2`会使对话更加集中和确定，默认为0.5，具体可参考[官方文档](https://platform.openai.com/docs/api-reference/chat/create)
4. 添加代理(国内必选) #填写`NoError`可以去除插件启动时的警告信息
5. 设置历史对话保存路径(可选,默认保存至`./data/ChatHistory`)  
6. 设置保存的最大历史聊天记录长度  
7. cf workers相关(?  (可选)
8. 预设文件夹路径(默认为`./data/Presets`)
9. 是否允许私聊触发插件
10. 因为电脑端的qq在输入/chat xxx时候经常被转换成表情，所以支持自定义指令前缀替换"chat"，支持中文。
    
    比如填写change_chat_to="Chat"，则所有以 /chat 开头的命令都支持 以 /Chat 开头，其余不变，如：/Chat list 

    说是替换，实际上原本的/chat仍然支持触发指令，只不过多了一个自定义别称

    如果不需要可以在配置文件中直接不填写这一项


|        配置项        | 必填  | 类型    |        默认值         |                                   说明                                    |
|:-----------------:|:---:|-------|:------------------:|:-----------------------------------------------------------------------:|
|      api_key      |  是  | str   |      "NoKey"       |                        填入你的api_key,类似"sk-xxx..."                        |
|    model_name     |  否  | str   |  "gpt-3.5-turbo"   |                             模型名称，具体可参考官方文档                              |
|    temperature    |  否  | float |        0.5         | 设置使用gpt的理智值(temperature)，介于0~2之间，较高值如`0.8`会使对话更加随机，较低值如`0.2`会使对话更加集中和确定 |
|   openai_proxy    |  否  | str   |        None        |                          正向HTTP代理 (HTTP PROXY)                          | 
| history_save_path |  否  | str   | "data/ChatHistory" |                               设置历史对话保存路径                                |
|    history_max    |  否  | int   |         10         |                        设置保存的最大历史聊天记录长度，填入大于2的数字                         |
|  openai_api_base  |  否  | str   |        None        |                                  反向代理                                   |
|    preset_path    |  否  | str   |   "data/Presets"   |                              填入自定义预设文件夹路径                               |
|   allow_private   |  否  | bool  |        true        |                              插件是否支持私聊，默认开启                              |
|  change_chat_to   |  否  | str   |        None        |           因为电脑端的qq在输入/chat xxx时候经常被转换成表情，所以支持自定义指令前缀替换"chat"            |

格式如下:  

```
api_key="填入你的api_key"
model_name="gpt-3.5-turbo" #默认为gpt-3.5-turbo，具体可参考官方文档
temperature=0.5 #理智值，介于0~2之间
openai_proxy="x.x.x.x:xxxxx" #填写NoError可去除插件启动时的警告信息
history_save_path="E:/Kawaii" #填入你的历史对话保存路径
history_max = 10 #填入大于2的数字
openai_api_base = "" #cf workers，空字符串或留空都将不使用
preset_path="填入自定义预设文件夹路径"
allow_private=true
change_chat_to="Chat"
```
## 功能  
新:支持读取json格式的预设(具体可以参考Presets文件夹中的json文件)，并使预设可作为模板被创建  
`组合技:可以用/chat create <prmopt>创建空白模板ChatGPT以及后续对话来调教机器人，调教完成后使用/chat dump导出聊天记录json，将user改为system，保存在json文件夹就可以创建一个属于你自己的预设了！`  
支持群聊与私聊  
群聊中可同时创建多个会话,对话间相互独立  
对话时会保存一部分上下文信息(默认保存10条,可在.env中设置)    
能导出导入历史记录一键回到曾经的会话  
有普通chatGPT(id:1)模板基础的猫娘模板(id:2)作为基础prompt来构建对话  
可自动在本地保存所有对话的聊天记录  


## 基础命令  
`/chat` 获取命令菜单  
`/chat create`  根据预制模板prompt创建一个新的对话  
`/chat create` (自定义prompt) (不需要括号，直接跟你的prompt就好)利用后面跟随的prompt作为基础prompt来创建一个新的对话  
`/chat list` 获取当前群所有存在的对话的序号及创建时间  
`/chat join <id>` 加入list中序号为<id>的对话(不需要尖括号，直接跟id就行)  
`/chat delete <id>` 删除list中序号为<id>的对话(不需要尖括号，直接跟id就行)  
`/chat json` 利用历史对话json来回到一个对话,输入该命令后会提示你在下一个消息中输入json  
`/chat dump` 导出当前对话的json文件  
`/talk` (对话内容) 在当前对话中进行对话(同样不需要括号，后面直接接你要说的话就行)  

|             指令             |       权限        | 需要@ |  范围   |                    说明                     |
|:--------------------------:|:---------------:|:---:|:-----:|:-----------------------------------------:|
|          `/chat`           |       群员        |  否  | 私聊/群聊 |                  获取命令菜单                   |
|       `/chat create`       |       群员        |  否  | 私聊/群聊 |           根据预制模板prompt创建一个新的对话            |
| `/chat create <自定义prompt>` |       群员        |  否  | 私聊/群聊 |     利用<自定义prompt>作为基础prompt来创建一个新的对话      |
|        `/chat list`        |       群员        |  否  | 私聊/群聊 |           获取当前群所有存在的对话的序号及创建时间            |
|     `/chat join <id>`      |       群员        |  否  | 私聊/群聊 |     加入list中序号为<id>的对话(不需要尖括号，直接跟id就行)     |
|    `/chat delete <id>`     | 主人/群主/管理员/会话创建人 |  否  | 私聊/群聊 |     删除list中序号为<id>的对话(不需要尖括号，直接跟id就行)     |
|        `/chat json`        |       群员        |  否  | 私聊/群聊 | 利用历史对话json来回到一个对话,输入该命令后会提示你在下一个消息中输入json |
|        `/chat dump`        |       群员        |  否  | 私聊/群聊 |               导出当前对话的json文件               |
|       `/talk <对话内容>`       |       群员        |  否  | 私聊/群聊 |  <对话内容> 在当前对话中进行对话(同样不需要括号，后面直接接你要说的话就行)  |


## 使用效果预览  
### 利用模板创建新的对话  
  ![image](https://user-images.githubusercontent.com/33772816/223602899-77ce2c3b-5d0f-40c2-8183-65e8447d9bec.png)
### 与bot进行对话  
  ![image](https://user-images.githubusercontent.com/33772816/223603028-4aeda385-6d29-4c67-b7b3-5295e7d6976b.png)
### 查看所有对话列表  
  ![image](https://user-images.githubusercontent.com/33772816/223603171-da174c03-ed0a-465d-9fa5-078ebee0602c.png)
### 加入其他对话  
  ![image](https://user-images.githubusercontent.com/33772816/223603352-d72309c8-4339-4630-9eb9-8bea855787d5.png)
### 删除对话  
  ![image](https://user-images.githubusercontent.com/33772816/223603427-146a70ae-7e47-404e-8f80-04c98380e5ba.png)
### 导出json  
  ![image](https://user-images.githubusercontent.com/33772816/223603499-52a2893f-14a7-4d58-9b6d-e8b3b3760d3f.png)
### 导入json并创建对话  
  ![image](https://user-images.githubusercontent.com/33772816/223603594-126b4b7a-4184-4129-bd72-fce62a90da8e.png)

