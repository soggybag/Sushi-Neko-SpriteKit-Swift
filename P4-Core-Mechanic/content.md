---
title: Working out the details
slug: working-out-the-details
---

# Working out the details

In this section you will create a stack of sushi pieces. To do this you will add some helper functions, and call these after
the scene is is displayed. 

An SKScene gets the message didMovetoView() after it is presented. This happen after init is called. Use didMoveToView() 
for setting up things need to happe right before the game is ready to start. 

Each SushiPiece might have chopsticks on the left, or the right, or no chopsticks at all. We will work out this detail 
in the SushiPiece class. 

## Define the Side enum

To keep track of the side for the Player and SushiPiece you will use an enum. An enum allows you to define descriptive value 
that is customized for use. 

> [action]
> Create a new Swift file. Name it "Side". Add the following to Side.swift.
>
```
/* Tracking enum for use with character and sushi side */
enum Side {
    case Left, Right, None
}
```
>

## Setting the side for Player

The player will need to appear in the left or the right of the tower. Notice the cat's right paw is raised. When the cat is 
on the right side we want the left paw to raised. You can make this happen easily by setting scaleX of the Player sprite to 
-1. Imagine the player sprite is a papaer card, think of setting the scale x like flipping the card over so it is inverted.

An easy way to move the player to the left or right of the tower is to set the anchor point. Normally you would want to use 
position.x to move a sprite. In this case the anchor works well because we also want flip the player. In your case the anchor
point will be in the center, the player will be offset to the left, and setting the scale x to -1 will flip the player around
the center making it appear on the right with the art inverted. 

To make this process easy to work with you will setup the Player class to handle it all. 

> [action]
> Add the following to Player `init()`.
>
```
anchorPoint.x = 1
```
>
> Testing at this point you should see the cat to the left of the sushi piece. 
>







## The SushiTower 

The sushi tower is a stack of SushiPieces. You will need to kep track of these. To do that you will store them in an array. 
By keeping them in an array you can get the first piece or the last piece. Count the number of pieces, or apply some 
effect or examine the features of all of the pieces. 

> [action]
> Add the follow class property to the top of your class. 
> 
```
/* Sushi tower array */
var sushiTower: [SushiPiece] = []
```
> 
> This defines an empty array that will object of type SushiPiece. 
> 

