---
title: Player health and scoring
slug: gameplay-part-2
---

# Game Play

At this stage you have a core mechanic but no game play. That is game functions but you can't win, lose, and score. 

## Losing the game

If the next piece is has a wasabi chopstick on the same side as the cat when you remove a piece you lose the game. To add 
this mechanic you will need to examine the sushi tower each time you remove a piece. You can check the side for the next
piece and compare it to the side of the player. 

### Game Over 

To signal a game over 

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
    playButton.selectedHandler = {
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
> You willuse this function to handle a game over. Calling `gameOver()` sets the state of the game to gameOver, then 
> turns all of the sushi pieces red with an action: `SKAction.colorize(with: ...`, next do the same thing with the player,
> 

### Check cat whack with wasabi

Next we need to check if the player will get whacked with wasabi. To do this you need to look at the first sushi piece
and check it's side against the side of the player. If they match the player is whacked. 

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



