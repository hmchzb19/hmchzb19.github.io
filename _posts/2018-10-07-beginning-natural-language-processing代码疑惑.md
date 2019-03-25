在123页的pos-tagging with spaCy，我电脑上运行代码的结果和书上的不一致，原因未知。

1. 运行环境  

        root@kali:~# ipython3
        Python 3.6.6 (default, Jun 27 2018, 14:44:17)
        Type "copyright", "credits" or "license" for more information.

        IPython 5.5.0 -- An enhanced Interactive Python.
        ?         -> Introduction and overview of IPython's features.
        %quickref -> Quick reference.
        help      -> Python's own help system.
        object?   -> Details about 'object', use 'object??' for extra details.

        In [16]: spacy.__version__
        Out[16]: '2.0.12'


2. 实际代码，只贴出不一致的两段代码  
书中的例子，sent_2中的refuse 在书中是'noun',  而我的结果是refuse ADJ JJ,adjective形容词。  
sent_3中,her 在书中是ADJ, 形容词,　而我的结果是her PRON PRP, pronoun代词.  
第一个fish在书中是动词verb, 而我的结果是fish NOUN NN, noun 名词。  

        In [4]: import spacy

        In [5]: nlp=spacy.load('en')

        In [6]: sent_0 = nlp('Mathiew and I went to the park.')

        In [7]: sent_1 = nlp('If Clement was asked to take out the garbage, he would ref
           ...: use.')

        In [8]: sent_2 = nlp('Baptiste was in charge of the refuse treatment center.')

        In [9]: sent_3 = nlp('Marie took out her rather suspicious and fishy cat to go f
           ...: ish for fish.')


        In [12]: for token in sent_2:
            ...: print(token.text, token.pos_, token.tag_)
            ...:
            ...:
        Baptiste PROPN NNP
        was VERB VBD
        in ADP IN
        charge NOUN NN
        of ADP IN
        the DET DT
        refuse ADJ JJ
        treatment NOUN NN
        center NOUN NN
        . PUNCT .


        In [15]: for token in sent_3:
            ...: print(token.text, token.pos_, token.tag_)
            ...:
        Marie PROPN NNP
        took VERB VBD
        out PART RP
        her PRON PRP
        rather ADV RB
        suspicious ADJ JJ
        and CCONJ CC
        fishy ADJ JJ
        cat NOUN NN
        to PART TO
        go VERB VB
        fish NOUN NN
        for ADP IN
        fish NOUN NN
        . PUNCT .

