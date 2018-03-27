"""Reads a .txt file and displays what words are in it, how many each is used,
    and the sentence number at which every instance occurs.

Reads a .txt file called sample.txt or any other appropriately named .txt file.
The provided file has  is split into sentences, has all of its words counted,
and then listed in descending order in the format:

index. word                  {num_occurences:location_1,location_2,...}

Here are the ways in which to use this script (typing in -h or help brings up these examples in the terminal):

        $ python solution.py
        $ python solution.py 1
        $ python solution.py sample.txt
        $ python solution.py sample.txt 1
        $ python solution.py [-h|help]

If the integer 1 is provided as the first or second argument after the provided file,
the "NLTK" sentence tokenizer is used instead of the very light weight regex one
made specifically with the following string in mind:

        Given an arbitrary text document written in English, write a program that will generate
        a concordance, i.e. an alphabetical list of all word occurrences, labeled with word
        frequencies. Bonus: label each word with the sentence numbers in which each occurrence appeared.

Note: As an unavoidable implementation detail, exceptions not clearly defined in the
exceptions variable within the solve() function will have their punctuation stripped
from them when using the nltk sentence tokenizer.

Author:
    Daniel Fehrenbach
"""
import sys
import string
import re
import nltk.data


def tokenize_regex(sentences):
    """Splits the full string given into sentences according to a simple regex that works great for this simple
        usecase from Avinash Raj on stackoverflow at the following url:
        https://stackoverflow.com/questions/25735644/python-regex-for-splitting-text-into-sentences-sentence-tokenizing

    Args:
        sentences (string): The string containing multiple sentences that need splitting

    Return:
        array: Each section of the array contains a single sentence
    """
    return re.split(r'(?<=[^A-Z].[.?]) +(?=[A-Z])', sentences)


def tokenize_nltk(sentences):
    """Splits the full string given into sentences according to the nltk punkt sentence tokenizer that's trained
        against modern english literature. To help the function with latin abreviations like 'i.e.'
        they're updated into the tokenizer's known abbreviations list

    Args:
        sentences (string): The string containing multiple sentences that need splitting

    Return:
        array: Each section of the array contains a single sentence
    """
    extra_abbreviations = ['vs', 'i.e', 'e.g']
    tokenizer = nltk.data.load('tokenizers/punkt/english.pickle')
    tokenizer._params.abbrev_types.update(extra_abbreviations)
    return tokenizer.tokenize(sentences)


def add_word_to_dictionary(words_dict, word, sentence_index):
    """Helper function to add a word to the dictionary used in the final output

    Args:
        words_dict (dictionary): The dictionary containing the full data structure which is:
            {word(string): [count(int), occurrences(str)]}
        word (string): The word to be either added as a key or incremented
        sentence_index (int): The current index within the sentences array

    Return:
        None
    """
    if word not in words_dict:  # setup data structure in value
        words_dict[word] = list([1, str(sentence_index+1)])   # initialize this as a string to use concatenation
    else:
        words_dict[word][0] += 1  # [0] is the count
        words_dict[word][1] += "," + str(sentence_index+1)  # [1] is the sentence occurrences


def solve(our_string, nltk_flag):
    """Utilizes some helper functions to tokenize by sentence the given string (our_string),
        splits it into words, strips punctuation and forces lower cases,
        and counts every occurrence of every word and marks where each occurrence happened in terms of which sentence.
        Finally solve() constructs the solution string and returns it back to the main function (for testing purposes)

    Args:
        our_string (string): The string to count the words of
        nltk_flag (boolean): If the flag is true, the nltk solution is used. Otherwise the regex is used instead.

    Return:
        string: A fully formatted solution! (This is done for unit testing purposes)
    """
    # Include more exceptions here!
    exceptions = list(['i.e.'])

    # Determine which sentence tokenization implementation to use
    if nltk_flag:
        sentences = tokenize_nltk(our_string)
    else:  # the nltk flag is only sent as 0 or 1
        sentences = tokenize_regex(our_string)

    # Input, for each sentence, every word into the words_dict
    words_dict = {}
    for sentence_index, sentence in enumerate(sentences):
        sentence = sentence.lower()
        words = sentence.split()
        for word in words:
            if word not in exceptions:  # only strip punctuation if not an exception!
                word = word.translate(None, string.punctuation)
            add_word_to_dictionary(words_dict, word, sentence_index)

    # Iterate through the dictionary and display!

    index = 0
    index_repetitions = 1
    solution = ""
    for key, value in sorted(words_dict.iteritems()):
        # There are 28 specified spaces in the provided solution for item leading up to the brackets.
        # The ". " accounts for the 2 that will always take up space
        # The index_repetitions refers to this 'a.' vs 'aa.', the index takes up room in the 28 as well.
        # Finally the word itself takes up the last of the 28 slots.
        # The rest should be filled in with spaces leading to the brackets
        spaces = 28 - 2 - index_repetitions - len(key)  # There are 28 specified

        # ascii 'a' is on 97 so 97 is added to the index
        solution += chr(index+97)*index_repetitions + ". " + key + (" "*spaces) + \
            "{" + str(value[0]) + ":" + value[1] + "}"
        if not key == sorted(words_dict.keys())[-1]:
            solution += '\n'

        # the index is modded by the number of letters in the alphabet to get a number 0-25 that can be added to 97
        index = (index + 1) % 26
        if index == 0:
            index_repetitions += 1

    return solution

if __name__ == '__main__':
    read_string = ''
    use_help = ("To use, follow the following examples:\n"
                "Type -h or help after 'solution.py' to bring these examples up at any time\n\n"
                "$ python solution.py\n"
                "$ python solution.py 1\n"
                "$ python solution.py sample.txt\n"
                "$ python solution.py sample.txt 1")

    # If there's only 1 argument use default file and default regex solution
    if len(sys.argv) == 1:
        try:
            with open('sample.txt', 'r') as our_file:  # try to access the default file
                read_string = our_file.read().replace('\n', '')
            print(solve(read_string, False))
        except ValueError and IOError:
            print(use_help)

    # If there are 2 arguments, check whether it's a custom file or the use of the nltk solution
    elif len(sys.argv) == 2:
        try:
            # If it's the custom file, use it with the default regex solution
            if sys.argv[1] != '1':
                with open(sys.argv[1], 'r') as our_file:
                    read_string += our_file.read().replace('\n', '')
            print(solve(read_string, False))
        except ValueError and IOError:
            print(use_help)

        try:
            # If it's the nltk flag, use the default file and the nltk solution
            if sys.argv[1] == '1':
                with open('sample.txt', 'r') as our_file:  # try to access the default file
                    read_string = our_file.read().replace('\n', '')
                print(solve(read_string, True))
        except ValueError and IOError:
            print(use_help)

    # If there are 3 arguments, check the validity of the file and tag, and use the provided file and nltk solution
    elif len(sys.argv) == 3 and sys.argv[2] == '1':
        try:
            with open(sys.argv[1], 'r') as our_file:  # try to access the provided file
                read_string += our_file.read().replace('\n', '')
            print(solve(read_string, True))
        except ValueError and IOError:
            print(use_help)

    elif len(sys.argv) > 3:  # If there are more than 4 arguments, print the help
        print(use_help)
