---
title: Stacking Sushi
slug: stacking-sushi
---

# Stacking Sushi


## Define the sushi tower

The game will create a tower of Suhi each a SushiPiece instance. You will use an array to keep track of these. 

> [action]
> Add the following to the top of the GameScene class. 
> 
```
/* Sushi tower array */
var sushiTower: [SushiPiece] = []
```
> 

## Generating random sushi pieces

Your program needs to generate a stack of sushi pieces. To make the game playable you need to have some rules. 

- The first piece, at the bottom of the stack, has no chopsticks side = .none
- If the last piece had chopsticks, side .left, or .right, the next piece should be .none

### Note on generating random numbers

Swift provides the `arc4random_uniform(n)`. This function returns a random integer in the range of 0 to n - 1. For example,
`arc4random_uniform(100)` returns a random number between 0 and 99. The type of number is UInt32. To use this with 
SpriteKit you'll need to convert to a CGFloat. 

## Generate random sides

 Add this helper function to generate a random value you can use to set the side of a SushiPiece. 
 
 > [action]
 > 
 ```
 // MARK: - Utility Functions
>
func randomSide() -> Side {
    let r = CGFloat(arc4random_uniform(100)) / 100
    if r < 0.45 {
        return .left
    } else if r < 0.9 {
        return .right
    }
    return .none
}
```
>
> This function returns a Side 45% of the time it's .left, 45% it's .right, and 10% it's .none. 
>

## Generate SushiPices

Add another function to make SushiPieces. This function will position the pieces, add them as child objects, and set the 
side. 

This function needs to look at previous piece in the sushiTower array, this piece below the new piece, to set it's position 
and side. If the array is empty it will use the sushiBasePiece. 

> [action]
> Add the following below `// MARK: - Utility Functions`.
>
```
func addSushiPiece() {
    let newPiece = SushiPiece()
    addChild(newPiece)
>    
    if let lastPiece = sushiTower.last {
        newPiece.position.x = lastPiece.position.x
        newPiece.position.y = lastPiece.position.y + sushiPieceHeight
        newPiece.zPosition = sushiBasePiece.zPosition + 1
        if lastPiece.side == .none {
            newPiece.side = randomSide()
        } else {
            newPiece.side = .none
        }
    } else {
        newPiece.position.x = sushiBasePiece.position.x
        newPiece.position.y = sushiBasePiece.position.y + sushiPieceHeight
        newPiece.zPosition = sushiBasePiece.zPosition + 1
    }
    sushiTower.append(newPiece)
}
```
> 
> Now create 10 default pieces of sushi to start the game. Add the following at the end of the `setupSushi()` method.
>
```
for _ in 1 ... 10 {
    addSushiPiece()
}
```
> 
> Test your project. You should see a tower of suhsi sprouting chopsticks at random. 
> 

## Game State 

The game will go through several states as you play. 

- Title, in this state the game is waiting for you to tap the play button
- Ready, in this state the game is ready to play waiting for you to make the first tap
- Playing, in this state you are playing the game
- GameOver, in this state the game is over

> [action]
> Create a new Swift file. Name this file: GameState. Add the following to your new file. 
> 
```
/* Enum for tracking game state */
enum GameState {
    case title, ready, playing, gameOver
}
```
> 
> 

## Add game state to GameScene

Now it's time to make use of the new GameState Enum in GameScene. 

> [action]
> Add the following to the top of the GameScene class. 
>
```
/* Game management */
var state: GameState = .title
```
> 
> 

### Tapping the play button

When you tap the play button the game state should change to ready.

> [action]
> Add the following to end of `setupButton()`.
> 
```
/* Setup play button selection handler */
playButton.selectedHandler = {
    /* Start game */
    self.state = .ready
}
```
> 
> 










