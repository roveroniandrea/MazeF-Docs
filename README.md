# MazeF#
Authors: **M. Checchin**, **L. Fasolato** and **A. Roveroni**

## Maze generation algorithm
In this project, we modified the *recursive backtracking* algorithm to implement maze generation. The basic algorithm is explained [here](https://en.wikipedia.org/wiki/Maze_generation_algorithm#Recursive_backtracker).

The main difference rely on the way we draw the walls of the maze. The classical algorithm draws lines between cells, but because of ASCII graphics, we had to rapresent a wall as a character in a cell. So, instead of having four boolean properties to define if a cell has walls around it, we have chosen to rapresent a wall as a cell. So, our `MazeCell` class has a property, called `isWall`, that specify if a cell has to be considered a wall or not. Due to this, we had to modify the original algorithm in order to avoid some ugly generations like two adiacent corridors.

In order to do this, we added another property to our `MazeCell` class called `isBlocked`. If it's set to true, the cell is marked as currently ignored by the backtracking algorithm. A blocked cell, if not unblocked, is considered wall at the end of the generation. Our backtracking algorithm has some priorities on which cell has to be explored next:
1. First, an unvisited and not blocked cell has highest priority
2. If none of adiacent cells meets the first requirement, the algorithm searches for unvisited but blocked cells. In order to avoid adiacent corridors, the algorithm can unblock and proceed to this cell only if it has no more than 1 corridor adiacent (i.e. the corridor the backtracker is walking on)
3. If no cells can be unblocked, the backtracker turns back by one position like in the original algorithm

Another modification of the algorithm consists in forcing it to maintain the same direction for a certain number of steps to better control the length of corridors.

### Multi path generation
The recursive backtracker only generates perfect mazes, which have only one path to the exit. We have chosen to implement a function that tears down some walls (if some conditions are met) to generate multiple solutions and add some difficulty to the game.

## Solution Finder
To find the best solution for our multi-path maze, we implemented the [Dijkstra's algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm).

## Different gamemodes
There are three different gamemodes:
1. **Easy** Where the player has to reach the exit of an entirely visible maze
2. **Blind** In this mode we simulate that the player has a torch in his hand. The maze is compleately dark and player can light only a small area around him
3. **Timed** The exit door stays open only for a limited time. The player has to run to reach the exit before the time runs out

In any gamemode, the player can see the solution if stuck, however in doing so he loses the game.

## Musics
The player can better enjoy the game by turning on the volume. There are different musics for each gamemode and situations.

## File organization
We have created three new files to organize the code:
1. `Menu.fs` Handles all the functions releated to menus and user interaction
2. `MazeGenerator.fs` Contains all the classes needed to the maze, like the `Vector` class for cells position, the `MazeCell` class and the `Maze` class
3. `Config.fs` Contains the ASCII texts that will be displayed in the game and paths for the sound resources

## Main functions
Here are listed some of the main functions used in this project
1. `Menu.init` Called directly from Main.main_game(), it's responsible for inititializing the engine and all his functions. It also contains the settings for the maze generation
2. `Menu.myLoop` The function called at each engine frame. It handles the various status of the game, like showing the menus, displaying win or lose screen and others. All possible status are listed in `type Status = Menu|InGame|Victory|ShowSolution|KeysMenu|SelectMode|Lose`
3. `MazeGenerator.Maze.generateMaze` It's the main algorithm that generates the maze. The generated MazeCells are listed in `MazeGenerator.Maze.mutableMaze : MazeCell list`
4. `MazeGenerator.Maze.weightedSolution` Finds the shortest solution using Dijkstra's algorithm and return it as a MazeCell list