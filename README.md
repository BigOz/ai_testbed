# ai_testbed

## Abstract

This project is intended to explore the question “Why does AI struggle to compete against humans in certain games?” In a timeline of when machines dominated humans at different games, similar-appearing games can appear sometimes decades apart.

Consider the games Reversi and Checkers:

||Reversi|Checkers|
|---|---|---|
|Board Size|8x8|8x8|
|Number of Pieces|up to 64 finishing|24 starting|
|Number of Legal Positions|10^28|10^20|
|Year AI defeated World Champion|1980|1994|
|Year all moves had been played out|NA|2007|

The first three points suggest Reversi is the more complicated game, but AI was able to defeat the World Champion over a decade before the AI for Checkers

## Method

Monte Carlo Tree Search (MCTS) was chosen as the AI algorithm to test. Two platforms were built to test MCTS in competition. The Monte Carlo game-playing agent was designed to allow specific limitations to be imposed on a player. Multiple competitions were carried out between the handicapped Monte Carlo player and the perfect player, with varying parameters set for the handicapped player. Both platforms use the same MCTS algorithm with slight modifications for the unique rules of each game.

Using the platform, three experiments were undertaken to explore the capabilities of AI in perfect-information 8x8 games:

- Experiment 1: Limit Time and Number of Permitted Simulations in both games
- Experiment 2: Attempt to determine how many resources MCTS needs to succeed in each game
- Experiment 3: Examine the success of Alpha-Beta Pruning when MCTS fails to perform

## Monte Carlo Tree Search Explained

Monte Carlo Tree Search is typically broken down into 4 steps:

- Selection: From the current game state, use some sort of heuristic to select a potentially promising move
- Expansion: From the chosen move, select another move is under-explored
- Simulation: From that under-explored move, play out random moves until game completion
- Backpropagation: Work back towards the original game state, updating each relevant game state with the result of the random playou

## Experiment 1: Limit Time and Number of Permitted Simulations

A series of tests were established and all tests were run on each game. For each data point, a handicapped player competes against a perfect player for 10 games. The perfect player is given 1 second to choose a move with no limit in simulations. The data point represents the percentage of time the handicapped player won matches. Draws are considered a loss for both agents.

![Alt text](.images/ex_1_reversi_2.png?raw=true "Experiment 1: Reversi Results")

![Alt text](.images/ex_1_reversi_2.png?raw=true "Experiment 1: Checkers Results")

At first glance, it would seem the results are predictable – as an agent is given more time and permitted more simulations, their win-ratio increases.
However, when a similar test is performed with a Monte Carlo agent competing against a random-choice agent 25 times, a different conclusion emerges.

||Reversi|Checkers|
|---|---|---|
|Win Frequency Against a Random Agent|100%||60%|

This result shows that while the Reversi Monte Carlo agent is better than random guessing, winning every match, the Checkers Monte Carlo agent only appears to win 15 of the 25 matches, which in practice is no better than random guessing.

Thus, the above table for Checkers cannot be entirely trusted – the results only reflect a handicapped agent facing off against a not-very-good agent.

## Experiment 2: Testing MCTS in Checkers

From the results of experiment 1, a theory emerged that a Monte Carlo agent in Checkers needs more resources to be successful against a random agent. An experiment was set up to test this theory.

For this new experiment, the Checkers Monte Carlo agent was set up to compete against a random-move agent. Each data point in the table below represents the percentage of matches won with increasing amount of time allotted the Monte Carlo agent to choose a move. For these matches, no limit was put on the number of simulations permitted.

![Alt text](.images/ex_2_2.png?raw=true "Experiment 2: Checkers Monte Carlo vs Random Agent Results")

This table suggests that even when allotted 150 seconds to choose a move, the Checkers Monte Carlo agent does no better than random guessing.

## Experiment 3: Study of an Alpha-Beta Pruning Agent

As a final experiment, an experiment was set up to test another algorithm commonly used in perfect-information, 2-player games like Checkers – Alpha Beta Pruning.

In this experiment, the Alpha-Beta Pruning agent was limited to a search depth of 9, which equated to approximately 9 seconds per move. The data represents the percentage of wins in 25 matches.

|Checkers|Monte Carlo|Alpha-Beta Pruning|
|---|---|---|
|Win Frequency Against a Random Agent|60%|80%|

While not perfect, the data suggest the Alpha-Beta Pruning agent does better than randomly guessing. In addition, the heuristic used by the Alpha-Beta Pruning agent is to simply compare the number of agent-controlled pieces – the number of opponent-controlled pieces. A more thorough heuristic may make the agent even more effective.

## Conclusion

After analyzing the data, the conclusion is that AI sometimes struggles to compete against humans because the rules of some games make gameplay decisions more complex.

In Reversi, there are at most 64 possible moves, meaning a Monte Carlo algorithm only has to search 64 moves deep during simulation. In addition, victory is easily decided after 64 rounds by determining who has the most pieces on the board.

Checkers is more complex. Although you only use half the board, there is more to think about. Components like forced jumps, kings, and sacrificing pieces all combine to accentuate the consequences of each move. In addition, the number of rounds is potentially infinite, and total domination is required for a victory. The increased search-depth leads to more branching and less confidence in each chosen move.

## Further Study

One area that may yield interesting results would be studying more about what techniques and algorithms are used to implement AI for similarly-complex games such as Checkers, Go and Chess.

Another area to explore would be to further study what kind of resources a Monte Carlo agent would need to be competitive in Checkers. Perhaps there are certain parameters that could be tweaked to get better results.

In addition, the current Monte Carlo agent makes decisions based purely on wins. Perhaps a more thorough heruistic with rewards for capturing men and penalties for losing men would be better, particularly in reducing the required search depth.
