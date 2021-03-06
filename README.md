# StructMineDataPipeline
Data Processing Pipeline for StructMine Tools: [CoType](https://github.com/shanzhenren/CoType), [PLE](https://github.com/shanzhenren/PLE), [AFET](https://github.com/shanzhenren/AFET).

## Description
It generates the train & test json files and brown cluster file for the above three information extraction models as input files. Each line of a json file contains information of a sentence, including entity mentions, relation mentions, etc.
To generate such json files, you need to provide the following input files (we include examples in ./data folder):

### Training:
1. Freebase files (download from [here](https://drive.google.com/file/d/0B--ZKWD8ahE4aXhOLXFUeDZBVzA/view?usp=sharing) (8G) and put the unzipped freebase folder in parallel with code/ and data/ folders)
  
    * The freebase folder should contain: 
    
      freebase-facts.txt (relation triples in the format of 'id of entity 1, relation type, id of entity 2'); 
      
      freebase-mid-name.map (entity id to name map in the format of 'entity id, entity surface name');
      
      freebase-mid-type.map (entity id to type map in the format of 'entity id, entity type'). 

2. Raw training corpus file (each line as a document)

3. Entity & Relation mention target type mapping from freebase type name to target type name

### Test:
1. Raw test corpus file (each line as a document) 

## Dependencies
We will take Ubuntu for example.

* python 3
* Python library dependencies
```
$ pip install nltk
```

* [stanford coreNLP 3.9.2](http://stanfordnlp.github.io/CoreNLP/) and its [python wrapper (stanza)](https://github.com/stanfordnlp/stanza). Please put the library under `code/DataProcessorLib/'.

```
$ cd code/DataProcessorLib/
$ git clone https://github.com/stanfordnlp/stanza.git
$ cd stanza
$ pip install -e .
$ cd code/
$ wget http://nlp.stanford.edu/software/stanford-corenlp-full-2018-10-05.zip
$ unzip stanford-corenlp-full-2018-10-05.zip
```

## Example Run
Run this tool to generate Json input files for the example training and test raw corpus

```
$ java -mx4g -cp "code/stanford-corenlp-full-2018-10-05/*" edu.stanford.nlp.pipeline.StanfordCoreNLPServer
$ ./getInputJsonFile.sh  
```
Our example data files are located in ./data folder. You should be able to see these 3 files generated in the same folder - train.json, test.json and brown (brown cluster file), after running the above command.

## Parameters - getInputJsonFile.sh
Raw train & test files to run on.
```
inTrainFile='./data/documents.txt'
inTestFile='./data/test.txt'
```
Output files (input json files for CoType, PLE, AFET).
```
outTrainFile='./data/train.json'
outTestFile='./data/test.json'
bcOutFile='./data/brown'
```
Directory path of freebase files:
```
freebaseDir='./freebase'
```
Mention type(s) required to generate. You can choose to generate entity mentions only or relation mentions only or both. The parameter value can be set to 'em' or 'rm' or 'both'.
```
mentionType='both'
```
Target mention type mapping files.
```
emTypeMapFile='./data/emTypeMap.txt'
rmTypeMapFile='./data/rmTypeMap.txt' # leave it empty if only entity mention is needed
```
Parsing tool to do sentence splitting, tokenization, entity mention detection, etc. It can be 'nltk' or 'stanford'.
```
parseTool='stanford'
```
Set this parameter to be true if you already have a pretrained model and only need to generate test json file.
```
testOnly=false
```
