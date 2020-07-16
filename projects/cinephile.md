# Cinephile
An app that uses IMDB data to display movie info.
A project to teach people full stack development.

## Technologies Used
- HTML
- CSS
- Javascript
- node.js
- postgres
- primsa
- jest

We need to get the IMDB dataset, load it into postgres and use node.js to create an api.
Then we will use javascript on the frontend to call those API's and display movie information to the user.

The data can be downloaded from https://datasets.imdbws.com/ and is described here: https://www.imdb.com/interfaces/.

Let's start by creating a directory called cinephile with a subdirectory called data. 
mkdir -p cinephile/data

Next lets go into the data directory so we can download the imdb data there.
We download the title basics dataset which can be downloaded using wget.
wget https://datasets.imdbws.com/title.basics.tsv.gz

unzip the file using gunzip title.basics.tsv.gz
Now we have a tsv file with basic movie data.
A tsv file is like a csv but instead of using commas to separate columns it uses tabs.

This file was half a gig when I downloaded it so I don't exactly want to open the whole file in a text editor.
Let's see how many rows this tsv has using the wc program
wc -l title.basics.tsv

I got ~ 7 million rows of data.

Let's use the head and tail commands to inspect this file, let's look at the first and last 100 rows to get an idea for what the data looks like.
head -100 title.basics.tsv
tail -100 title.basics.tsv

Documentation is also available at https://www.imdb.com/interfaces/.
I like having a copy locally so I 
wget https://www.imdb.com/interfaces/ -O data-documentation.html
This saves a html file in the data folder named data-documentation.html


Using pgloader to load a csv file

