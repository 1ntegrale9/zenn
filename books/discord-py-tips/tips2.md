---
title: "Botのプロフィールに「Nサーバーをプレイ中」を表示する"
---

![](https://storage.googleapis.com/zenn-user-upload/p4twctciw282uy5rfsb20cv8vrrb)

```python
import discord
from discord.ext import tasks

# discordbot が持つ各種データの箱を用意します
# ログインする(on_readyが発火する)まではデータは空(None)のままです
client = discord.Client()

# 指定間隔(60秒に1回)で実行されます
@tasks.loop(seconds=60)
async def change_activity():
    # 「○○をプレイ中」の内容を変更します
    await client.change_presence(
        activity=discord.Game(name=f'{len(client.guilds)}サーバー')
    )

# tasks.loop の開始直前に実行されます
@change_activity.before_loop
async def before_change_activity():
    # botがログインするまで(on_readyが発火するまで）待機します
    await client.wait_until_ready()


# tasks.loop を開始します
change_activity.start()

# ログイン処理を行います
client.run('THi5IsDuMMyaCCesSTOK3n00.DUMMY.ThIsi5DUMMyAcc3s5ToKen0000')
```
