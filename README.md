**Run:**
```python
>>> python Hangman.py
```
**Run and show flow graph:**
```python
>>> pycallgraph graphviz —- ./Hangman.py
```
### Basic Methodology

I have basically tried to implement a model named n-gram counts. N-grams are typically used to determine the likelihood of a word following another, but in this case, I have used it to determine letters in a word. In this, the model uses n-grams from 1 (unigram), 2 (bigram) up to 5 (five-gram). Five is
the chosen cutoff because most 6 and above letter sequences begin to be just a chain of smaller sequences.

The model is trained on the provided dictionary of approximately 250,000 words. This dictionary is used to determine the n-gram frequencies. For example, the following structure for the bigram frequencies are:
• word length (n-gram frequencies depend on length of the word)
  ▪ first letter
  ▪ second letter
    o second letter frequency (this indicates how many letter1-letter2 sequences there are)

### Explanation using an example

The game is started by calling the hangman api. It returns a word consisting of underscores. The word is then fed to the model to guess the first letter. For example, "hangman" is converted to "_ _ _ _ _ _ _". Any correctly guessed letter will replace the underscore, and this new string is passed for the next guess.
This string is then passed over to find any potential n-grams. For example, a unigram can be where there is any single blank space. A bigram can be anywhere with a blank and a letter, no matter the order, meaning "letter-blank" is a bigram as is "blank-letter".

Therefore, a single blank space can be multiple n-grams. If the current status of the word is "h a n _ m a n", the blank space is a unigram, bigram (for both "n _ " and "_ m"), trigram ("a n _", "n _ m", "_ m a"), four-gram and five-gram similarly. For each blank, a vector of probabilities for each letter not yet guessed is tabulated by calculating the frequencies of each n-gram, with more weight given to larger n. The letter with the highest probability is taken.

Also, to come up with smarter guesses, the n-gram frequencies are recalibrated to remove counts for incorrectly guessed letters. This improves the frequency values only focusing on what letters are still possible to guess. However, this process makes the code runtime much longer, and therefore the recalibration only occurs if there are less than 3 guesses remaining. (This condition can be modified if runtime is not an issue but no greater impact was seen in practice cases regarding the accuracy of the model.)

### Probability of Success on 1,000,000 Commonly Used Sentences:
```python
>>> python TestAverage.py
Tests Done: 1000000/1000000
Average: 98.5602%
```
### Probability of Success on 1,000,000 Randomly Generated Input (Extreme Cases):
```python
>>> python TestAverage.py
Tests Done: 1000000/1000000
Average: 92.5692%
```
### Sample Input
```
1 : Generate input for me
2 : Enter custom input
Type 'out' to exit program
--> 1
Generated Input: pardy stoutheartedly valetudinarians cusp portion
_____ ______________ _______________ ____ _______

Guess: e
_____ ______e___e___ ___e___________ ____ _______

Guess: s
_____ s_____e___e___ ___e__________s __s_ _______

Guess: y
____y s_____e___e__y ___e__________s __s_ _______

Guess: u
____y s__u__e___e__y ___e_u________s _us_ _______

Guess: t
____y st_ut_e__te__y ___etu________s _us_ ___t___

Guess: v
____y st_ut_e__te__y v__etu________s _us_ ___t___

Guess: r
__r_y st_ut_e_rte__y v__etu____r___s _us_ __rt___

Guess: o
__r_y stout_e_rte__y v__etu____r___s _us_ _ort_o_

Guess: p
p_r_y stout_e_rte__y v__etu____r___s _usp port_o_

Guess: n
p_r_y stout_e_rte__y v__etu__n_r__ns _usp port_on

Guess: l
p_r_y stout_e_rte_ly v_letu__n_r__ns _usp port_on

Guess: i
p_r_y stout_e_rte_ly v_letu_in_ri_ns _usp portion

Guess: h
p_r_y stouthe_rte_ly v_letu_in_ri_ns _usp portion

Guess: d
p_rdy stouthe_rtedly v_letudin_ri_ns _usp portion

Guess: c
p_rdy stouthe_rtedly v_letudin_ri_ns cusp portion

Guess: a
pardy stoutheartedly valetudinarians cusp portion

ALL COMPLETED! SOLVED!

TRIES LEFT: 3 out of 3
```
#### Flow Diagram for input above:
![alt text](https://raw.githubusercontent.com/endeavors/HangmanAI/master/hangman_graph.png)
