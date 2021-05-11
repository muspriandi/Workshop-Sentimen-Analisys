# Pertemuan-2

Tools : [Jupyter Classic Notebook](https://jupyter.org/try)

Kamus Slangword & Stopword : 
- [GitHub](https://github.com/louisowen6/NLP_bahasa_resources)
- [Google Drive](https://drive.google.com/drive/folders/11dL25bqsFdID7cR0DMNorIzAANSMsQkF?usp=sharing)


## Paket Install

Gunakan paket manager [pip](https://pip.pypa.io/en/stable/) untuk install:

```bash
pip install xlrd
pip install openpyxl
pip install sastrawi

```

## Bagian 2 - Pra-Pemrosesan Data

#### 1. Inisialisasi Kebutuhan

```python
import re
from Sastrawi.Stemmer.StemmerFactory import StemmerFactory

factory = StemmerFactory()
stemmer = factory.create_stemmer()

dataFrameSlangword = pandas.read_excel('tbl_slangword -- 1725.xls')
dataFrameStopword = pandas.read_excel('stopword - 209.xls')

```

#### 2. Bagian Utama Proses Pra-Pemrosesan Data
```python
cleanText = []

for data in dataFrame['tweet']:
    
    # Casefolding
    resultText = data.lower()
    
    # Cleansing : Hapus URL, Mention, Hastag & Selain Huruf / Menghilangkan kata yang diawali dengan kata 'http', '@', '#' & selain huruf
    resultText = re.sub(r'http\S+', ' ', resultText) # hapus url
    resultText = re.sub(r'@\S+', ' ', resultText) # hapus mention
    resultText = re.sub(r'#\S+', ' ', resultText) # hapus hashtag
    resultText = re.sub(r'[^a-z ]', ' ', resultText) # hapus selain huruf a-z
    
    # Remove Whitespaces : Menghilangkan spasi/tab/baris yang kosong
    resultText = resultText.strip()
    resultText = re.sub('\s+', ' ', resultText)
    
    # Mengubah slangword
    for index in range(len(dataFrameSlangword['slangword'])):
        if dataFrameSlangword['slangword'][index] in resultText:
            resultText = re.sub(r'\b{}\b'.format(dataFrameSlangword['slangword'][index]), dataFrameSlangword['kata_asli'][index], resultText)
    
    # Menghapus slangword
    for index in range(len(dataFrameStopword['stopword'])):
        if dataFrameStopword['stopword'][index] in resultText:
            resultText = re.sub(r'\b{}\b'.format(dataFrameStopword['stopword'][index]), '', resultText)
    
    # Stemming (sastrawi)
    resultText = stemmer.stem(resultText)
    
    cleanText.append(resultText)
```

#### 3. Generatefile excel (.xlsx)
```python
dataFrame['clean text'] = cleanText

dataFrame.to_excel('preprocessing.xlsx', index=False)
```

## License
[Mus @2021](https://github.com/muspriandi/)