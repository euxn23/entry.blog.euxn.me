---
title: "rubyでTwitterから画像urlを取得する"
date: 2015-08-15
---

gem twitter を使用してツイートから画像の URL だけを抽出します。

```ruby:app.rb
require 'twitter'

@client = Twitter::REST::Client.new do |config|
  config.consumer_key = CONSUMER_KEY
  config.consumer_secret = CONSUMER_SECRET
  config.access_token = ACCESS_TOKEN
  config.access_token_secret = ACCESS_TOKEN_SECRET
end

tweets = @client.home_timeline(count: 200)
imgs = tweets.flat_map { |s| s.media}.map { |m| m.media_url.to_s}
```

以上のように media を取得すると**pic.twitter.com の画像しか取得できない**ため、動画の判定処理に加え、url からも画像であるか判定します。
今回は url が画像であるかどうかの判定に fastimage を使用します。
fastimage は本来画像のサイズを取得するためのものですが、(url の文字列からではなく)実際に url にアクセスしてファイル形式を判別できるため採用しました。

```ruby:app.rb
require 'twitter'
require 'fastimage'

# 省略

imgs = []
tweets = @client.home_timeline(count: 200)
imgs.concat tweets.flat_map { |s| s.media }.flat_map { |m|
              case m
              when Twitter::Media::AnimatedGif
                m.video_info.variants.map { |v| v.url.to_s }
              when Twitter::Media::Photo
                m.media_url.to_s
              else
                []
              end
            }
imgs.concat tweets.flat_map { |s| s.urls }.flat_map { |u|
              case FastImage.type(u.url.to_s)
              when :bmp, :gif, :jpeg, :png, :webm
                u.expanded_url.to_s
              else
                []
              end
            }
```

画像をファイルとして保存する場合は以下のようになります。

```ruby:app.rb
require 'open-uri'

imgs.each do |url|
  path = "./#{File.basename(url)}"
  File.open(path, 'w') do |f|
    f.write open(url).read
  end
end
```

url で動画を投稿している場合(vine とか)の処理は書いていません。画像だけでいいかな……
