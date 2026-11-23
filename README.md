# Warehouse robot pathfinding

A robot starts in one corner of a warehouse and needs to reach the opposite corner. Two shelves block the middle aisle. This AI course project uses breadth-first search (BFS) in an n8n workflow to find a shortest route around them.

**[Watch the narrated walkthrough](https://al-tam.github.io/warehouse-robot-pathfinding/)** to see which squares the search checks, which route the robot takes, and what happens when both exits from the start are blocked.

## Run it in n8n

1. Download [workflow.json](workflow.json) using GitHub’s **Download raw file** button.
2. Open a blank workflow in n8n. In the editor’s **…** menu, choose **Import from File** and select the JSON file.
3. Select **Execute workflow**. Open **Record Path Found** and look at the output. `finalPath` lists the route in order, `totalCost` counts the moves, and `visualGrid` draws the result.

The workflow uses built-in nodes and a manual trigger. All map data is inside **Generate Warehouse Grid**. You can change the start, goal, or obstacles there and run another search. Coordinates are written as `[row, column]`, starting at zero in the top-left corner.

## The default run

The robot starts at `[0,0]`. Shelves occupy `[1,1]` and `[1,2]`. To reach `[3,3]`, the robot takes three steps right along the top aisle and three steps down the last column. The search expands 14 squares. The robot travels only six moves.

BFS checks squares in order of distance from the start. Each time it discovers a square, it saves the square it came from. Once it reaches the goal, it follows those links back to recover the route. Every move costs one, so the first route it finds to the goal has the lowest possible cost. When routes tie, this workflow checks right, down, left, then up.

## Try blocking the start

In **Generate Warehouse Grid**, add `{ row: 0, col: 1 }` and `{ row: 1, col: 0 }` to the obstacle list. Run the workflow again and open **Record No Path**. The robot checks only its starting square and returns **Goal is unreachable**. Putting a shelf directly on the start instead produces an input validation message before the search begins.

The walkthrough animates the expansion order and route returned by the workflow’s JavaScript nodes for these example maps.
