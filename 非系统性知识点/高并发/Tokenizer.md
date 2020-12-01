分词组件(Tokenizer)会做以下几件事情( 此过程称为Tokenize) ：
1. 将文档分成一个一个单独的单词。
2. 去除标点符号。
3. 去除停词(Stop word) 。

经过分词(Tokenizer) 后得到的结果称为词元(Token) 。

对于英语，语言处理组件(Linguistic Processor) 一般做以下几点：
1. 变为小写(Lowercase) 。
2. 将单词缩减为词根形式，如“cars ”到“car ”等。这种操作称为：stemming 。
3. 将单词转变为词根形式，如“drove ”到“drive ”等。这种操作称为：lemmatization 。

Stemming 和 lemmatization的异同：
- 相同之处：Stemming和lemmatization都要使词汇成为词根形式。
- 两者的方式不同：
	- Stemming采用的是“缩减”的方式：“cars”到“car”，“driving”到“drive”。
	- Lemmatization采用的是“转变”的方式：“drove”到“drove”，“driving”到“drive”。

两者的算法不同：
- Stemming主要是采取某种固定的算法来做这种缩减，如去除“s”，去除“ing”加“e”，将“ational”变为“ate”，将“tional”变为“tion”。
- Lemmatization主要是采用保存某种字典的方式做这种转变。比如字典中有“driving”到“drive”，“drove”到“drive”，“am, is, are”到“be”的映射，做转变时，只要查字典就可以了。
