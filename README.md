# Collocated Topic Modeling R Implementation

This repositoty implements the collocated topic modeling method in R. This method is described in Jung, Zhou, & Smith (2024) with an illustrative study. There is also a Python implementation available at the [WTTP](https://github.com/wzhou7/WTTP) repository.


# Citation

If you use our code in your study, please cite:

> Jung, J., Zhou, W., and Smith, A. (2024). **From Textual Data to Theoretical Insights: Introducing and Applying the Word-Text-Topic Extraction Approach.** *Organizational Research Methods (ORM)*.

or via BibTeX code:

```
@ARTICLE{jung2024textual,
  title =        {From Textual Data to Theoretical Insights: Introducing and
                  Applying the Word-Text-Topic Extraction Approach},
  author =       {Jung, Jaewoo and Zhou, Wenjun and Smith, Anne},
  journal =      {Organizational Research Methods (ORM)},
  pages =        {forthcoming},
  year =         {2024}
}
```


# Special Notes

## Using Google Pretrained Word Vectors in R

The Google Pretrained Word Vectors are available in a binary file. Since it is very large, we do not include a copy here. You can obtain it as follows:

1. Go to Google's [word2vec archive page](https://code.google.com/archive/p/word2vec/), search for "`GoogleNews-vectors-negative300.bin.gz`" to find its download [link](https://drive.google.com/file/d/0B7XkCwpI5KDYNlNUTTlSS21pQmM/edit?usp=sharing).
2. Download it and and unzip locally into your working directory. You will have a file `GoogleNews-vectors-negative300.bin`. (Note the .gz file is more than 1GB, and when extracted, the .bin file will be more than 3GB.)

One could use the [rword2vec](https://github.com/mukul13/rword2vec) package to load these pretrained vectors. However, that repository appears to be inactive. Here's my fix based on Windows.

1. Download [distance.c](https://github.com/mukul13/rword2vec/blob/master/src/distance.c) from the `rword2vec` repository and save it in your working directory.
2. Compile it in RStudio. See https://stackoverflow.com/questions/15992767/create-a-dll-dynamic-library-from-c-in-r-windows
3. Call it from R. E.g., 

```
dyn.load("distance.dll")

file_name = "GoogleNews-vectors-negative300.bin"
search_word = "good"
num=5
OUT <- .C("distance",
   rfile_name=as.character(file_name),
   search_word=as.character(search_word),
   rN=as.integer(num),
   rbestw=as.character(rep("123",num)),
   rbestd=as.double(rep(0,num)))

data=as.data.frame(cbind(OUT$rbestw,OUT$rbestd))
colnames(data)=c("word","dist")
data
```
