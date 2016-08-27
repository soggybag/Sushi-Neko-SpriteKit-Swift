---
title: Player health and scoring
slug: gameplay-part-2
---

# Game Play

At this stage you have a core mechanic but no game play. That is the game functions but you can't win, lose, and score. 

## Losing the game

If the next piece is has a wasabi chopstick on the same side as the cat when you remove a piece you lose the game. To add 
this mechanic you will need to examine the sushi tower each time you remove a piece. You can check the side for the next
piece and compare it to the side of the player. 

### Game Over 

To signal a game over you will color the suchi tower and the cat red. You can do this easily with a an action: 
`SKAction.colorize(with:, colorBlendFactor:, duration:)` the tricky part is the tower is made up of ten pieces, you'll 
need to color them all. Since all of the pieces are in the sushiTower array you can use a loop. 

> [action]
> Add the following below `// MARK: - Utility`. 
> 
```
func gameOver() {
    /* Game over! */
    state = .gameOver
>    
    /* Turn all the sushi pieces red */
    for sushiPiece in sushiTower {
        sushiPiece.run(SKAction.colorize(with: UIColor.red, colorBlendFactor: 1.0, duration: 0.50))
    }
>    
    // Make the player turn red
    player.run(SKAction.colorize(with: UIColor.red, colorBlendFactor: 1.0, duration: 0.50))
>    
    // Change play button selection handler
    playButton.selectedHandler = { [unowned self] in
        /* Grab reference to our SpriteKit view */
        let skView = self.view as SKView!
>        
        /* Load Game scene */
        let scene = GameScene(size: self.view!.frame.size) as GameScene!
>        
        /* Ensure correct aspect mode */
        scene?.scaleMode = .aspectFill
>        
        /* Restart GameScene */
        skView?.presentScene(scene)
    }
}
```
>
> You will use this function to handle a game over. Calling `gameOver()` sets the state of the game to gameOver, then 
> turns all of the sushi pieces red with an action: `SKAction.colorize(with: ...`, next do the same thing with the player.
> 
> You can also change the action the button performs. Now that the game is over rather than starting the game you can 
> reload this scene, which will effectively reset the game. 
>

### Check cat whack with wasabi

Now with the `gameOver()` method set you need to check for situations that might cause a game over.

If the player gets whacked by wasabi it's game over. To check for this each time the cat punches a piece off the stack 
you need to look at the first sushi piece and check it's side against the side of the player. If they match the 
player is whacked. 

> [action]
> Add the following directly after `let firstPiece = sushiTower.removeFirst()`. It's important this appears before
> `flip(piece: firstPiece, side: player.side)`!
>
```
/* Check character side against sushi piece side (this is our death collision check)*/
if player.side == firstPiece.side {
    gameOver()
    /* No need to continue as player dead */
    return
}
```
>

## The timer

You added the art for the timer already now you will make use of the timer. 

> [action]
> Add the following to the update method. 
>
```
/* Called before each frame is rendered */
if state != .Playing { return }
>   
/* Decrease Health */
health -= 0.01
>       
/* Has the player ran out of health? */
if health < 0 { gameOver() }
```
> 

## Setting health and keeping score

> [action]
> Add the following to the top of your gameScene class. 
>
```
var health: CGFloat = 1.0 {
    didSet {
        /* Cap Health */
        if health > 1.0 { health = 1.0 }
>        
        /* Scale health bar between 0.0 -> 1.0 e.g 0 -> 100% */
        healthBar.xScale = health
    }
}
>
var score: Int = 0 {
    didSet {
        scoreLabel.text = String(score)
    }
}
```
>

## Z postition is still a problem

You may notice that after, and seemingly at random the score and the health bar fall behind the sushi pieces. 





