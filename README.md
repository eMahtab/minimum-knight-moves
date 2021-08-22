# Minimum Knight Moves

## https://leetcode.com/problems/minimum-knight-moves

In an infinite chess board with coordinates from -infinity to +infinity, you have a knight at square [0, 0].

A knight has 8 possible moves it can make, as illustrated below. Each move is two squares in a cardinal direction, then one square in an orthogonal direction.

![Knight Moves](knight.png?raw=true "Knight Moves")

Return the minimum number of steps needed to move the knight to the square [x, y].  It is guaranteed the answer exists.

# Solution 1 : Time Limit Exceeded
```java
class Solution {
    public int minKnightMoves(int x, int y) {
        int[][] moves = { {2, 1}, {1, 2}, {-1, 2}, {-2, 1}, {-2, -1}, {-1, -2}, {1, -2}, {2, -1}};
        
        Queue<int[]> q = new LinkedList<>();
        q.add(new int[]{0, 0});
        
        Set<String> visited = new HashSet<>();
        visited.add("0,0");
        
        int minMoves = 0;
        while(!q.isEmpty()) {
            int size = q.size();
            for(int i = 0; i < size; i++) {
                int[] curr = q.remove();
                if(curr[0] == x && curr[1] == y) {
                    return minMoves;
                }
            
                for(int[] move : moves) {
                    int posX = curr[0] + move[0];
                    int posY = curr[1] + move[1];
                    if(!visited.contains(posX + "," + posY)) {
                        visited.add(posX + "," + posY);
                        q.add(new int[]{posX, posY});
                    }
                }
            }
            
            minMoves++;    
        }
        return minMoves;
    }
}
```

## Knight Moves Symmetry

![Knight Moves Symmetry](knight-moves-symmetry.png?raw=true "Knight Moves Symmetry")

If you look carefully in the above diagram, knight moves are symmetric vertically, horizontally and diagonally.

# Solution 2 : Using Symmetry

```java
class Solution {
    public int minKnightMoves(int x, int y) {
        int[][] moves = { {2, 1}, {1, 2}, {-1, 2}, {-2, 1}, {-2, -1}, {-1, -2}, {1, -2}, {2, -1}};
        int absX = Math.abs(x);
        int absY = Math.abs(y);
        
        Queue<int[]> q = new LinkedList<>();
        q.add(new int[]{0, 0});
        
        Set<String> visited = new HashSet<>();
        visited.add("0,0");
        
        int minMoves = 0;
        while(!q.isEmpty()) {
            int size = q.size();
            for(int i = 0; i < size; i++) {
                int[] curr = q.remove();
                if(curr[0] == absX && curr[1] == absY) {
                    return minMoves;
                }
            
                for(int[] move : moves) {
                    int posX = curr[0] + move[0];
                    int posY = curr[1] + move[1];
                    if(posX < 0 || posY < -1) // we can also write it like (posX < -1 || posY < 0) continue;
                        continue;
                    if(!visited.contains(posX + "," + posY)) {
                        visited.add(posX + "," + posY);
                        q.add(new int[]{posX, posY});
                    }
                }
            }
            
            minMoves++;    
        }
        return minMoves;
    }
}
```
## Why we are neglecting posX < -1 and posY < 0
We only have issue reaching cell (1,1), to reach that cell we need to visit either cell (-1, 2) or cell (2, -1). All other cells can be skipped, where posX is less than -1 or posY is less than -1. By doing this we are reducing our search space drastically and its a great improvement by utilising the fact that Knight Moves are symmetric.


# References :
1. https://leetcode.com/problems/minimum-knight-moves/discuss/392053/Here-is-how-I-get-the-formula-(with-graphs)
