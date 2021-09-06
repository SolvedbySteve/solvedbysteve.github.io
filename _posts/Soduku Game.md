---
title: "Python Sodoku Solver"
date: 2021-09-06
tags: [python, gaming, programming]
header:
  image: "/images/sodoku-header.png"
excerpt: "Python, Gaming, Programming"
mathjax: "true"
---




# Sodoku Fun

This is a very simple Python game that allows you to play Soduku and solve the game if you get overwhelmed.

## Imports
For this project, there are only two imports. The first is *pygame* which enable the game to run. The second is *time* which is used to keep track of how long the game is being played. (Refer to the bottom right-hand corner of the game board below.)

## Setting The Board

The Sodoku board in this game will have 81 squares. To win the game, each row should be filled with a number between 1 and 9. Numbers can't be repeated in a row or column.

The initial board looks like this:

<img src="{{ site.url }}{{ site.baseurl }}/images/sodoku-empty.jpg" alt="empty sodoku board">

This is what the code looks like for the board:

```python
class Grid:
    board = [
        [7, 8, 0, 4, 0, 0, 1, 2, 0],
        [6, 0, 0, 0, 7, 5, 0, 0, 9],
        [0, 0, 0, 6, 0, 1, 0, 7, 8],
        [0, 0, 7, 0, 4, 0, 2, 6, 0],
        [0, 0, 1, 0, 5, 0, 9, 3, 0],
        [9, 0, 4, 0, 6, 0, 0, 0, 5],
        [0, 7, 0, 3, 0, 0, 0, 1, 2],
        [1, 2, 0, 0, 0, 7, 4, 0, 0],
        [0, 4, 9, 2, 0, 6, 0, 0, 7]
    ]
```
*The zeros represent the empty spaces that the player will use to input their answers.*

## How To Play

In order to enter the numbers to solve the board, players will click an empty box and use the number pad to enter a number. The board will indicate the box that the player is manipulating by surrounding the box with a red outline. To delete a number, the player will simply click on the square and click ***delete*** on their keyboard. (*Hint: the backspace key won't work here.*)   The player will repeat this until all blocks are filled an and have satisfied the rules of Sodoku. If you need a refresher on the rules, refer to the **Gold Star Puzzles** instructions guide [click here](https://uploads-ssl.webflow.com/5ea38283dddc5f54a3838a7a/5eb43006f66db4625decf7d9_Classic%20Sudoku%20Instructions.pdf) .


## How To Win

There are two ways to win the game. The first is for the player to manually enter a valid (per Sodoku rules) answer for all of the empty squares without repeating numbers in rows or columns. The second method is to depress the space bar and the "solver" will complete the board for the player.

### How the solving algorithm works.

In order to solve the board, the solver.py program uses a backtracking algorithm. This is a very complex algorigthm so I'll give a high-level of how it works:

1. Picks an empty square
2. Tries all numbers (1-9)
3. Finds a number that works
4. Repeat process on empty square(s) until solution cannot be completed. (Ex: If a number is duplicated in a row or column of the board)
5. Backtrack- Erases solution and evaluates previous answers recursively until the sodoku rules have been satisfied. The solver then moves to the next row and repeats this process until the entire board is solved.



## References

The below mentioned sources were instrumental in helping me create this post. Tim Ruscica's Youtube channel is one that anyone looking to improve their Python skills should check out. You can visit his channel here: [Tech With Tim YT](https://www.youtube.com/channel/UC4JX40jDee_tINbkjycV4Sg)

Ruscica, T. (Director). (2019, April 03). Sodoku Solver Tutorial [Video file]. Retrieved September 06, 2021, from [Youtube.com](https://www.youtube.com/watch?v=eqUwSA0xI-s&amp;t=441s)

Sudoku: Backtracking-7. (2021, July 22). Retrieved September 06, 2021, from [Geeksforgeeks.org](https://www.geeksforgeeks.org/sudoku-backtracking-7/)

## Run in Gitpod

You can also run Sudoku-GUI-Solver in Gitpod, a free online dev environment for GitHub:


[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/SolvedbySteve/Sudoku-GUI-Solver/blob/master/GUI.py)