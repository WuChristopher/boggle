# boggle
##[Programming Assignment 4: Boggle](http://coursera.cs.princeton.edu/algs4/assignments/boggle.html)

Write a program to play the word game Boggle®.

The Boggle game. Boggle is a word game designed by Allan Turoff and distributed by Hasbro. It involves a board made up of 16 cubic dice, where each die has a letter printed on each of its sides. At the beginning of the game, the 16 dice are shaken and randomly distributed into a 4-by-4 tray, with only the top sides of the dice visible. The players compete to accumulate points by building valid words out of the dice according to the following rules:

A valid word must be composed by following a sequence of adjacent dice—two dice are adjacent if they are horizontal, vertical, or diagonal neighbors.
A valid word can use each die at most once.
A valid word must contain at least 3 letters.
A valid word must be in the dictionary (which typically does not contain proper nouns).
Here are some examples of valid and invalid words:
pins	   	pines	   	dates
PINS
(valid)	   	PINES
(valid)	   	DATES
(invalid—dice not adjacent)
 
pint	   	tepee	   	sid
PINT
(invalid—path not sequential)	   	TEPEE
(invalid—die used more than once)	   	SID
(invalid—word not in dictionary)

Scoring. Words are scored according to their length, using this table:

  word length  	  points  
0–2	0
3–4	1
5	2
6	3
7	5
  8+	11
The Qu special case. In the English language, the letter Q is almost always followed by the letter U. Consequently, the side of one die is printed with the two-letter sequence Qu instead of Q (and this two-letter sequence must be used together when forming words). When scoring, Qu counts as two letters; for example, the word QuEUE scores as a 5-letter word even though it is formed by following a sequence of 4 dice.

Qu

Your task. Your challenge is to write a Boggle solver that finds all valid words in a given Boggle board, using a given dictionary. Implement an immutable data type BoggleSolver with the following API:


public class BoggleSolver
{
    // Initializes the data structure using the given array of strings as the dictionary.
    // (You can assume each word in the dictionary contains only the uppercase letters A through Z.)
    public BoggleSolver(String[] dictionary)

    // Returns the set of all valid words in the given Boggle board, as an Iterable.
    public Iterable<String> getAllValidWords(BoggleBoard board)

    // Returns the score of the given word if it is in the dictionary, zero otherwise.
    // (You can assume the word contains only the uppercase letters A through Z.)
    public int scoreOf(String word)
}

It is up to you how you search for and store the words contained in the board, as well as the dictionary used to check them. If you need help getting started, some hints are available on the checklist.

The board data type. We provide an immutable data type BoggleBoard.java for representing Boggle boards. It includes constructors for creating Boggle boards from either the 16 Hasbro dice, the distribution of letters in the English language, a file, or a character array; methods for accessing the individual letters; and a method to print out the board for debugging. Here is the full API:


public class BoggleBoard
{
    // Initializes a random 4-by-4 Boggle board.
    // (by rolling the Hasbro dice)
    public BoggleBoard()

    // Initializes a random m-by-n Boggle board.
    // (using the frequency of letters in the English language)
    public BoggleBoard(int m, int n)

    // Initializes a Boggle board from the specified filename.
    public BoggleBoard(String filename)

    // Initializes a Boggle board from the 2d char array.
    // (with 'Q' representing the two-letter sequence "Qu")
    public BoggleBoard(char[][] a)

    // Returns the number of rows.
    public int rows()

    // Returns the number of columns.
    public int cols()

    // Returns the letter in row i and column j.
    // (with 'Q' representing the two-letter sequence "Qu")
    public char getLetter(int i, int j)

    // Returns a string representation of the board.
    public String toString()
}

Testing. We provide a number of dictionary and board files for testing.

Dictionaries. A dictionary consists of a sequence of words, separated by whitespace, in alphabetical order. You can assume that each word contains only the uppercase letters A through Z. For example, here are the two files dictionary-algs4.txt and dictionary-yawl.txt:

% more dictionary-algs4.txt        % more dictionary-yawl.txt
ABACUS                             AA
ABANDON                            AAH
ABANDONED                          AAHED
ABBREVIATE                         AAHING
...                                ...
QUEUE                              PNEUMONOULTRAMICROSCOPICSILICOVOLCANOCONIOSIS
...                                ...
ZOOLOGY                            ZYZZYVAS

The former is a list of 6,013 words that appear in Algorithms 4/e; the latter is a comprehensive list of 264,061 English words (known as Yet Another Word List) that is widely used in word-game competitions.
Boggle boards. A boggle board consists of two integers M and N, followed by the M × N characters in the board, with the integers and characters separated by whitespace. You can assume the integers are nonnegative and that the characters are uppercase letters A through Z (with the two-letter sequence Qu represented as either Q or Qu). For example, here are the files board4x4.txt and board-q.txt:

% more board4x4.txt        % more board-q.txt
4 4                        4 4
A  T  E  E                 S  N  R  T
A  P  Y  O                 O  I  E  L
T  I  N  U                 E  Qu T  T
E  D  S  E                 R  S  A  T

The following test client takes the filename of a dictionary and the filename of a Boggle board as command-line arguments and prints out all valid words for the given board using the given dictionary.

public static void main(String[] args) {
    In in = new In(args[0]);
    String[] dictionary = in.readAllStrings();
    BoggleSolver solver = new BoggleSolver(dictionary);
    BoggleBoard board = new BoggleBoard(args[1]);
    int score = 0;
    for (String word : solver.getAllValidWords(board)) {
        StdOut.println(word);
        score += solver.scoreOf(word);
    }
    StdOut.println("Score = " + score);
}

Here are two sample executions:

% java BoggleSolver dictionary-algs4.txt board4x4.txt     % java BoggleSolver dictionary-algs4.txt board-q.txt
AID                                                       EQUATION
DIE                                                       EQUATIONS
END                                                       ...
ENDS                                                      QUERIES
...                                                       QUESTION
YOU                                                       QUESTIONS
Score = 33                                                ...
                                                          TRIES
                                                          Score = 84

Performance. If you choose your data structures and algorithms judiciously, your program can preprocess the dictionary and find all valid words in a random Hasbro board (or even a random 10-by-10 board) in a fraction of a second. To stress test the performance of your implementation, create one BoggleSolver object (from a given dictionary); then, repeatedly generate and solve random Hasbro boards. How many random Hasbro boards can you solve per second? For full credit, your program must be able to solve thousands of random Hasbro boards per second. The goal on this assignment is raw speed—for example, it's fine to use 10× more memory if the program is 10× faster.

Interactive game (optional, but fun and no extra work). Once you have a working version of BoggleSolver.java, download, compile, and run BoggleGame.java to play Boggle against a computer opponent. To enter a word, either type it in the text box or click the corresponding sequence of dice on the board. The computer opponent has various levels of difficulty, ranging from finding only words from popular nursery rhymes (easy) to words that appear in Algorithms 4/e (medium) to finding every valid word (humbling).

Boggle GUI
Challenge for the bored. Here are some challenges:

Find a maximum scoring 4-by-4 Hasbro board. Here is the best known board (4540 points), which was discovered by Robert McAnany in connection with this Coursera course.
Find a maximum scoring 4-by-4 Hasbro board using the Zinga list of 584,983 Italian words.
Find a minimum scoring 4-by-4 Hasbro board.
Find a maximum scoring 5-by-5 Deluxe Boggle board.
Find a maximum scoring N-by-N board (not necessarily using the Hasbro dice) for different values of N.
Find a board with the most words (or the most words that are 8 letters or longer).
Find a 4-by-4 Hasbro board that scores exactly 2,500, 3,000, 3,500, or 4,000 points.
Design an algorithm to determine whether a given 4-by-4 board can be generated by rolling the 16 Hasbro dice.
How many words in the dictionary appear in no 4-by-4 Hasbro boards?
Add new features to BoggleGame.java.
Extend your program to handle arbitrary Unicode letters and dictionaries. You may need to consider alternate algorithms and data structures.
Extend your program to handle arbitrary strings on the faces of the dice, generalizing your hack for dealing with the two-letter sequence Qu.
Unless otherwise stated, use the dictionary-yawl.txt dictionary. If you discover interesting boards, you are encouraged to share and describe them in the Discussion Forums.
Deliverables. Submit BoggleSolver.java and any other supporting files (excluding BoggleBoard.java and algs4.jar). You may not call any library functions other those in java.lang, java.util, and algs4.jar.

This assignment was developed by Matthew Drabick and Kevin Wayne, inspired by Todd Feldman and Julie Zelenski's classic Nifty Boggle assignment from Stanford. 
Copyright © 2013.

##[Programming Assignment Checklist: Boggle](http://coursera.cs.princeton.edu/algs4/checklists/boggle.html)


UNDER CONSTRUCTION


Frequently Asked Questions
If a valid word can be formed in more than one way, how many times should I return it? Only once. For example, you should return the word EQUATION only once in board-q.txt even though there are two ways to form it.

Do In need to return the words in alphabetical order? No, that is not required.

Which data structure(s) should I use to store the dictionary? It is up to you to decide. A trie is a natural starting point.

My program isn't fast enough to get 100%. What can I do to improve performance? See the Possible Optimizations section below for some ideas. It may be a challenge.

Input, Output, and Testing
The directory boggle contains some sample dictionaries and boards for testing. For convenience, boggle-testing.zip contains all of these files bundled together.

Dictionaries. Below are some dictionaries for testing. Each dictionary consists of a sequence of words containing only the uppercase letters A through Z, separated by whitespace, and in alphabetical order.

file	words	description
dictionary-nursery.txt	1,647	words that appear in several popular nursery rhymes
dictionary-algs4.txt	6,013	words that appear in Algorithms 4/e
dictionary-common.txt	20,068	a list of common English words
dictionary-shakespeare.txt	23,688	  words that appear in the complete works of William Shakespeare  
dictionary-enable2k.txt	173,528	Enhanced North American Benchmark Lexicon
dictionary-twl06.txt	178,691	Tournament Word List
dictionary-yawl.txt	264,061	Yet Another Word List
dictionary-sowpods.txt	267,751	the SOWPODS list of words
  dictionary-zingarelli2005.txt  	  584,983  	Italian Scrabble Dictionary
Boards. We provide a number of boards for testing. The boards named boards-points[xxxx].txt are Boggle board that results in a maximum score of xxxx points using the dictionary dictionary-yawl.txt. The other boards are designed to test various corner cases, including dealing with the two-letter sequence Qu and boards of dimensions other than 4-by-4.

Possible progress steps
These are purely suggestions for how you might make progress. You do not have to follow these steps.

Familiarize yourself with the BoggleBoard.java data type.
Use a standard set data type to represent the dictionary, e.g., a SET<String>, a TreeSet<String>, or a HashSet<String>.
Create the data type BoggleSolver. Write a method based on depth-first search to enumerate all strings that can be composed by following sequences of adjacent dice. That is, enumerate all simple paths in the Boggle graph (but there is no need to explicitly form the graph). For now, ignore the special two-letter sequence Qu.
Now, implement the following critical backtracking optimization: when the current path corresponds to a string that is not a prefix of any word in the dictionary, there is no need to expand the path further. To do this, you will need to create a data structure for the dictionary that supports the prefix query operation: given a prefix, is there any word in the dictionary that starts with that prefix?
Deal with the special two-letter sequence Qu.
Possible Optimizations
You will likely need to optimize some aspects of your program to pass all of the performance points (which are, intentionally, more challenging on this assignment). Here are a few ideas:

Make sure that you have implemented the critical backtracking optimization described above. This is, by far, the most important step—several orders of magnitude!
Think about whether you can implement the dictionary in a more efficient manner. Recall that the alphabet consists of only the 26 letters A through Z.
Exploit that fact that when you perform a prefix query operation, it is usually almost identical to the previous prefix query, except that it is one letter longer.
Consider a nonrecursive implementation of the prefix query operation.
Precompute the Boggle graph, i.e., the set of cubes adjacent to each cube. But don't necessarily use a heavyweight Graph object.
