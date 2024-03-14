+++
title = "Explanation and simulation of the Monty Hall (Three Door) Problem"
date = 2021-03-07

[taxonomies]
tags = ["Probability", "Python"]
+++


The Monty Hall problem (also known as the Three door problem) concerns what decision to take in the following scenario: There are three doors, with a prize behind one of them. You get to choose one door, then one of the other doors which doesn't have the prize behind it is opened. And now you have to choose whether you want to change door to the one that is left closed (which you didn't select in the beginning) or keep the door which you selected in the beginning. What do you do?
<!-- more -->

Assuming that the prize is placed at random and that the host chooses to open a door at random if possible, the correct choice is to switch doors since you'll have a chance of 2/3 to be correct. When this solution to the problem (based on American TV-show Let's Make a Deal, hosted by Monty Hall) was presented by statistician Marilyn vos Savant, it was hard to accept for a substantial amount of people due to its counterintuitive nature.

Fortunately, it's easy to practically test it, or better yet simulate it. But first, the reasoning for choosing to switch: When you first select doors, you'll have a 1/3 chance to get it right. If you were to switch door before one of the others has been opened, there will still be a 1/3 to get it right, since there are two doors to choose from (and all of the three doors have an equal probability to be correct). After one of them has been opened, the probability for each of the initial doors you didn't choose to be correct (1/3) is combined, since there is now only one door left but the probability of the choices you didn't make to be correct is still 2/3.

When simulating this, the result is as expected. I wrote a short script in Python simulating this scenario: A prize is put behind a random door (out of three possible), a random door is "guessed" to be the winning door, a door that neither contains the prize behind it nor was initially chosen as the winning door is opened (which is random if two doors are selectable), and then the chosen door is switched to the one remaining. The number of times it got it right in one batch of simulations was 66’488 out of 100’000 (66.49 %).

Of course, as you get convinced that the reasoning above is correct, a lot of the code that I wrote seems redundant, since the only thing that matters in the end is this:
* Do you want to switch doors in the end? Then you need to select the incorrect door in the beginning.
* Do you not want to switch doors in the end? Then you need to select the correct door in the beginning.
# Source

The code for the script can be found [here](https://github.com/vcrn/monty-hall-problem-simulator).

