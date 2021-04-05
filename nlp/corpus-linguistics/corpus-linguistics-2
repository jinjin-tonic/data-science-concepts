  
### Know what escaping is, and what characters typically require escaping
- \", \', \\, \n, \t, \v, \\. \\^ \\?, \\* \\+ etc
- You can avoid having to escape backslashes with:
	- ''' '''
	- `r""` from `re` library

![Regex for Google Analytics & Google Tag Manager - Tutorial](https://www.optimizesmart.com/wp-content/uploads/2010/06/regex-cheatsheet-for-Google-Analytics1.jpg)

### Know the meaning of major regex characters and operators discussed       
![](https://media.cheatography.com/storage/thumb/davechild_regular-expressions.750.jpg?last=1584011681)
### Be able to identify whether a given regex matches a given string, or a given description

![](https://image.slidesharecdn.com/pyregexslideshare-171125171801/95/python-regular-expressions-15-638.jpg?cb=1511630945)

### Know the right regex method for a particular situation (match, search, finditer, etc.)
1. `match`
	- Match finds the words in the beginning (force carat)
2. `search`
	- If you want to find a (single) match within a larger span of text, use `search`.
3. `finditer`
	- If you want multiple matches, use `finditer`.
- All these functions return `match objects`, which evaluate to True

### Know that match objects are the result of certain regex methods and how their group method can get the matched string or substring
1. `match.group`
	- Returns one or more subgroups of the match.
	- If you specify number, you can access to particular parts of the match. 
2. `match.start`
	- Returns the starting index of the match
3. `match.end`
	- Returns the end index of the match


### Know the syntax of XML tags (within the tag, and also how tags can be used together to form a valid XML document)
- tags should NOT contain most non-alphanumberic characters, i.e. !"#$%&'()*+,/;<=>?@[\]^`{|}~
- tags should NOT contain white space
- tags CAN contain numbers, hyphens, and periods, but not as **the first character**
- Start-tags optionally include attributes and their values 
	- `<tag attribute1="value" attribute2="value" ...> some text </tag>`

### Know what is generally accessible at a given node in a Beautiful Soup parse tree
- . You can just create a "Soup" tree by passing in a string of XML.
	- `soup = BeautifulSoup(example, "lxml")`
- Find a specific noe or node in the tree by using `find/find_all` (find matches the first element)
```python
for node in soup.find_all(["keyword", "punct"]): # look for tags
    print(node)
```
- Nodes that are returned by `find/find_all` are a special type, a `Tag`.
	- `name`: the string identifier for the tag associated with the node
	- `attrs`: a dictionary of the XML attributes, providing key/value pairs
		- You can directly access to attributes: `node["type"]`, **`soup.find_all("sent", n=["2", "3"])`**
	- `contents`: a list of the node's children in the XML document tree
	- `parent`: the node's parent in the XML document tree
- `node.get_text()` to get all the text under a particular node in the tree as a string (by default they are not strings)

### Know that webpage from the internet are in HTML, and the relationship between HTML and XML.
- HTML has a fixed set of tags, but you can make any tags for XML.

- Some HTML tags:
	-   `<html>`  The entire page is nested in this tag
	-   `<head>`  Metadata associated with the page and other miscellaneous information is stored here. No text!
	-   `<body>`  This is where everything which is displayed can be found (texts)
	-   `<div>`  This creates sections of the document (like headers, sidebars, etc.). Very important! (Breaking things up. Smaller chunks.)
	-   `<form`> Used for user input, e.g. a search bar
	-   `<a>`  An anchor, this creates links to other pages, link is in  `href`  attribute (links document to document)
	-   `<li`> A list, with bullets
	-   `<h1>`  header, number indicates size (also  `<h2>`, etc.)
	-   `<img>`  A image,  `src`  gives its location
	-   `<table>`  A table, to display data.
	-   `<p>`  a paragraph, has formating options (e.g. indentation, spacing) (bigger than span, smaller than text)
	-   `<span>`  a span of text, smaller than a paragraph, formatting options like colour highlighting
	-   `<script>`  used to load javascript (the programming language run by browsers)


### Know what sentence segmentation is and why it is easy for some language but not English.
- Breaking a text into sentences
- English is not so easy because the period (".") is ambiguous.
	- Splitting only when a period is followed by a space and then an upper case letter
		- `regex = r"(?<!Dr)\. (?=[A-Z])"`
	- Or `nltk`'s sentence splitter
		- `sent_tokenize(en_text)`

### Know what word tokenization is, why it is not trivial for English, and why it is very hard for some languages.
- Breaking a sentence up into words
- Punctuation is a problem: 's, 4.1, wasn't, ...
- `Clitics`: small words that are phonologically joined to a host word
	- ex) `n't`, `'s` - oftern treated as separate words
	- `word_tokenize(sent)` can deal with this problem

### Know what lemmatization and stemming are, how they are different, and be able to explain their (common) purpose.
- Lemmatization: converts an inflected form to a base, uninflected form
	- decreases the size of the vocabulary
	- useful if you want to calculate statistics using lexiconds, but don't want to store all the possible forms
	```python
	lemmatizer= WordNetLemmatizer()
	lemmatizer.lemmatize("kitties", "n")
	>>> 'kitty'
	```
- Stemming:strips off both inflectional and derivational mophology to reach a stem.
	- The stem is not always itself a word (cf. lemmatization)
	- Also decreases the size of the vocabulary
	- It strips off a lot more than lemmatization

	```python
	from nltk.stem.porter import PorterStemmer
	stemmer = PorterStemmer()
	stemmer.stem("kitty")
	>>> 'kitti'
	```
### Know what part-of-speech tagging is, and how that a simple POS tagger can be built from a tagged corpus/regular expressions
- Useful for doing analysis of corpora
- It can be used to focus on particular kinds of words for certain applications
- it can provide simple word sense disambiguation (e.g. the word "cross").

```python
import nltk
nltk.download('averaged_perceptron_tagger')
from nltk import pos_tag

S = "I think the NLTK pos tagger is a pretty solid tool for doing computational linguistics"
print(pos_tag(S.split(" "))) # you need to pass tokenized sentences
```
- Building a POS tagger for any languages (using morphological information which can be expressed in the form of regular expressions)
```python
from nltk import RegexpTagger, UnigramTagger
from nltk.corpus import treebank 

patterns = [(r"\d+", "CD"),(r".*ing$", "VBG"), (r".*ed$", "VBD"),(r".*s$", "NNS"),(r".*", "NN")]
sentence = ["he", "googled", "cats"]
tagged = treebank.tagged_sents()  # tags are basically from treebank tags

re_tagger= RegexpTagger(patterns)
uni_tagger= UnigramTagger(tagged, backoff=re_tagger)  # if words are not in treebank tags -> re_tagger
print(pos_tag(sentence))
print(uni_tagger.tag(sentence))  # no words for gooogled, cats ... -> re_tagger in patterns (regex)
```

### List the two ways to use the open command to open a file from disk and their pluses and minuses
### Understand the different ways to read the contents of file, and which is appropriate for a given situation
### Know what encodings are for
### Know the names of the 3 main encodings we discussed and which categories of languages they will/won't work for
### Know what can be stored using JSON
### Know two general ways to iterate over multiple files in Python
