# bprForBli
## Bayesian Personalized Ranking for Bilingual Lexicon Induction:

The code for the paper: Learning Translations via Matrix Completion, Wijaya et al., EMNLP 2017

Citation:

```
@inproceedings{wijaya2017learning,
  title={Learning Translations via Matrix Completion},
  author={Wijaya, Derry Tanti and Callahan, Brendan and Hewitt, John and Gao, Jie and Ling, Xiao and Apidianaki, Marianna and Callison-Burch, Chris},
  booktitle={Proceedings of the 2017 Conference on Empirical Methods in Natural Language Processing},
  pages={1452--1463},
  year={2017}
}
```

## Requirements:

java, maven, tensorflow, numpy, math, sklearn, sed

## Instructions:
1. Download [bprForBli.tar.xz](https://www.seas.upenn.edu/~derry/bprForBli.tar.xz) and unzip it
2. Change directory `cd` to librec directory
3. Install `happy coding`:
```
mvn install:install-file -Dfile=lib/happy.coding.utils-1.2.5.jar -DgroupId=happy.coding -DartifactId=utils -Dversion=1.2.5 -Dpackaging=jar
```
4. Do a clean install: 
```
mvn clean install
```
5. Gather the required files:
* English word embeddings, space separated, first line is the length and dimension, first column are words, all lower-cased e.g., [en.vec](https://www.seas.upenn.edu/~derry/en.vec)
* Foreign word embeddings, space separated, first line is the length and dimension, first column are words, all lower-cased e.g., [ko.vec](https://www.seas.upenn.edu/~derry/ko.vec)
* Bilingual dictionary, in JSON format, all lower cased e.g., [ko.json](https://www.seas.upenn.edu/~derry/ko.json)
* List of foreign words to be translated, one word per line, all lower-cased e.g., [ko.words](https://www.seas.upenn.edu/~derry/ko.words)

To generate the embedding files (\*`.vec`) for a new language, you can use `gensim`: (where filename is the file containing tokenized and lower-cased English/foreign language text)
```
import gensim, logging
logging.basicConfig(format='%(asctime)s : %(levelname)s : %(message)s', level=logging.INFO)
sentences = gensim.models.word2vec.LineSentence(filename)
model = gensim.models.Word2Vec(sentences, iter=15, negative=15)
model.wv.save_word2vec_format(fileoutname)
```
6. Execute `run.sh` with 5 arguments: 
* 2-letter language code of the foreign language
* English word embeddings file e.g., `en.vec`
* Foreign word embeddings file e.g., `ko.vec`
* Bilingual dictionary file e.g., `ko.json`
* List of foreign language words to be translated e.g., `ko.words`
```
./run.sh ko en.vec ko.vec ko.json ko.words
```

## Troubleshooting
1. If out-of-memory, modify this line in `run.sh` with higher memory requirement: e.g., -Xmx200G
```
MAVEN_OPTS="-Xmx200G" mvn exec:java -Dexec.mainClass=librec.main.LibRec -Dexec.args="-c multi/config/BPR-$lang.conf"
```
2. If you encounter problem, try running the commands in `run.sh` line by line and debugging the errors. This code has been tested on a Linux machine, but running it on other machines may cause some of the commands (e.g., `sed`) in the script to run differently. 
