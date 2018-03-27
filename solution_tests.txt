import unittest
from solution import solve

class TestSolutionMethods(unittest.TestCase):
    def test_one_character(self):
        self.assertEqual(solve('A.', 0),
                         'a. a                        {1:1}')
        self.assertEqual(solve('A.', 1),
                         'a. a                        {1:1}')

    def test_multiples(self):
        self.assertEqual(solve('Dogs are dogs.', 0),
                         'a. are                      {1:1}\n'
                         'b. dogs                     {2:1,1}')
        self.assertEqual(solve('Dogs are dogs.', 1),
                         'a. are                      {1:1}\n'
                         'b. dogs                     {2:1,1}')

    def test_multiple_sentences(self):
        self.assertEqual(solve('Cats are not dogs. Dogs are not cats.', 0),
                         'a. are                      {2:1,2}\n'
                         'b. cats                     {2:1,2}\n'
                         'c. dogs                     {2:1,2}\n'
                         'd. not                      {2:1,2}')
        self.assertEqual(solve('Cats are not dogs. Dogs are not cats.', 1),
                         'a. are                      {2:1,2}\n'
                         'b. cats                     {2:1,2}\n'
                         'c. dogs                     {2:1,2}\n'
                         'd. not                      {2:1,2}')

    def test_colon(self):
        self.assertEqual(solve('Bonus: feed the dog.', 0),
                         'a. bonus                    {1:1}\n'
                         'b. dog                      {1:1}\n'
                         'c. feed                     {1:1}\n'
                         'd. the                      {1:1}')
        self.assertEqual(solve('Bonus: feed the dog.', 1),
                         'a. bonus                    {1:1}\n'
                         'b. dog                      {1:1}\n'
                         'c. feed                     {1:1}\n'
                         'd. the                      {1:1}')

    def test_latin_abbreviation(self):
        self.assertEqual(solve('The dog jumps i.e. hops.', 0),
                         'a. dog                      {1:1}\n'
                         'b. hops                     {1:1}\n'
                         'c. i.e.                     {1:1}\n'
                         'd. jumps                    {1:1}\n'
                         'e. the                      {1:1}')
        self.assertEqual(solve('The dog jumps i.e. hops.', 1),
                         'a. dog                      {1:1}\n'
                         'b. hops                     {1:1}\n'
                         'c. i.e.                     {1:1}\n'
                         'd. jumps                    {1:1}\n'
                         'e. the                      {1:1}')

    def test_provided_sample(self):
        self.assertEqual(solve('Given an arbitrary text document written in English, '
                               'write a program that will generate a concordance, i.e. '
                               'an alphabetical list of all word occurrences, labeled '
                               'with word frequencies. Bonus: label each word with the '
                               'sentence numbers in which each occurrence appeared.', 0),
                         'a. a                        {2:1,1}\n'
                         'b. all                      {1:1}\n'
                         'c. alphabetical             {1:1}\n'
                         'd. an                       {2:1,1}\n'
                         'e. appeared                 {1:2}\n'
                         'f. arbitrary                {1:1}\n'
                         'g. bonus                    {1:2}\n'
                         'h. concordance              {1:1}\n'
                         'i. document                 {1:1}\n'
                         'j. each                     {2:2,2}\n'
                         'k. english                  {1:1}\n'
                         'l. frequencies              {1:1}\n'
                         'm. generate                 {1:1}\n'
                         'n. given                    {1:1}\n'
                         'o. i.e.                     {1:1}\n'
                         'p. in                       {2:1,2}\n'
                         'q. label                    {1:2}\n'
                         'r. labeled                  {1:1}\n'
                         's. list                     {1:1}\n'
                         't. numbers                  {1:2}\n'
                         'u. occurrence               {1:2}\n'
                         'v. occurrences              {1:1}\n'
                         'w. of                       {1:1}\n'
                         'x. program                  {1:1}\n'
                         'y. sentence                 {1:2}\n'
                         'z. text                     {1:1}\n'
                         'aa. that                    {1:1}\n'
                         'bb. the                     {1:2}\n'
                         'cc. which                   {1:2}\n'
                         'dd. will                    {1:1}\n'
                         'ee. with                    {2:1,2}\n'
                         'ff. word                    {3:1,1,2}\n'
                         'gg. write                   {1:1}\n'
                         'hh. written                 {1:1}')
        self.assertEqual(solve('Given an arbitrary text document written in English, '
                               'write a program that will generate a concordance, i.e. '
                               'an alphabetical list of all word occurrences, labeled '
                               'with word frequencies. Bonus: label each word with the '
                               'sentence numbers in which each occurrence appeared.', 1),
                         'a. a                        {2:1,1}\n'
                         'b. all                      {1:1}\n'
                         'c. alphabetical             {1:1}\n'
                         'd. an                       {2:1,1}\n'
                         'e. appeared                 {1:2}\n'
                         'f. arbitrary                {1:1}\n'
                         'g. bonus                    {1:2}\n'
                         'h. concordance              {1:1}\n'
                         'i. document                 {1:1}\n'
                         'j. each                     {2:2,2}\n'
                         'k. english                  {1:1}\n'
                         'l. frequencies              {1:1}\n'
                         'm. generate                 {1:1}\n'
                         'n. given                    {1:1}\n'
                         'o. i.e.                     {1:1}\n'
                         'p. in                       {2:1,2}\n'
                         'q. label                    {1:2}\n'
                         'r. labeled                  {1:1}\n'
                         's. list                     {1:1}\n'
                         't. numbers                  {1:2}\n'
                         'u. occurrence               {1:2}\n'
                         'v. occurrences              {1:1}\n'
                         'w. of                       {1:1}\n'
                         'x. program                  {1:1}\n'
                         'y. sentence                 {1:2}\n'
                         'z. text                     {1:1}\n'
                         'aa. that                    {1:1}\n'
                         'bb. the                     {1:2}\n'
                         'cc. which                   {1:2}\n'
                         'dd. will                    {1:1}\n'
                         'ee. with                    {2:1,2}\n'
                         'ff. word                    {3:1,1,2}\n'
                         'gg. write                   {1:1}\n'
                         'hh. written                 {1:1}')

if __name__ == '__main__':
    unittest.main()
