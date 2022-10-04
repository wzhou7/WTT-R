# Using Google Pretrained Word Vectors in R

The Google Pretrained Word Vectors are available in a binary file. It can be downloaded here and unzip locally.

GoogleNews-vectors-negative300.bin

To load these vectors, there was an R package called rword2vec to use. It is now unmaintained.
After searching around, here's my fix based on Windows.

1. Download the .c function from `rword2vec` and save it in the working directory

URL: https://github.com/mukul13/rword2vec/blob/master/src/distance.c 

2. Compile it in RStudio

See https://stackoverflow.com/questions/15992767/create-a-dll-dynamic-library-from-c-in-r-windows

3. Call it from R

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
