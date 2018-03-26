# Spacy-BIST Parser
## Raw text parser based on Spacy and BIST parsers

The parser uses Spacy's english model for sentence breaking, tokenization and token annotations (part-of-speech, lemma, NER).
Dependency relations between tokens are extracted using BIST parser.
The BIST parser is described in [this](../bist/README.md) README file.

## Installation
**To install all dependencies, run the included [install.sh](../spacy_bist/install.sh) script.**
    
## Dependencies
- **Python 3.6.3** was used in this project
- **bist** (imported locally from `ai-lab-models/libs/bist`)
- **spacy** v2.0.5 used
- **spacy en** v2.0.0 used
- **dynet** v2.0.2 used
- **numpy** v1.14.0 used, installed during **dynet** installation
- **cython** v0.27.3 used, installed during **dynet** installation

## Usage
The module can be imported and used in python. To import, type the following:
```python
from libs.spacy_bist.parser import SpacyBISTParser
```
Note: For usage in import, the root directory `/ai-lab-models` must be added to `PYTHONPATH` environment variable.

## Training
By default, the parser uses a pre-trained BIST model (located in `pretrained/bist.model`) and Spacy's English model (`spacy en`).
To use other models, supply the path or link to each model at initialization (see example below).

For instructions on how to train these models, see:
- BIST: [README file](../bist/README.md)
- Spacy: [instructions](https://spacy.io/usage/training)

#### Example
```python
parser = SpacyBISTParser(spacy_model='/path/or/link/to/spacy/model', bist_model='/path/to/bist/model')
```

## Inference
Inference accepts document(s) either as a string of raw text or a directory of raw text files. Text must be encoded in UTF-8 format.
The parser outputs a [ParsedDocument](../utils/parsed_document.py) object for each processed document.

#### Example - Parse Files
```python
parser = SpacyBISTParser()
parsed_docs = parser.inference(input_dir='/path/to/text/files', out_dir='/output/path')
```
The `out_dir` argument is optional. When specified, each resulting `ParsedDocument`
will be saved in the output directory as a python `dict` in JSON format.

#### Example - Parse a String
```python
parser = SpacyBISTParser()
parsed_doc = parser.inference(doc_text='This is a document. It contains two sentences.')
print(parsed_doc)
```

**Output**
```json
{
    "doc_text": "First sentence. Second sentence",
    "sentences": [
        [
            {
                "start": 0,
                "len": 5,
                "pos": "JJ",
                "ner": "ORDINAL",
                "lemma": "first",
                "gov": 1,
                "rel": "amod",
                "text": "First"
            },
            {
                "start": 6,
                "len": 8,
                "pos": "NN",
                "ner": "",
                "lemma": "sentence",
                "gov": -1,
                "rel": "root",
                "text": "sentence"
            },
            {
                "start": 14,
                "len": 1,
                "pos": ".",
                "ner": "",
                "lemma": ".",
                "gov": 1,
                "rel": "punct",
                "text": "."
            }
        ],
        [
            {
                "start": 16,
                "len": 6,
                "pos": "JJ",
                "ner": "ORDINAL",
                "lemma": "second",
                "gov": 1,
                "rel": "amod",
                "text": "Second"
            },
            {
                "start": 23,
                "len": 8,
                "pos": "NN",
                "ner": "",
                "lemma": "sentence",
                "gov": -1,
                "rel": "root",
                "text": "sentence"
            }
        ]
    ]
}
```


## Citation
```
Kiperwasser, E., & Goldberg, Y. (2016). Simple and Accurate Dependency Parsing Using Bidirectional LSTM Feature Representations. 
Transactions Of The Association For Computational Linguistics, 4, 313-327. 
https://transacl.org/ojs/index.php/tacl/article/view/885/198
```