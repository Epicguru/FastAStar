# Fast A*
A generic but fast implementation of the A* pathfinding algorithm. Can be used with any game engine, and has been tested with Unity.
Requires you to have certain things set up within your project: see below for details on getting things running.
This system is made possible by [BlueRaja's High Speed Priority Queue](https://github.com/BlueRaja/High-Speed-Priority-Queue-for-C-Sharp).

## Requirements
You must already have some kind of 2D tile-based world set up.
This 2D world must have limits, as in a width and a height that never change.
The very minimum required to use the pathfinding system is to be able to quickly determine weather a particular tile is 'walkable' or not. Specifically you must be able to return true or false to the following:
```
public bool IsTileWalkable(int x, int y)
{
  // Return true if the tile can be walked through, false if it cannot be walked through (such as a wall).
  // Your code here...
  
  return true;
}
```

## Installing
Simply copy the Pathfinding and HSPQ folders into your project source code directory.
You should also copy the LICENSE.md file, but it isnt require for the project to work (obviously).
The two folders are located [here](https://github.com/Epicguru/FastAStar/tree/master/FastAStar).

## Usage
This is how you would use this system within your own project...
### Provider
Once you are sure you meet the requirements within your project, create a new class that inherits from TileProvider.
Then override the abstract function called IsTileWalkable. This function is the base minimum for the system to work, as described in the Requirements section.
The complete class might looks something like this.
```
public class MyProvider : TileProvider
{
  // Some kind of 2D map. How you implement this is up to you.
  public bool[,] map;
  
  public MyProvider(int width, int height) : base(width, height)
  {
    map = new bool[width, height];
  }
  
  public override bool IsTileWalkable(int x, int y)
  {
    // Returns the value of the map at that position.
    // True means that the tile can be pathfinded accross, false means that it is treated as unpassable.
    // In your project you would have a better 2D map set up and you would query that instead.
    return map[x, y];
  }
}
```
### Queries
Now that you have the provider class set up, you can use the Pathfinding class to actually do pathfinding to determine a path from a starting position to a target position.

Here is a sample of code that would determine the path from a starting point to the end, using the provider that we set up:
```
// Create an instance of the Pathfinding class. You will normally only need one instance of this for your whole project.
Pathfinding pather = new Pathfinding();

// Define world width and height, in tiles. You can do this wherever and however you like.
int width = 100;
int height = 100;

// Create an instance of your TileProvider. Again, you generally only need one instance of this per project.
TileProvider provider = new CustomProvider(width, height);

// To actually run pathfinding, we need a starting point and a destination.
// Remember, the units are tiles.

// Starting at (10, 15)
int startX = 10;
int startY = 15;
// Ending at destination (50, 75)
int endX = 50;
int endY = 75;

// Now run the algorithm.
List<PNode> path;
PathfindingResult result = pather.Run(startX, startY, endX, endY, provider, out path);

// Make sure that the path was found correctly...
if(result != PathfindingResult.SUCCESSFUL)
{
  Console.WriteLine("Error running pathfinding: " + result);
}
else
{
  Console.WriteLine("Path found! It is " + path.Count + " tiles long.");
  
  // Print out each tile along the path. The final path includes both the start and end positions.
  foreach(PNode tile in path)
  {
    Console.WriteLine(tile);
  }
}
```
