Tristan Cooper - A17085814
# Lab Report 3: Researching Commands

*Hello, and welcome to the Markdown page for my third CSE 15L Lab Report!*
For this report, I've chosen the `grep` command to research.

# The `Grep` Command

>For brief context, `grep` stands for: 
__G__ lobal
__R__ egular 
__E__ xpression
__P__ rint

As we've seen in lecture, `grep` can be used to search through lines to find a specific "pattern." It then returns the lines that contain the matching data.

For lab and even in the skill demonstration, we've become familiar `grep -l`. 
*Now let's take a look a more interesting command-line options!*

## 1. `grep -i`
### `-i` stands for "ignore"
- __Description:__ Turns out, the `grep` command is *case sensitive*, meaning, upper and lower case characters can produce different results! And for good reason, too. 
    - `grep -i` exists to "ignore" this case sensitive nature of `grep`
    - In short, it makes `grep` **case IN-sensitive**

1. `grep -i` __first__ example: Say we are planning to go on a trip with our dog. We would want to search all the travel guides for the where the word "dog" appears. For this example, lets just look at the *number* of times it appears in `.txt` files in `written_2/travel_guides`
    - **Without** `-i`:
        ```
        grep "dog" ./*/*/* | wc -l
        ```
        - Output:
        ```
        66
        ```
    - **With** `-i`:
        ```
        grep -i "dog" ./*/*/* | wc -l
        ```
        - Output:
        ```
        77
        ```
    - **What's happening here?**
        -  Note that `wc -l` will print the *number of lines* that the specified word is present in.
        - The first command (without `-i`) only searches for lines that contain the word "dog", all lowercase. 
        - The second command (with `-i`) searches for lines that contain the word "dog" or "Dog". 
            - Technically, it searches for "dOG", "DoG", etc.
        - To confirm that the command is working we can run:
            ```
            grep "Dog" ./*/*/* | wc -l
            ```
        And unless there are unconventional capitalizations, we would expect this command to produce the total number of lines (77) minus the lowercase number of lines (66).
        - Output:
        ```
        11
        ```
        `77-66` does, in fact, equal `11`
    - **Why is `grep -i` useful in this case?**
        - Because we, for the sake of our travels, we don't care whether the word appears at the beginning of a sentence (capitalized) or other, only that it appears. Without, `grep -i` we would be missing out on eleven whole lines that may contain important information about traveling with dogs.


2. `grep -i` __second__ example: Lets do a similar example. Say we wan't a rough estimate of the ratio of times the word "car" is at the beginning of a sentence in non-fiction books.
*These commands are being run from the `non-fiction` directory*

    - Beginning of sentence (roughly):
    ```
    grep "Car"  ./*/*/* | wc -l
    ```
    
    - Output:
    ```
    76
    ```

    - Total number of appearances:
    ```j
    grep -i "Car"  ./*/*/* | wc -l
    ```
    - Output:
    ```
    439
    ```
    - What's happening here?
        - `76` out of `439` is ~ `17%`
    - Why is `grep -i` useful in this case?
        - Although a relatively trivial shortcut, `grep -i` allows us to calculate this percentage with less commands and calculations.


>*Source:* [Geeks For Geeks](https://www.geeksforgeeks.org/grep-command-in-unixlinux/)



## 2. `grep -v`
### `-v` stands for "invert"
- __Description:__ `grep -v` inverts pattern-matching. This means that it searches for all lines the __*don't*__ contain the string.

1. `grep -v` __first__ example: Say our English major friend is doing a study on the use of the letter "e" across different authors and time periods. They want our help finding all the lines in Rybczynski's writings where this most common letter DOESN'T exist.
    - Code Block:
    ```
    grep -v "e" * | wc -l
    ```
    - Output:
    ```
    62
    ```
    - What's happening here?
        - `grep -v` found 62 lines where the letter "e" doesn't appear.

        > If we lose the `-l` after `wc`: 
        `grep -v "e" * | wc` produces `66`,
        Indicating that there are lines that contain multiple words that don't contain the letter "e"
        - **NOTE:** This command is not the  best, as it includes *empty lines*. So, unless changed, our friend is going to have some pretty crummy statistics!
            - By running the `cat` command on the right side of the pipe instead of word count, we can see that there are actually only three non-empty lines where the letter "e" never appears.

    - Why is `grep -v` useful in this case?
        -  `-v` proves extremely useful because "e" is such a common letter, so parsing through these lines on our own would be near impossible!

2. `grep -v` __second__ example: Let's revisit the dog example. Say that we are terribly allergic to cats. So much so that we don't even like the word, regardless of if it's contained within another word or not. So far, we've found all the lines that contain the word "dog". Now, let's find the number of lines that contain "dog" but don't contain the word "cat".
    - Command:
    ```
    grep -i "dog" ./*/*/* | grep -i  -v "cat" | wc -l
    ```
    - Output:
    ```
    68
    ```
    - What's happening here?
        - Of the `77` lines that contain the word "dog", `68` of them also don't include the word "cat".
    - Why is `grep -v` useful in this case?
        - By pairing `-v` with a previous command option that we learned `-i`, we are able to customize our search to include words we like, and also one's we don't.

>*Source:* [Java Revisited Blog](https://javarevisited.blogspot.com/2011/06/10-examples-of-grep-command-in-unix-and.html)


## 3. `grep -h`
### `-h` indicates "matched lines"
- __Description:__ `grep -h` allows us to display the matching lines without including the filenames/paths.
This command can be particularly helpful when paired with `cat`

1. `grep -h` __first__ example: I went ahead and found a phrase that appears in only one line in the entire `travel_guides` directory.
    - **Without** `-h`:
    ```
    grep "pilgrims were" ./*/*/* | cat
    ```
    - Output:
    ```
    ./travel_guides/berlitz1/HistoryJerusalem.txt:        barbed wire, no Israeli or Jewish pilgrims were allowed to visit the
    ```

     - **With** `-h`:
    ```
    grep -h "pilgrims were" ./*/*/* | cat
    ```
    - Output:
    ```
    barbed wire, no Israeli or Jewish pilgrims were allowed to visit the
    ```
    - What's happening here?
        - As you can see, the second output doesn't contain a file path.
    - Why is `grep -h` useful in this case?
        - This could be particularly useful if you were showing a friend who doesn't understand file paths, and you don't want them to get distracted or confused by including the path; you'd rather they just focus on the sentence you printed for them. In the next example, we'll see a far more useful example.

2. `grep -h` __second__ example: Now, as a computer science student interested in linguistics, you want to see what context the word "computers" appears in. In your half baked study, you don't want to know who wrote the lines that the word appears in or where they come from, as a single-blind measure. Here's how you can store and display all the lines that contain the word "computer", without the path or file names. 
    - Commands:
    ```
    find . | grep -h "computers" ./*/*/* > lines_using_h.txt
    cat lines_using_h.txt 
    ```
    - Output:
    ```
    cameras and computers. Those who wish to can have a quite active
    bargain prices on computers, tape-recorders, video cameras, and other
    duty-free retail businesses, especially imported cameras, computers,
    The skyline of the capital rises higher and higher each year, glass and steel replace brick and mud in the old courtyards, cars replace carts in the streets, and computers replace abacuses in the schools.
    ```
    - What's happening here?
        - We are using output redirection to store the result of using `grep -h` to search for the word "computer" then printing it out with `cat`.
    - Why is `grep -h` useful in this case?
        - Because we get to view the lines without the paths/file names.

>*Source:* [This](https://www.cyberciti.biz/faq/howto-use-grep-command-in-linux-unix/) forum



## 4. `grep -o`
### `-o` *likely* stands for "only"
- __Description:__

1. `grep -o` __first__ example:
    - Code Block:
    ```java

    ```
    - Output:
    ```java

    ```
    - What's happening here?
    - Why is `grep -i` useful in this case?

>*Source:*

2. `grep -0` __second__ example:
    - Code Block:
    ```java

    ```
    - Output:
    ```java

    ```
    - What's happening here?
    - Why is `grep -o` useful in this case?

>*Source:*







