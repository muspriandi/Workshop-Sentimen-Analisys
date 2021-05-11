# Pertemuan-3

Tools : [Jupyter Classic Notebook](https://jupyter.org/try)

Kamus Kata Positif & Kata Negatif : 
- [GitHub](https://github.com/louisowen6/NLP_bahasa_resources)
- [Google Drive](https://www.google.com)


## Paket Install

Gunakan paket manager [pip](https://pip.pypa.io/en/stable/) untuk install:

```bash
pip install xlrd

```

## Bagian 3 - Pelabelan Data

#### 1. Inisialisasi Kebutuhan

```python
dataFrameKataPositif = pandas.read_excel('tbl_lexicon_positive - 1105.xls')
dataFrameKataNegatif = pandas.read_excel('tbl_lexicon_negative - 2240.xls')

```

#### 2. Bagian Utama Proses Pelabelan Data
```python
labelType = []

for tweet in dataFrame['tweet']:
    
	jumlahPositif = 0
	jumlahNegatif = 0
	skor = 0
	label = ''
	
	for kataTweet in tweet.split():		# tokenizing
	
		# Hitung jumlah kata positif
		for kataPositif in dataFrameKataPositif['positive_word']:	# loop data kata positif
			if kataTweet == kataPositif:
				jumlahPositif += 1
				break
		
		# Hitung jumlah kata negatif
		for kataNegatif in dataFrameKataNegatif['negative_word']:	# loop data kata negatif
			if kataTweet == kataNegatif:
				jumlahNegatif += 1
				break
	
	skor = jumlahPositif - jumlahNegatif
	
	if skor > 0:
		label = 'POSITIF'
	elif skor < 0:
		label = 'NEGATIF'
	else:	# skor == 0	
		label = 'NETRAL'
	
    labelType.append( label )
```

#### 3. Generatefile excel (.xlsx)
```python
dataFrame['label sentimen'] = labelType

dataFrame.to_excel('labeling.xlsx', index=False)
```

## License
[Mus @2021](https://github.com/muspriandi/)
