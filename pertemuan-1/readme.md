# Pertemuan-1

Tools : [Jupyter Classic Notebook](https://jupyter.org/try)

Kamus Slangword & Stopword : 
- [GitHub](https://github.com/louisowen6/NLP_bahasa_resources)
- [Google Drive](https://drive.google.com/drive/folders/11dL25bqsFdID7cR0DMNorIzAANSMsQkF?usp=sharing)


## Paket Install

Gunakan paket manager [pip](https://pip.pypa.io/en/stable/) untuk install:

```bash
pip install tweepy
pip install openpyxl

```

## Bagian 1 - Pengumpulan Data

#### 1. Inisialisasi API Tweepy

```python
import tweepy

auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)

api = tweepy.API(auth)
```

#### 2. Bagian Utama Proses Pengumpulan Data
```python
kataKunci = '#belajardirumah -filter:retweets'

tweets = api.search(kataKunci, lang='id', tweet_mode='extended')

id = []
text = []
created_at = []
username = []

for data in tweets:
    id.append( data._json['id'] )
    text.append( data._json['full_text'] )
    created_at.append( data._json['created_at'] )
    username.append( data._json['user']['screen_name'] )
```

#### 3. Generatefile excel (.xlsx)
```python
import pandas

dataFrame = pandas.DataFrame({
    'id': id,
    'tweet': text,
    'dibuat pada': created_at,
    'user': username
})

dataFrame.to_excel('crawling.xlsx', index=False)
```

## License
[Mus @2021](https://github.com/muspriandi/)