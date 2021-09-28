# Word_Guesser

**Basic Methodology**


I have basically tried to implement a model named n-gram counts. N-grams are typically used to determine the likelihood of a word following another, but in this case, I have used it to determine letters in a word.

In this, the model uses n-grams from 1 (unigram), 2 (bigram) up to 5 (five-gram). Five is the chosen cutoff because most 6 and above letter sequences begin to be just a chain of smaller sequences.

The model is trained on the provided dictionary of approximately 250,000 words. This dictionary is used to determine the n-gram frequencies. For example, the following structure for the bigram frequencies are:
• word length (n-gram frequencies depend on length of the word)
  ▪ first letter
  ▪ second letter
    o second letter frequency (this indicates how many letter1-letter2 sequences there are)

**Explanation using an example**

The game initialy returns a word consisting of underscores. The word is then fed to the model to guess the first letter. For example, "hangman" is converted to "_ _ _ _ _ _ _". Any correctly guessed letter will replace the underscore, and this new string is passed for the next guess.

This string is then passed over to find any potential n-grams. For example, a unigram can be where there is any single blank space. A bigram can be anywhere with a blank and a letter, no matter the order, meaning "letter-blank" is a bigram as is "blank-letter".
Therefore, a single blank space can be multiple n-grams. If the current status of the word is "h a n _ m a n", the blank space is a unigram, bigram (for both "n _ " and "_ m"), trigram ("a n _", "n _ m", "_ m a"), four-gram and five-gram similarly.

For each blank, a vector of probabilities for each letter not yet guessed is tabulated by
calculating the frequencies of each n-gram, with more weight given to larger n. The letter with the highest probability is taken. Also, to come up with smarter guesses, the n-gram frequencies are recalibrated to remove counts for incorrectly guessed letters. This improves the frequency values only focusing on what letters are still possible to guess. However, this process makes the code runtime much longer, and therefore the recalibration only occurs if there are less
than 3 guesses remaining. (This condition can be modified if runtime is not an issue but no greater impact was seen in practice cases regarding the accuracy of the model.)
