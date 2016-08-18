# Displaying Objects 

In the last section you created various objects to display, mostly SKSpriteNodes, your SKSpriteNode sublcasses, and an
SKLabelNode, but just creating these doesn't make them appear on the screen. To be visible on the screen every node must 
be a child of the Scene, or a child of another node that is child of the scene. 

Make an object a child of a node by calling addChild(node:). 

Note! You can't call any methods of a class before super.init()! 

## Setup

To keep out code organized we'll put all of our setup code into one or more setup methods. You will call on these setup 
methods from init().

## Add a Background

Here you will make a sprite node that will display the background image. You wn't need a reference to it after its created 
so you didn't create a variable for it earlier. 

> [action]
> Add the following to your GameScene class
>
```
// MARK: - Setup
>
func setupBackground() {
   let background = SKSpriteNode(imageNamed: "background")
   addChild(background)
   background.anchorPoint = CGPoint(x: 0, y: 0)
   background.size = size
}
```
>
> Here you made a new sprite stored in a local variable, background is only accessible in setupBackground(). Then you
> used addChild to make it a child of GameScene, an SKScene subclass. Next you moved the anchor point to the lower left 
> corner. Last you set the size of the image to match the size of game scene. Remember earlier you initialized GameScene
> with the size of the View in GameViewController. 
> 
> Now call `setupBackground()` in init. 
> 
> [action]
> Add the following after `super.init()`.
>
```
setupBackground()
```
>

Test your work. Run your project int eh simulator. You should see something like this: 

![]()

## Add some constants

When you have numbers that might apper in several places it'a good idea to store them in a variable. You need two numbers. 
One to set the vertical location of the first sushi piece, and another to hold the height of sushi pieces. 

> [action]
> Add the following to the top your GameScene class. 
> 
```
/* Some useful numbers */
let sushiPieceHeight: CGFloat = 55
let firstPieceY: CGFloat = 200
```
> 

## Setup player sprite

Next let's follow the same procedure you used with the background to set up the player. 

> [action]
> Add the following below `// MARK: - Setup`
> 
```
func setupPlayer() {
    addChild(player)
    player.position.x = size.width / 2
    player.position.y = firstPieceY
    player.zPosition = 99
}
```
>
> Next call `setupPlayer()` below `super.init()`.
> 
```
setupPlayer()
```
>
> Here you added the player sprite as a child of the scene. Then you set it's x position to half the width of the screen
> and it's y position to firstPieceY. The zPosition determins the stacking order, or what is front of what. This number 
> is arbitrary, we just need to make sure it's higher than the other objects so the player appears on top. 
> 

## Setup SushiBasePiece

This is the first sushi piece on the stack. It's at the bottom of the stack at the same y value as the cat. 

> [action]
> Add the following after `MARK: - Setup`.
> 
```
func setupSushi() {
   addChild(sushiBasePiece)
   sushiBasePiece.position.x = size.width / 2
   sushiBasePiece.position.y = firstPieceY
}
```
>
> Then call `setupSushi()`, add the following after `super.init()`.
>
```
setupSushi()
```
>
> Here you added the first sushi piece as a child of the scene. Then positioned at in the middle of the screen on the x and 
> set the y to firstPieceY. 
> 

## Setup playButton

Do it all again for the playButton!

> [action]
> Add the following after `MARK: - Setup`. 
> 
```
func setupPlayButton() {
    addChild(playButton)
    playButton.position.x = size.width / 2
    playButton.position.y = 75
}
```
>
> Then add a call to `setupPlayButton()` in 'super.init()`.
> 
```
setupPlayButton()
```
>
