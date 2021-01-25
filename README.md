# Minimum Knight Moves

## https://leetcode.com/problems/minimum-knight-moves


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
                    if(!visited.contains(posX + "," + posY) && posX >= -2 && posY >= -2) {
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

# References :
1. https://leetcode.com/problems/minimum-knight-moves/discuss/392053/Here-is-how-I-get-the-formula-(with-graphs)
