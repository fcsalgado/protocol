# **Regular expressions (Regex)**

One the biggest problems that we have is keeping our data is certain order, either taking field notes or in the lab. :honeybee: :bookmark_tabs:
Additionally, sometimes our spreadsheets or annotations need to be reorganized according to new needs that arise when we are analyzing the data. We spend a lot of time doing this. Well, maybe this nightmare is going to be over, because today we'll see that these changes can be done in seconds using the beautiful **Regular expressions (Regex)**

tip: When you are copying and pasting too much is because you need some _Regex_

What do we need?

1. A text editor that support Regex, I like [Atom](https://atom.io/)
2. A text or spreadsheet to modify 
3. Keep in mind some special characters called *wildcards*

Wildcard: Special character that represent an specific variation of characters 

## How the search and replace bar work?
1. Open your text editor and type `crtl + f`. You will have a search and replace bar like this one:
![barra](https://raw.githubusercontent.com/fcsalgado/bioinformatics_urosario/master/images/barra.png)
Please activate the button `.*`

2. Using the search and replace bar transform the following scientific name `Gasteracantha cancriformis` to `G. cancriformis`. Pretty easy, right? 

3. Using the same method, transform these three scientific names 

```
 Gasteracantha cancriformis
 Macracantha arcuata
 Austracantha minax
```
This is when it starts to get tricky 

Imagine now scientific names such as `Ameiva ameiva`. Impossible!

4. Let's use our first wildcard: `\w`. This wildcard represents any number `(0-9)`, letter `(A-z)` or under score `(_)`. Explore how it works. 

5. This is not enough, we need to learn a couple of extra things. wildcards such as `\w` works really good with the `+` symbol. Let's add it to the end of the wildcard. cool, now we captured a whole word. So, if we want to capture the first letter plus the other letters, it would be like this `\w\w+`. We are almost there, we need to figure it out how the replace bar works.

6. To use the replace bar, we need to use the symbols `()` and `$`. The first one is going to capture text from the search bar, and the second one is going to let us use the terms of the search bar the way we want. It sounds weird, but let's try this:

<p>Search bar: (\w)(\w+) </p>
<p>Replace bar: $2$1 </p>

Awesome, we moved the first letter of both words. To tell Atom (or the text editor that you are using) that we just want the word that is at the begging of the line, we can use the a symbol `^` that represents _The begging of the line_. Please add this symbol to your search term, it will look like this `(^\w)(\w+)`. What this wildcard is saying is: "Capture a 1st term that is the first word at the begging of the line, and also capture a 2nd term that is the other letter of the word". 

<p>First term: (^\w) </p>
<p>Second term: (\w+) </p>

## First exercise

Using regular expressions, transform this: 
```
Gasteracantha cancriformis
Macracantha arcuata
Austracantha minax
Ameiva ameiva
``` 
into this: 
```
G. cancriformis
M. arcuata
A. minax
A. ameiva
```

<details><summary><span style="font-size: 18pt;"><strong>Answer</strong></span></summary>
    <div>
        <p>Search bar: `(^\w)(\w+)`</p>
        </p>Replace bar: `$1.`</p>
    </div>
</details>


## Second exercise

**Important**

The wildcard `\s` represents any type of space

Using regular expressions, transform this: 
```
Gasteracantha cancriformis
Macracantha arcuata
Austracantha minax
Ameiva ameiva
``` 
into this: 

```
Gasteracantha cancriformis G. cancriformis
Macracantha arcuata M. arcuata
Austracantha minax A. minax
```
<details><summary><span style="font-size: 18pt;"><strong>Answer</strong></span></summary>
    <div>
        <p>Search bar: `(^\w)(\w+)\s(\w+)`</p>
        </p>Replace bar: `$1$2 $3 $1. $3`</p>
    </div>
</details>

## Third exercise

To capture weird terms such as `,` or `(` or `[`, we use the backslash symbol, like this: `\,` 

Transform this quote:

`"What speak to the soul, escapes our measurements." (Alexander von humboldt)`

Into this quote:

`“our measurements speak to the soul” humboldt.`

<details><summary><span style="font-size: 18pt;"><strong>Answer</strong></span></summary>
    <div>
        <p>Search bar: `^(\")\w+(\s+\w+.+)\,\s+\w+\s+(\w+.+)\.\"\s+\((\w+\s+)+(\w+)\)`</p>
        </p>Replace bar: `$1$3$2$1 $5`</p>
    </div>
</details>

## Important sources of wildcards and Regex in R

[A list wildcards and Regex tips](http://practicalcomputing.org/files/PCfB_Appendices.pdf) from a book called **Practical computing for biologist** that is awesome and available in the library. You can also download it from [here](https://drive.google.com/file/d/0B65PEDolIc6oSjVpMTVtY05wVkU/view?usp=sharing) today :wink:

[Regex in base R](http://uc-r.github.io/regex)

[Regex in the Tidyverse](https://stringr.tidyverse.org/articles/regular-expressions.html)

## Real life exercise

Imagine that you download a lot of sequences from the GenBank, as the ones in this [Fasta file](https://raw.githubusercontent.com/fcsalgado/bioinformatics_urosario/master/files/16s_Aranediae.fas), they usually have a lot words:

`>MG670140.1 Actinacantha globulata isolate AGLO1 16S ribosomal RNA gene, partial sequence; mitochondrial`

which make difficult to identify species that are problematic in our alignments or make the phylogenetic tree unreadable. 

For example `>Actinacantha_globulata MG670140.1 16S` looks so much better and has all the info that we need :bowtie:

transform the format of the 216 sequences in this Fasta file [Fasta file](https://raw.githubusercontent.com/fcsalgado/bioinformatics_urosario/master/files/16s_Aranediae.fas) using Regex

<details><summary><span style="font-size: 18pt;"><strong>Answer</strong></span></summary>
    <div>
        <p>Search bar: `(\w+\.\d+)\s+(\w+)\s+(\w+)+.+(16S)+.+`</p>
        </p>Replace bar: `$2_$3 $2 $4`</p>
    </div>
</details>


## Real life exercise 2

When I created this [CSV file](https://raw.githubusercontent.com/fcsalgado/bioinformatics_urosario/master/files/exercise_file.csv) I forgot to include a column that represents the genus of each species, and now I need this column to do some calculations per genus. Using regular expressions create this column. 

<details><summary><span style="font-size: 18pt;"><strong>Answer</strong></span></summary>
    <div>
        <p>Search bar: `^(\w+)`</p>
        </p>Replace bar: `$1,$1`</p>
    </div>
</details>

## Real life exercise 3

Transform this space sepated [file](https://raw.githubusercontent.com/fcsalgado/bioinformatics_urosario/master/files/exercise_file.txt) into a CSV

<details><summary><span style="font-size: 18pt;"><strong>Answer</strong></span></summary>
    <div>
        <p>Search bar: `[ ]`</p>
        </p>Replace bar: `,`</p>
    </div>
</details>

## Real life exercise 4

You need to create an email list of students, but all you have is a [file](https://raw.githubusercontent.com/fcsalgado/bioinformatics_urosario/master/files/student_list.csv) with the names of the students and their university users. You know that the university emails follow this pattern `student_user@student.university.edu.co` and the list of emails need to be separated by `;` to create an email list. Using Regex create the email list.

<details><summary><span style="font-size: 18pt;"><strong>Answer</strong></span></summary>
    <div>
        <p>Search bar: `^(\w+\s+)+\w+\,(\w+)\s+`</p>
        </p>Replace bar: `$2@student.university.edu.co;`</p>
    </div>
</details>
