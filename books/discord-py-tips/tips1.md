---
title: "数字の絵文字を扱う 1️⃣2️⃣3️⃣4️⃣5️⃣6️⃣7️⃣8️⃣9️⃣"
---

## 1. 絵文字を直接使う

これが一番簡単だと思います。
（Macなら Ctrl+Cmd+Space で絵文字一覧を出せます）

```python
await channel.send('1️⃣') # 絵文字のメッセージを送信
await message.reaction_add('1️⃣') # 絵文字をリアクションとして付与
if message.content == '1️⃣': # メッセージがこの絵文字の時
    pass
if str(message.reactions[0].emoji) == '1️⃣': # リアクションの最初がこの絵文字の時
    pass
```

## 2. Unicodeを利用する

R.Danny という bot に `?charinfo` というコマンドがあり、
`?charinfo 文字列` の形式でコマンドを打つと、
文字列の内の文字情報（主にUnicode）を教えてくれます。

![](https://storage.googleapis.com/zenn-user-upload/2grmjyxzln1jyscmbvz3jhhlo8xh)

これを活用して数字絵文字のUnicodeを取得しましょう。

![](https://storage.googleapis.com/zenn-user-upload/sop4lthmhzvaztm304gk3pheymk1)

しかしDiscordのメッセージは2000文字が上限のため、
このように多くの情報を要求すると弾かれます。
その場合は分割して情報を取得しましょう。

![](https://storage.googleapis.com/zenn-user-upload/jv5n6ymdmip44um6slphbdhmt5ah)

![](https://storage.googleapis.com/zenn-user-upload/8twb8d3jfzxnglozovvq96njf7rc)

![](https://storage.googleapis.com/zenn-user-upload/5kyw8bqvifdktz2571jingvz90hm)

これらより、数字絵文字のUnicodeを一般化すると以下のようになります。

```python
for n in range(1, 10, 1):
    print(f'\\U0000003{n}\\U0000fe0f\\U000020e3')
```

## 3. 絵文字名(?)を利用する

(正式名称が分かったら修正します）

Unicodeはどう頑張っても人が読むための文字列ではないので、
charinfoで取得したUnicodeの隣に明記されている名前を利用することもできます。
ただし、かなり長くなるので注意が必要です。

以下は数字のリアクションを順番に付与する例です。

```python
emoji_digits = [
    '\N{DIGIT ONE}\N{VARIATION SELECTOR-16}\N{COMBINING ENCLOSING KEYCAP}'
    '\N{DIGIT TWO}\N{VARIATION SELECTOR-16}\N{COMBINING ENCLOSING KEYCAP}'
    '\N{DIGIT THREE}\N{VARIATION SELECTOR-16}\N{COMBINING ENCLOSING KEYCAP}'
    '\N{DIGIT FOUR}\N{VARIATION SELECTOR-16}\N{COMBINING ENCLOSING KEYCAP}'
    '\N{DIGIT FIVE}\N{VARIATION SELECTOR-16}\N{COMBINING ENCLOSING KEYCAP}'
    '\N{DIGIT SIX}\N{VARIATION SELECTOR-16}\N{COMBINING ENCLOSING KEYCAP}'
    '\N{DIGIT SEVEN}\N{VARIATION SELECTOR-16}\N{COMBINING ENCLOSING KEYCAP}'
    '\N{DIGIT EIGHT}\N{VARIATION SELECTOR-16}\N{COMBINING ENCLOSING KEYCAP}'
    '\N{DIGIT NINE}\N{VARIATION SELECTOR-16}\N{COMBINING ENCLOSING KEYCAP}'
]

for digit in emoji_digits:
    await message.reaction_add(digit)
```
