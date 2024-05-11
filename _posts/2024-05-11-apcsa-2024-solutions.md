# Solutions for APCSA 2024 FRQs (Form O)

These are my solutions for the <a href="https://apcentral.collegeboard.org/media/pdf/ap24-frq-comp-sci-a.pdf" target="_blank">2024 AP Computer Science A Free Response Questions</a>, as College Board hasn't released their own.

## Question 1

a)
```java
Complete the simulateOneDay method.
/**
* Simulates one day with numBirds birds or possibly a bear at the bird feeder,
* as described in part (a)
* Precondition: numBirds > 0
*/
```
For this problem, there was a 95% chance of a normal day. If the day was normal, each bird would eat between 10 and 50 seeds (inclusive), shown by `(int)(Math.random() * 41) + 10`. If this total amount ended up being more than the current amount, then the current amount is set to 0. If the day is abnormal (a bear eats it all), then the current amount is set to 0.
```java
public void simulateOneDay(int numBirds) {
        if((int)(Math.random() * 100) < 95)
        {
            int amount = (int)(Math.random() * 41) + 10;
            int total = amount * numBirds;
    
            if(currentAmount - total >= 0)
            {
               currentAmount -= total;
            }
            else
            {
                currentAmount = 0;
            }
        }
        else
        {
            currentAmount = 0;
        }
    }
```

b)
```java
Complete the simulateManyDays method. Assume that simulateOneDay works as intended, regardless of what you wrote in part (a). You must use simulateOneDay appropriately in order to receive full credit.
/**
* Returns the number of days birds or a bear found food to eat at the feeder in this simulation,
* as described in part (b)
* Preconditions: numBirds > 0, numDays > 0
*/
```
For this problem, I initially checked if the current amount of food was 0. If it was, 0 days can pass, so I return 0. Then, I iterate through the number of days, calling `simulateOneDay` each time. After the simulation, if the current amount of food is 0, I return the current day. If the loop finishes, all days were successful, so I return the number of days.
```java
public int simulateManyDays(int numBirds, int numDays) {
        if(currentAmount == 0) return 0;
        for(int i = 1; i <= numDays; i++)
        {
            simulateOneDay(numBirds);
            if(currentAmount == 0) return i;
        }
        return numDays;
    }   
```

## Question 2
`Write the complete Scoreboard class. Your implementation must meet all specifications and conform to the examples shown in the preceding table.`

The second question of every FRQ is a class-writing problem. This problem wanted a class that would keep track of the score of a game between two teams, and keep track of whose turn it was. I created a constructor that would take in the names of the two teams, and set their scores to 0. I also set the default turn to team1 (specified in the instructions). 

The `recordPlay(int points)` method would take in the number of points scored and add it to the current team's score. If the points were 0, the current team would switch.

The `getScore()` method would return the current score in the format `t1Score-t2Score-currentTeamName`.

```java
class Scoreboard {
    private String team1;
    private String team2;
    private int t1Score;
    private int t2Score;
    private boolean t1Turn;
    
    public Scoreboard(String t1, String t2)
    {
        team1 = t1; t1Score = 0;
        team2 = t2; t2Score = 0;
        
        t1Turn = true;
    }
    
    public void recordPlay(int points)
    {
        if(t1Turn) t1Score += points;
        else t2Score += points;
        
        if(points == 0) t1Turn = !t1Turn;
    }
    
    public String getScore()
    {
        String result = "";
        result = t1Score + "-" + t2Score + "-";
        if(t1Turn) result += team1;
        else result += team2;
        
        return result;
    }
}
```

## Question 3

a)

```java
Complete the isWordChain method.
/**
* Returns true if each element of wordList (except the first) contains the previous
* element as a substring and returns false otherwise, as described in part (a)
* Precondition: wordList contains at least two elements.
* Postcondition: wordList is unchanged.
*/
```

For this problem, I set the initial return boolean to true. I then iterate through every element in `wordList`, beggining at index 1, until the current element does NOT contain the previous element. If this is the case, it sets isChain to false and breaks the loop.

```java
public boolean isWordChain() {
        boolean isChain = true;
        for(int i = 1; i < wordList.size(); i++) {
            if(!wordList.get(i).contains(wordList.get(i-1))) {
                isChain = false;
                break;
            }
        }
        return isChain;
    }
```

b)

```java
Complete the createList method.
/**
* Returns an ArrayList<String> based on strings from wordList that start
* with target, as described in part (b). Each element of the returned ArrayList has had
* the initial occurrence of target removed.
* Postconditions: wordList is unchanged.
* Items appear in the returned list in the same order as they appear in wordList.
*/
```

For this problem, I iterated through the `wordList` ArrayList. If the beginning of the word matched the `target` String, I would add the word to the new ArrayList `toReturn`. I also added in checks to make sure the iterated word was `>=` to the target, or else there would be an error.

```java
public static ArrayList<String> createList(String target) {
        ArrayList<String> toReturn = new ArrayList<String>();

        for(String word : wordList) {
            if(word.length() < target.length()) {
                continue;
            }
            if(word.substring(0, target.length()).equals(target)) {
                toReturn.add(word.substring(target.length()));
            }
        }

        return toReturn;
    }
```

## Question 4
a)
```java
Complete the getNextLoc method.
/**
* Returns the Location representing a neighbor of the grid element at row and col,
* as described in part (a)
* Preconditions: row is a valid row index and col is a valid column index in grid.
* row and col do not specify the element in the last row and last column of grid.
*/
```

For this problem, I first checked if the current row was the last row. If it was, I would return the Location immediately to the right. If the current column was the last column, I would return the Location immediately below. If both of these are false, I return the Location with the smaller value of the two.

```java
public Location getNextLoc(int row, int col) {
        if(row == grid.length - 1) return new Location(row, col + 1);
        if(col == grid[0].length - 1) return new Location(row + 1, col);
        
        if(grid[row][col + 1] < grid[row + 1][col]) return new Location(row, col + 1);
        else return new Location(row + 1, col);
    }
```

b)
```java
Write the sumPath method. Assume getNextLoc works as intended, regardless of what you wrote in part (a). You must use getNextLoc appropriately in order to receive full credit.
/**
* Computes and returns the sum of all values on a path through grid, as described in
* part (b)
* Preconditions: row is a valid row index and col is a valid column index in grid.
* row and col do not specify the element in the last row and last column of grid.
*/
```

For this problem, I first set the sum to the current value of the grid at the given row and column. I then run a while loop until the next Location is the last element in the grid. In each loop, I add the value of the current Location to the sum, and set the next Location to the result of `getNextLoc`. After the loop, I add the value of the last element to the sum. I then return the sum.

```java
public int sumPath(int row, int col)
    {
        int sum = grid[row][col];
        Location next = getNextLoc(row, col);
        while(next.getRow() != grid.length - 1 || next.getCol() != grid[0].length - 1)
        {
            sum += grid[next.getRow()][next.getCol()];
            next = getNextLoc(next.getRow(), next.getCol());
        }
        sum += grid[grid.length - 1][grid[0].length - 1];
        return sum;
    }
```
