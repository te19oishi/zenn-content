---
title: "PythonのSeleniumでInstagramの投稿画像をスクレイピングした"
emoji: "📷"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Python, Selenium, Scraping, Instagram]
published: false
---

## 1. 概要

先日地元で行われたハッカソンでインスタグラムの画像をスクレイピングしてURLのリストとして出力したのでメモとして残しておきます。

## 2. 環境

- MacBook Air(M1)
- macOS Ventura version13.1

## 3. 結論

### 3.1. コード

取得したい画像の枚数が2枚以下ならクリックする必要がないのでより早く短いコードになります。

```python:scraping.py

from time import sleep
from selenium import webdriver
# seleniumが最新版なら必要
#from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service

MAX_POST_SIZE = 4

driver_path = chromedriverのパス # "/Users/ユーザーネーム/ディレクトリ名/chromedriver_mac_arm64/chromedriver"
service = Service(executable_path=driver_path)
driver = webdriver.Chrome(service=service)

# post = 1
# driver.get("https://instagram.com/p/Cpm53N4ybXQ/")

# post = 2
# driver.get("https://www.instagram.com/p/Cpp1gWwvw5q/")

# post = 3
# driver.get("https://www.instagram.com/p/Cpm4Wy8voaC/")

# post = 4
# driver.get("https://www.instagram.com/p/CpZ96mNvM6Q/")

# post >= 5
driver.get("https://www.instagram.com/p/Cphqarrr7GL/")

sleep(12)

# 画像(MAX_POST_SIZE)枚までを取得
post_size = len(driver.find_elements_by_css_selector("div._acnb"))
click_count = MAX_POST_SIZE-2 if post_size >= MAX_POST_SIZE-1 else 1
images = []

# 画像が複数あれば指定回数クリック
if post_size > 1:
    next = driver.find_element_by_css_selector("button[aria-label='次へ']")
    for i in range(click_count):
        next.click()

# imgのurlを取得
img = driver.find_elements_by_css_selector("img[decoding='auto']")

# imgをリスト化
for j in img:
    srcset = j.get_attribute('srcset')
    img_url = srcset[:srcset.find(" 640w,https://")]
    images.append(img_url)

print(images)

```

### 3.2. 結果

例として画像が5枚以上あるものの実行結果を記載します。

問題なく4枚まで出力することができました。

#### 3.2.1. 投稿画像5枚以上

```:terminal
['https://instagram.ffuk2-1.fna.fbcdn.net/v/t51.2885-15/333611737_534811062116354_811929644859076739_n.jpg?stp=dst-jpg_e35_s640x640_sh0.08&_nc_ht=instagram.ffuk2-1.fna.fbcdn.net&_nc_cat=102&_nc_ohc=7v99iTCS6BsAX9hDXoj&edm=AP_V10EBAAAA&ccb=7-5&oh=00_AfC6lDI0c3dN8H-ZLzpCZLsEcZ5Oc5DU-UnQEdbd25C3jA&oe=64154BB3&_nc_sid=4f375e', 
'https://instagram.ffuk2-1.fna.fbcdn.net/v/t51.2885-15/332607874_1208963356674542_9106270431600679415_n.jpg?stp=dst-jpg_e35_s640x640_sh0.08&_nc_ht=instagram.ffuk2-1.fna.fbcdn.net&_nc_cat=102&_nc_ohc=TbNrJt3SsowAX-1UM4r&edm=AP_V10EBAAAA&ccb=7-5&oh=00_AfBAD7uZ_gV0idgEA3WlTRR-RAuDR8VmLt18v1Npqx9ehw&oe=6415F54F&_nc_sid=4f375e', 
'https://instagram.ffuk2-1.fna.fbcdn.net/v/t51.2885-15/332289846_856335152131851_2304385560818269389_n.jpg?stp=dst-jpg_e35_s640x640_sh0.08&_nc_ht=instagram.ffuk2-1.fna.fbcdn.net&_nc_cat=103&_nc_ohc=aNoXGct_u3cAX_PMzcV&edm=AP_V10EBAAAA&ccb=7-5&oh=00_AfCnBJUcFGier6TfMz4fUFt3YHioS0mpmTtNKwaEKLLkIA&oe=6414E1E3&_nc_sid=4f375e', 
'https://instagram.ffuk2-1.fna.fbcdn.net/v/t51.2885-15/334613049_1021919285862447_8118206522581362204_n.jpg?stp=dst-jpg_e35_s640x640_sh0.08&_nc_ht=instagram.ffuk2-1.fna.fbcdn.net&_nc_cat=103&_nc_ohc=fkAM32Ai128AX_latZ6&edm=AP_V10EBAAAA&ccb=7-5&oh=00_AfBuyeJolFZivNQS-ybqFfnNVgEq9MQLu4X9E4s-PZSt9A&oe=6415038D&_nc_sid=4f375e']
```
ちなみにこちらの投稿の1枚目から4枚目です。

https://www.instagram.com/p/Cphqarrr7GL/

## 4. 手順

今回私はローカルでWebDriverというものをインストールしましたが、dockerで環境構築する方法が簡単らしいです。
https://github.com/SeleniumHQ/docker-selenium

### 4.1. 必要なもの

- Google Chrome
- WebDriver
- Selenium

#### 4.1.1. WebDriver
WebDriverをインストールしたいのですが、その為には自分が使用しているGoogleChromeのバージョンを確認し一致するものをインストールする必要があります。

##### 4.1.1.1. GoogleChromeのバージョン確認
画面右上の「メニュー」ボタンをクリックします。
![](/images/62ddcaadca0849/2023-03-14-22-29-18.png)

「設定」をクリックします。
![](/images/62ddcaadca0849/2023-03-14-22-36-02.png)

「Chromeについて」にバージョンの記載がありました。
![](/images/62ddcaadca0849/2023-03-14-22-38-01.png)

#### 4.1.2. WebDriverをダウンロード
ページは以下の通りです。

https://chromedriver.chromium.org/downloads

ここから先ほど記載のあったバージョンを探しダウンロードします。

:::message alert
ダウンロード先はこれから開発するディレクトリにすることをお勧めします。後ほどパスで指定する必要があります。
:::

![](/images/62ddcaadca0849/2023-03-14-22-42-53.png)

#### 4.1.3. Seleniumをインストール

ターミナルで以下のコマンドを使用してSeleniumをインストールします。

```:terminal
pip install selenium==3.2
```

以上です。

SeleniumはJSを操作できる分動作が遅かったり今回はバージョンが低いものを使用している影響で実行時に警告が出またりしますが問題なく動作します。
最新版だと記述方法が変わってきます。また、Chrome用のオプションも使用することができるらしくより便利に使用することができそうです。JSを操作する必要がない場合はbeautifulSoupとかもあるようなのでそちらを利用した方が早く簡単だと思われます。

今後インスタグラムのUI等が変更されたらこの方法が通用しなくなる可能性は十分あります。
最後に、スクレイピングする際は利用規約に十分注意して行うようにしてください。

## 5. 関連リンク
- 公式ドキュメント
  - https://www.selenium.dev/ja/documentation/ 
