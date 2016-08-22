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

This function needs to look at previous piece in the sushiTower array, this piece below the new piece, to set it's
position and side. If the array is empty it will use the sushiBasePiece. 

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

## Tapping the play button

When you tap the play button the game state should change to ready.

> [action]
> Add the following to end of `setupPlayButton()`.
> 
```
/* Setup play button selection handler */
playButton.selectedHandler = {
    /* Start game */
    self.state = .ready
}
```
> 
> This won't have any visible effect at the moment. You need some more logic to run the game.  
>

## Game Logic 

Tapping on the screen causes the cat to punch some sushi. The sushi flies off the screen opposite the cat. At this point
all of the sushi descend. If the bottom sushi piece has a chopstick on the same side as the cat the player loses. This is
the core game mechanic. You need roll some code to make this work. 

The mechanic above really centers around the tap. At the moment you tap you can determine all of the logica above because
you know or can ask:

- Which side of the screen the tap occured?
- What is the side of the cat?
- What is the side of the next sushi piece?

From this information you can give points, whack cat, and move sushi. 

## Ignore touches when the game is over

If the game is over, or the game is in title mode we can ignore touch events. 

> [action]
> Add the following at the **top** of `touchesBegan()`.
>
```
/* Game not ready to play */
if state == .gameOver || state == .title { return }
```
>
> This exits the function early if we aren't ready or playing. 
>

## Switch from ready to playing

If the game is in the ready state you're ready to play at the firts tap. 

> [action]
> Add the following just below `if state == .gameState...`.
>
```
/* Game begins on first touch */
if state == .ready {
    state = .playing
}
```
>
> If the state was ready we enter the state becomes playing.
>

## Adding the flip action

Before we can finish the game logic we need define some actions to "flip" pieces to the left or right as they are punched. 
You will do this by creating a utility function: `flip(piece:, side:)`. This method takes the piece to flip, and which 
side to flip towards. 

> [action]
> The following after `// MARK: - Utility`. 
> 
```
func flip(piece: SushiPiece, side: Side) {
    var x: CGFloat = 300
    var r = CGFloat(M_PI_2)
>    
    if player.side == .right {
        x = -x
        r = -r
    }
 >   
    let moveAction = SKAction.moveBy(x: x, y: 0, duration: 0.5)
    let rotateAction = SKAction.rotate(byAngle: r, duration: 0.5)
    let moveAndRotate = SKAction.group([moveAction, rotateAction])
    let removeAction = SKAction.removeFromParent()
    let sequenceAction = SKAction.sequence([moveAndRotate, removeAction])
>    
    piece.run(sequenceAction)
}
```
> 
> This function first calculates the x position and rotation (r) that will used to move the piece. If the side is right
> it inverts these values. 
>
> Rotation in SpriteKit is measured in radians, one full rotation is 2 Pi. M_PI is a constant for PI. M_PI_2 is a cosntant
> for half PI. In radians half PI is equivalent to 90 degrees. 
>
> The next section builds a complex action. THe first step is to make an action that moves and rotates the suhi piece. You
> did this with a group. Notice that moveAction and rotateAction, and grouped together into moveAndRotate. 
> 
> Next you want to run the moevAndRotate action and follow this by removing the sushiPiece from it's parent. Here you 
> created removeAction, then made a sequence (sequenceAction) of moveAndRotate and removeAction. A sequence runs 
> actions in series. 
> 
> Last you ran the sequenceAction on the piece of sushi. 
> 

## Punching sushi from the stack

Now we can start punching sushi from the stack. To make this happen you need to remove the first sushiPiece from the 
sushitower, this should be the piece at the bottom. Flipping, that is passing it to the flip function, will animate it off
the screen and remove it. Then you need to add a new sushi piece from the stack. Last you need to move all of the
sushi pieces down. 

So far we have code to flip sushi pieces and add new sushi pieces. Later you will add code to move pieces down the 
screen. 

> [action]
> Add the followng at the end of `touchesBegan()`. 
> 
```
let firstPiece = sushiTower.removeFirst()
flip(piece: firstPiece, side: player.side)
addSushiPiece()
```
> 
> This gets the first sushi piece in the tower array, while also removing it from the array. Next you passed that pieces
> to the `flip()` function, this should move it off the screen and rmeove it from it's parent. Last add a new sushi piece
> to the tower. 
> 
> Testing at this stage, tap the play button to start the game, tapping the left side should send a piece off to the right
> tapping the right side should send a piece off the left. The pieces don't move down the screen yet. 
> 

## Moving the tower down

You can take several approaches ot moving pieces down the screen. Actions are one choice. This tutorial will use
`update()` instead. Update makes a good choice here for two reasons. First it will give you a look at `update()` an 
important method of SKScene. Second, using Actions can be problematic if sushi pieces are removed faster than than the 
action that is moving the pieces. 

### update()

Your scene calls update 60 frames per second (FPS) while the game is running. This is useful for moving game elements and 
checking game logic while the game is running. In this case we will use update to position game pieces, constantly 
moving them down the screen. 

### It's not really 60 FPS

The system attempts to call upate 60 times per second but do to processing speed the computer will not always achieve
this. When `skView.showsFPS` is set to true you will see the FPS in the lower right of your screen. 

Update like init may have lots of code. With init you added code to setup methods that were called from init. Use the same
strategy here. 

> [action]
> Add the following to the GameScene class. 
> 
```
// MARK: - Update 
>
override func update(_ currentTime: CFTimeInterval) {
    moveTowerDown()
}
>
func moveTowerDown() {
    var n: CGFloat = 0
    for piece in sushiTower {
        let o = state == .gameOver ? 0 : sushiPieceHeight
        let y = (n * sushiPieceHeight) + firstPieceY + o
        piece.position.y -= (piece.position.y - y) * 0.5
        piece.zPosition = n
        n += 1
    }
}
```
> 
> The update method calls `moveTowerDown()`. This method declares `n` to count the Sushi pieces. Then it loops through 
> all of the sushi pieces in the sushi tower. For each piece it finds the y position for that pieces. Which is calculated 
> as position in the array (`n`) times the height of the piece, plus the y position of the first piece. Now that you 
> have found the y position of the piece you move the piece 50% of of the distance from it's current location to y 
> position. Last set the z position of the piece and increment n. 
> 
> This line `let o = state == .gameOver ? 0 : sushiPieceHeight' is used to land the piece on the head of the cat when the 
> game is over. Normally the peiecs stop one sushi piece height above the base piece. When the game is over the last 
> piece moves down to to cover the base piece. 
> 
> Testing the game at this stage should be pretty satisfying. After tapping Play, you should be able to tap on the left 
> and right to remove sushi pieces, the tower should fall as pieces are removed, and a new piece is added each time 
> the first piece is removed. Endless sushi madness!
> 












