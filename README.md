# Artificial Intelligence Agent for Connect Four
Author: Martin SAVESKI
Date: May 2009
License: MIT
***

This is a program I have developed during my undergraduate studies as a project for an introductory course in Artificial Intelligence. I particularly like this program as it is the first thing I have developed that seems to show intelligence, i.e., can kind of make its own decision, beyond what it was explicitly programmed by a human.

Connect four is a strategy board game where the player who aligns four disks wins. Program has two main components: (1) the search algorithm, and (2) the evaluation function. The evaluation function quantifies how desired or undesired a state of the board is for the agent. The search algorithm helps the agent decided on the next move by efficiently exploring what might be the consequences of each move. This program implements the Minmax algorithm with two extensions: Alpha Beta pruning — which substantially cuts the search space; and iterative deepening — which combines the advantages of depth-first and breadth-first search.

## Evaluation function
The Evaluation function is supposed to evaluate how good a given state is for the agent, i.e., how close it is to winning the game. Since, this is the agent’s view of the “real world”, I considered the design of the evaluation function as most important. Having complex evaluation function with many features results with more clever behavior of the agent, but it increases the number of calculations and time needed to evaluate each board state. As the time for each move is limited, searching with complex evaluation function cannot be performed in great depth. The trade-off between complex evaluation function and search at larger depth is obvious. After experimenting with several different models I found an approach that suited me the best in evaluation of a state. I decided to consider all possible combinations of four fields (horizontal, vertical and diagonal) and if none of the players or both of the players has selected fields in the same 4-field combination, the combination will be instantly discarded. Otherwise, it will be added to a state-score depending on the number of fields taken. Depending on the number, and if it’s agent or player fields, a certain weight will be applied. Initially, I started out with weights that were the square of the number of occupied fields in each block of four (i.e., 3-in-a-row would give a score of 9, 2 would be 4, 1=1) and if state resulted in 4 in a row it would get a big bonus/punishment of 10000/-10000. However, it quickly became evident that I would at least need to raise the punishment if the opponent had 4 in a row. In order to compare between the suitability of the weights I set up a game to allow two agents with different weights to play against each other. After trying with different weights 15-20 times, I decided that it was fairly optimized. I could imagine automating this process, perhaps using Simulated Annealing.

## Search Algorithm
My initial implementation used solely the Minimax search algorithm and could search in depth of six for around one minute. Obviously this was not fast enough, my first improvement was implementing Alpha Beta Pruning to minimize the search space. In order to improve the pruning I changed the order in which moves are considered, stating in the middle and alternating one row left and right. This resulted in tremendous speed up of the search. However, searching only to fixed depth it was hard to control the time for search to be in the given time limit. As a solution, I implemented Iterative Deepening Search, which called the Alpha Beta Pruning search with increased the depth in each iteration and returned the value of the last iteration.

## Optimizations
Obviously, to make the agent look more intelligent it had to be well optimized. Since the evaluation function is called extremely often during the search, it is the best place to start. As there are 2187 (3^7) possible combinations of lines of seven fields (729 of six fields, and 243 lines of five fields), I precomputed the evaluation values of every possible horizontal, vertical, diagonal line on the board. The evaluated values I stored in hash tables with the combinations as keys. So that the evaluation function only looks up the value for each line and adds it to the state value. The next optimization was inspired by the Iterative Deepening Search. During the search in every iteration, we know exactly what was the best move in the previous iteration, so if we consider the best row from the previous iteration first, it is very possible that it will have a high alpha/beta value and more of the search tree will be pruned. This results in decreased search space and allows search to be performed at grater depth.
***
