一个快速搭建的微信聊天机器人。
依赖于两个开源项目：

    ChatterBot 一个基于机器学习的聊天机器人
    ItChat 微信号接口

原理：
1、利用ItChat模块对微信号进行登录和消息接收的发送
2、将接收到的消息使用ChatterBot进行学习，将学习的结果作为消息来回复。

代码如下：

import itchat
from chatterbot import ChatBot
from chatterbot.trainers import ChatterBotCorpusTrainer
deepThought = ChatBot("deepThought")
deepThought.set_trainer(ChatterBotCorpusTrainer)
# 使用中文语料库训练它
deepThought.train("chatterbot.corpus.chinese") 
 
@itchat.msg_register(itchat.content.TEXT)
def text_reply(msg):
    response = deepThought.get_response(msg['Text'])
    print("from",msg['FromUserName'],msg['Text'])
    print("to",response)
    itchat.send(msg=str(response),toUserName=msg['FromUserName'])
 
itchat.auto_login(enableCmdQR=True)
itchat.run()

 

运行程序，出现微信二维码，扫码进行登录：

然后就可以进行聊天了，机器人回复的消息依赖于ChatterBot的中文语料库进行学习，初期可能会前言不搭后语，随着聊天的对话训练，语料库会越来越丰富，回答的消息也会越来越准确，当然前提是进行了正确的训练，不然，你的微信机器人可能会学坏，可能会变污…………嗯，变污…………。

一个简单的微信聊天机器人Demo就完成了，大家可以基于此进行扩展和丰富。

