# Bridgewater Coding Test

## Problem Statement
##### Generate a Concordance 

Given an arbitrary text document written in English, write a program that will generate 
a concordance, i.e. an alphabetical list of all word occurrences, labeled with word 
frequencies. Bonus: label each word with the sentence numbers in which each occurrence appeared. 
Sample Output:

        a. a                        {2:1,1}
        b. all                      {1:1}
        c. alphabetical             {1:1}
        d. an                       {2:1,1}
        e. appeared                 {1:2}
        f. arbitrary                {1:1}
        g. bonus                    {1:2}
        h. concordance              {1:1}
        i. document                 {1:1}
        j. each                     {2:2,2}
        k. english                  {1:1}
        l. frequencies              {1:1}
        m. generate                 {1:1}
        n. given                    {1:1}
        o. i.e.                     {1:1}
        p. in                       {2:1,2}
        q. label                    {1:2}
        r. labeled                  {1:1}
        s. list                     {1:1}
        t. numbers                  {1:2}
        u. occurrence               {1:2}
        v. occurrences              {1:1}
        w. of                       {1:1}
        x. program                  {1:1}
        y. sentence                 {1:2}
        z. text                     {1:1}
        aa. that                    {1:1}
        bb. the                     {1:2}
        cc. which                   {1:2}
        dd. will                    {1:1}
        ee. with                    {2:1,2}
        ff. word                    {3:1,1,2}
        gg. write                   {1:1}
        hh. written                 {1:1}

## Setup
##### The File Structure
* Bridgewater_Test
    * README.md
    * sample.txt
    * solution.py
    * solution_tests.py

##### [Install Python](https://www.python.org/download/releases/2.7/)
1. To run the solution please use the latest version of python 2.x (download from [here](https://www.python.org/download/releases/2.7/))

2. Following this, install pip. (To do this download from this [link](https://bootstrap.pypa.io/get-pip.py) and run:
`$ python get-pip.py`

##### [Install NLTK](http://www.nltk.org/install.html)
1. Install NLTK: `run sudo pip install -U nltk`
2. Install Numpy (optional): `run sudo pip install -U numpy`
3. Test installation: run python then type `import nltk`

##### [Install NLTK Data](http://www.nltk.org/data.html)
1. Run the following lines of code within the python interpreter (pull this up by typing `python` in the command line):
`>>> import nltk`
`>>> nltk.download()`
2. This will pull up an interactive download window
3. **IMPORTANT:** Within the interactive download window navigate to **"Models"** and skim through the listings for **"punkt"**
Select **"punkt"**, refer to this [link](http://www.nltk.org/data) for what to set the Download Directory to, and finally click **"Download"**

Note: Make sure this is landing in the same python environment that will be running the solution script

##### Run solution.py
Here are the ways in which to use this script (typing in -h or help brings up these examples in the terminal):
`$ python solution.py`
`$ python solution.py 1`
`$ python solution.py sample.txt`
`$ python solution.py sample.txt 1`
`$ python solution.py [-h|help]`

If the integer 1 is provided as the first or second argument after the provided file, the "NLTK" sentence tokenizer 
is used instead of the very light weight regex one made with the following string in mind:

        Given an arbitrary text document written in English, write a program that will generate
        a concordance, i.e. an alphabetical list of all word occurrences, labeled with word
        frequencies. Bonus: label each word with the sentence numbers in which each occurrence appeared.

**Note:** As an unavoidable implementation detail, exceptions not clearly defined in the exceptions variable within 
the solve() function will have their punctuation stripped from them when using the nltk sentence tokenizer.

##### Run solution_tests.py
To run the tests cases included *part 5* of the **Process** section below run:
`python solution_tests.py`


## Process
This was my process from start to finish which is roughly about ~8 hours of work.
It was a lot of fun.
Enjoy!

#### 1. Declare assumptions
- Arbitrary "text document"
	- Could contain any set of words
	- Script must run with a file or a set of words
	- The solution must complete for at least the text given in the problem statement
- ONLY English
- Document may contain multiple lines (aka. \n)

#### 2. Interpret requirements
- The solution shall represent each word in the following format:
    
a. a                        {2:1,1}
	
- The solution shall have 28 spaces for the word index "a.",
	the word "a", a space separating these " ", and an
	appropriate # of spaces following to add up to 28
- This is followed by the {#ofOccurences:location_1,location_2,...}
	- Note: no spaces
- The solution shall list the words in alphabetical order
- The solution shall, for each unique word, provide an index that
	starts at "a. " and increment alphabetically. Upon reaching
	"z. ", the solution shall return to 'a' and increase the letter's
	repetition by 1. E.g. "z. " is followed by "aa. " followed by "bb. "
	and "zz. " is followed by "aaa. " and "bbb. "
- The solution shall group the same words together regardless of case
- e.g. "Base" and "base" will be recorded as the same word, "base"
- The final solution shall represent all words in all lowercase

- The solution shall record the particular sentence location of each word
	as well as however many times it occurs within a document

- The solution shall strip punctuation except in scenarios where the punctuation
	is a part of the word itself
	- examples: "i.e.", "e.g.", "M.D.", "etc."

- The solution shall tokenize by sentence and appropriately do so with the  
following edge cases as listed in the problem statement: 
	- latin abbreviations
		- examples: "i.e.", "e.g."
	- presence of a colon
		- example: "Bonus:"

- The solution shall accept a single text file included as an argument to the script
	- e.g. solution.py a_single_text_file.txt 

#### 3. Explore edge cases of distinguishing sentences
- Known abbreviations like these include periods:
	- "U.S." or latin abbreviations like: "i.e." and "e.g"
- Currency references like this include periods:
	- "He made $12.52 cents."
- A period isn't always followed by a whitespace:
	- It can be followed by a " or ' or '"
		- He said, "Think again."
		- He said, "'Blimey.' she said. Could you believe it?"
		- He said, "And, then she said, 'Blimey.'"
- Ellipses are sometimes terminal and sometimes not:
	- According to style guidelines, Chicago style will never
		terminate with an ellipses (even if one may be appropriate 
		to show a cut in content). MLA style, if the ellipses
		are used to show a cut of quoted content and comes
		at the end of the sentence, 4 periods are used--the
		ellipses and the period closing the sentence.
	- That said, ellipses are often used to show a cut in content
		in the middle of sentences, it needs to be stripped
		like other punctuation
	- 3 periods in a row should be stripped or disregarded
		- Texts, especially written with dialogue, may need
			require the sentence tokenizer to include a separate 
			rule that allows for an ellipses to 
			terminate a sentence
- Sentences can be concluded with ! and ? too
	 

#### 4. Identify Strategy
Since the problem of sentence tokenization is actually a rather deep one with numerous
previously implemented strategies, an Engineer should do what an engineer does
and make a decision between using a library that may have extra, unneeded bulk
that will include an additional step to follow before running the solution,
or a decision that follows only the rules and specifications given by the client.

On one hand, there's the use of the library that will, despite an extra step on setup,
future-proof the functionalities of a solution and allow more advanced uses of the
solution to be immediately available. Oh, and that goes without mentioning that
attempting to recreate the full capabilities of an already implemented sentence tokenization
library goes against the engineering principles of "Don't reinvent the wheel" and
"KISS" (Keep It Simple, Silly).

On the other hand, reducing the overhead for a solution that will only be requried
for simple, non-complex tasks can be invaluable for spreading the solution quickly.
It's especially useful when the solution is written using ONLY the exact capabilities
that are required. This way, it's trim, it's easy to read, it's fast, and it's
quick to implement.

Simply put, each has it's specific use case, and for this reason, as well as for 
the sake of this exercise, I will provide two solutions. One will use a pre-created sentence
tokenization library. The other will tokenize by sentence with simple handwritten regex
rules that keep in mind only the special cases included in the original test script.

#### 5. Setup Tests
##### 1: 
Input: A.
Output:

        a. a                        {1:1}

##### 2: 
Input: Dogs are dogs.
Output:

        a. are                      {1:1}
        b. dogs                     {2:1,1}

##### 3: 
Input: Cats are not dogs. Dogs are not cats.
Output:

        a. are                      {2:1,2}
        b. cats                     {2:1,2}
        c. dogs                     {2:1,2}
        d. not                      {2:1,2}

##### 4: 
Input: Bonus: feed the dog.
Output:

        a. bonus                    {1:1}
        b. dog                      {1:1}
        c. feed                     {1:1}
        d. the                      {1:1}

##### 5: 
Input: The dog jumps i.e. hops.
Output:

        a. dog                      {1:1}
        b. i.e.                     {1:1}
        c. jumps                    {1:1}
        d. hops                     {1:1}
        e. the                      {1:1}

##### 6:
Input: Given an arbitrary text document written in English, write a program that will generate 
a concordance, i.e. an alphabetical list of all word occurrences, labeled with word 
frequencies. Bonus: label each word with the sentence numbers in which each occurrence appeared.
Output:

        a. a                        {2:1,1}
        b. all                      {1:1}
        c. alphabetical             {1:1}
        d. an                       {2:1,1}
        e. appeared                 {1:2}
        f. arbitrary                {1:1}
        g. bonus                    {1:2}
        h. concordance              {1:1}
        i. document                 {1:1}
        j. each                     {2:2,2}
        k. english                  {1:1}
        l. frequencies              {1:1}
        m. generate                 {1:1}
        n. given                    {1:1}
        o. i.e.                     {1:1}
        p. in                       {2:1,2}
        q. label                    {1:2}
        r. labeled                  {1:1}
        s. list                     {1:1}
        t. numbers                  {1:2}
        u. occurrence               {1:2}
        v. occurrences              {1:1}
        w. of                       {1:1}
        x. program                  {1:1}
        y. sentence                 {1:2}
        z. text                     {1:1}
        aa. that                    {1:1}
        bb. the                     {1:2}
        cc. which                   {1:2}
        dd. will                    {1:1}
        ee. with                    {2:1,2}
        ff. word                    {3:1,1,2}
        gg. write                   {1:1}
        hh. written                 {1:1}

#### 6. Research/Pseudocode and solultions
I will need to engineer the two solutions such that they each reuuse as much code
as they can. I can provide a secondary command line argument that can specify which
sentence tokenization solution to use (the one that went on a diet or the library). Then,
I select the appropriate sentence tokenization function to use depending on the switch. The default
should be the one that's not the library (in case it's not setup yet to prevent the
program from not running at all). The code that can be shared, then, is as follows:

- the file input
- (sentence tokenizing is next)
- the stripping of excess punctuation and splitting into words
- the count of the words and inclusion into the dictionary (along with the
	recording of the sentence number)
- the use of the completed data structure to format each line of the output

The library solution will use and need to install NLTK, NLTK Data, and the Punkt module.

#### 7. Develop solution
##### Diffculties:
One of the oddest difficulties is that the nltk library using punkt insisted on 
splitting the sentence after i.e. The trick was to modify the known abbreviations list
of the tokenizer.

#### 8. Make deployable aka. convert for submission
**DONE**