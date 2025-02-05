# Python3 wrapper for Stanford OpenIE
![Stanford NLP Wrapper CI](https://github.com/philipperemy/Stanford-OpenIE-Python/workflows/Stanford%20NLP%20Wrapper%20CI/badge.svg)

Open information extraction (open IE) refers to the extraction of structured relation triples from plain text, such that the schema for these relations does not need to be specified in advance. For example, Barack Obama was born in Hawaii would create a triple `(Barack Obama; was born in; Hawaii)`, corresponding to the open domain relation "was born in". CoreNLP is a Java implementation of an open IE system as described in the paper:

More information can be found here : http://nlp.stanford.edu/software/openie.html

The OpenIE library is only available in english: https://stanfordnlp.github.io/CoreNLP/human-languages.html

## Installation

You need python3 and Java installed. Java is used by the CoreNLP library.

```bash
pip install stanford_openie
```

## Example

```python
from openie import StanfordOpenIE

# https://stanfordnlp.github.io/CoreNLP/openie.html#api
# Default value of openie.affinity_probability_cap was 1/3.
properties = {
    'openie.affinity_probability_cap': 2 / 3,
}

with StanfordOpenIE(properties=properties) as client:
    text = 'Barack Obama was born in Hawaii. Richard Manning wrote this sentence.'
    print('Text: %s.' % text)
    for triple in client.annotate(text):
        print('|-', triple)

    graph_image = 'graph.png'
    client.generate_graphviz_graph(text, graph_image)
    print('Graph generated: %s.' % graph_image)

    with open('corpus/pg6130.txt', encoding='utf8') as r:
        corpus = r.read().replace('\n', ' ').replace('\r', '')

    triples_corpus = client.annotate(corpus[0:5000])
    print('Corpus: %s [...].' % corpus[0:80])
    print('Found %s triples in the corpus.' % len(triples_corpus))
    for triple in triples_corpus[:3]:
        print('|-', triple)
    print('[...]')
 ```
 
 *Expected output*
 ```
 |- {'subject': 'Barack Obama', 'relation': 'was', 'object': 'born'}
 |- {'subject': 'Barack Obama', 'relation': 'was born in', 'object': 'Hawaii'}
 |- {'subject': 'Richard Manning', 'relation': 'wrote', 'object': 'sentence'}
 Graph generated: graph.png.
 Corpus: ﻿According to this document, the city of Cumae in Ćolia, was, at an early period [...].
 Found 1664 triples in the corpus.
 |- {'subject': 'city', 'relation': 'is in', 'object': 'Ćolia'}
 |- {'subject': 'Menapolus', 'relation': 'son of', 'object': 'Ithagenes'}
 |- {'subject': 'Menapolus', 'relation': 'was Among', 'object': 'immigrants'}
 ```
 
It will generate a [GraphViz DOT](http://www.graphviz.org/) in `graph.png`:

<div align="center">
  <img src="img/out.png"><br><br>
</div>

*Note*: Make sure GraphViz is installed beforehand. Try to run the `dot` command to see if this is the case. If not, run `sudo apt-get install graphviz` if you're running on Ubuntu. 

## V1

Still available here [v1](v1).

## References

- https://www.kaggle.com/asitang/stanford-resources
- https://www.kaggle.com/geofila/corenlp?select=stanford-corenlp-full-2018-10-05

## Cite

```
@misc{StanfordOpenIEWrapper,
  author = {Philippe Remy},
  title = {Python wrapper for Stanford OpenIE},
  year = {2020},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/philipperemy/Stanford-OpenIE-Python}},
}
```
