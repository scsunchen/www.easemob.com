---
title: 环信
sidebar: iossidebar
secondnavios: true
---

# 消息 EMMessage

    消息：IM交互实体，在SDK中对应的类型是 **EMMessage**，EMMessage可以由多个符合<IEMMessageBody>协议的body组成，但是 推荐使用一个body，多个body有bug，正在优化。

以下的讲解以一个body为例：

## 文字消息构造 {#textalloc}

<pre class="hll"><code class="language-java">
EMChatText *txtChat = [[EMChatText alloc] initWithText:@"要发送的消息"];
EMTextMessageBody *body = [[EMTextMessageBody alloc] initWithChatObject:txtChat];

// 生成message
EMMessage *message = [[EMMessage alloc] initWithReceiver:@"6001" bodies:@[body]];
message.messageType = eMessageTypeChat; // 设置为单聊消息
//message.messageType = eConversationTypeGroupChat;// 设置为群聊消息
//message.messageType = eConversationTypeChatRoom;// 设置为聊天室消息
</code></pre>

## 图片消息构造 {#imagealloc}

<pre class="hll"><code class="language-java">
EMChatImage *imgChat = [[EMChatImage alloc] initWithUIImage:img displayName:@"displayName"];
EMImageMessageBody *body = [[EMImageMessageBody alloc] initWithChatObject:imgChat];

// 生成message
EMMessage *message = [[EMMessage alloc] initWithReceiver:@"6001" bodies:@[body]];
message.messageType = eMessageTypeChat; // 设置为单聊消息
//message.messageType = eConversationTypeGroupChat;// 设置为群聊消息
//message.messageType = eConversationTypeChatRoom;// 设置为聊天室消息
</code></pre>

## 位置消息构造 {#locationalloc}

<pre class="hll"><code class="language-java">
EMChatLocation *locChat = [[EMChatLocation alloc] initWithLatitude:35.1 longitude:35.1 address:@"地址"];
EMLocationMessageBody *body = [[EMLocationMessageBody alloc] initWithChatObject:locChat];

// 生成message
EMMessage *message = [[EMMessage alloc] initWithReceiver:@"6001" bodies:@[body]];
message.messageType = eMessageTypeChat; // 设置为单聊消息
//message.messageType = eConversationTypeGroupChat;// 设置为群聊消息
//message.messageType = eConversationTypeChatRoom;// 设置为聊天室消息
</code></pre>

## 语音消息构造 {#audioalloc}

<pre class="hll"><code class="language-java">
EMChatVoice *voice = [[EMChatVoice alloc] initWithFile:recordPath displayName:@"audio"];
voice.duration = aDuration;
EMVoiceMessageBody *body = [[EMVoiceMessageBody alloc] initWithChatObject:voice];

// 生成message
EMMessage *message = [[EMMessage alloc] initWithReceiver:@"6001" bodies:@[body]];
message.messageType = eMessageTypeChat; // 设置为单聊消息
//message.messageType = eConversationTypeGroupChat;// 设置为群聊消息
//message.messageType = eConversationTypeChatRoom;// 设置为聊天室消息
</code></pre>

## 视频消息构造 {#videoalloc}

<pre class="hll"><code class="language-java">
EMChatVideo *videoChat = [[EMChatVideo alloc] initWithFile:localPath displayName:@"displayName"];
EMVideoMessageBody *body = [[EMVideoMessageBody alloc] initWithChatObject:videoChat];
// 生成message
EMMessage *message = [[EMMessage alloc] initWithReceiver:@"6001" bodies:@[body]];
message.messageType = eMessageTypeChat; // 设置为单聊消息
//message.messageType = eConversationTypeGroupChat;// 设置为群聊消息
//message.messageType = eConversationTypeChatRoom;// 设置为聊天室消息
</code></pre>

## 文件消息构造 {#filealloc}

<pre class="hll"><code class="language-java">
EMChatFile *fileChat = [[EMChatFile alloc] initWithFile:localPath displayName:@"displayName"];
EMFileMessageBody *body = [[EMFileMessageBody alloc] initWithChatObject:fileChat];
// 生成message
EMMessage *message = [[EMMessage alloc] initWithReceiver:@"6001" bodies:@[body]];
message.messageType = eMessageTypeChat; // 设置为单聊消息
//message.messageType = eConversationTypeGroupChat;// 设置为群聊消息
//message.messageType = eConversationTypeChatRoom;// 设置为聊天室消息
</code></pre>

## 透传消息构造 {#cmdalloc}

SDK提供的一种特殊类型的消息，即CMD，不会存db，也不会走apns推送，类似一种指令型的消息，比如您的服务器要通知客户端做某些操作，您可以服务器和客户端提前约定好某个字段，当客户端收到约定好的字段时，执行某种特殊操作。

<pre class="hll"><code class="language-java">
EMChatCommand *cmdChat = [[EMChatCommand alloc] init];
cmdChat.cmd = @"reason";
EMCommandMessageBody *body = [[EMCommandMessageBody alloc] initWithChatObject:cmdChat];
// 生成message
EMMessage *message = [[EMMessage alloc] initWithReceiver:@"6001" bodies:@[body]];
message.messageType = eMessageTypeChat; // 设置为单聊消息
//message.messageType = eConversationTypeGroupChat;// 设置为群聊消息
//message.messageType = eConversationTypeChatRoom;// 设置为聊天室消息
</code></pre>

## 自定义消息扩展 {#ext}

> 有时候需要在消息中携带一些扩展内容，用来实现特殊需求，比如阅后即焚等。EMMessage提供了ext属性，撰文用来存放扩展内容。**ext属性是NSDictionary类型，key和value必须是基本类型，且不能是json。**
>
> 可以这样使用：EMMessage.ext = @{@"key":@"value"};

## 插入消息 {#insertMessage}

<pre class="hll"><code class="language-java">
    EMChatText *txt = [[EMChatText alloc] initWithText:@"test1"];
    EMTextMessageBody *body = [[EMTextMessageBody alloc] initWithChatObject:txt];
    EMMessage *message = [[EMMessage alloc] initWithReceiver:_conversation.chatter bodies:@[body]];
    message.messageType = eMessageTypeChat; // 设置为单聊消息
    //message.messageType = eConversationTypeGroupChat;// 设置为群聊消息
    //message.messageType = eConversationTypeChatRoom;// 设置为聊天室消息
    message.deliveryState = eMessageDeliveryState_Delivered;
    [[EaseMob sharedInstance].chatManager insertMessageToDB:message];
</code></pre>

## 更新消息属性 {#updateMessage}

<pre class="hll"><code class="language-java">
/*!
 @method
 @brief  更新消息发送状态
 @result 是否更新成功
 */
- (BOOL)updateMessageDeliveryStateToDB;

/*!
 @method
 @brief  更新消息扩展属性
 @result 是否更新成功
 */
- (BOOL)updateMessageExtToDB;

/*!
 @method
 @brief  更新消息的消息体
 @result 是否更新成功
 */
- (BOOL)updateMessageBodiesToDB;

/*!
 @method
 @brief  修改当前 message 的发送状态, 下载状态为 failed (crash 时或者 terminate)
 @return 是否更新成功
 */
- (BOOL)updateMessageStatusFailedToDB;
</code></pre>
