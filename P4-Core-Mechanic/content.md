---
title: Working out the details
slug: working-out-the-details
---

# Working out the details



## Define the Side enum

To keep track of the side for the Player and SushiPiece you will use an enum. An enum allows you to define descriptive value 
that is customized for use. 

The cat will use this enum also since it will appear on one side or the other. 

Sharing the enum between the two classes gives us a common value we can compare when we need to ask if a chop stick is on 
the same side as the cat. 

> [action]
> Create a new Swift file. Name it "Side". Add the following to Side.swift.
>
```
/* Tracking enum for use with character and sushi side */
enum Side {
    case left, right, none
}
```
>

## Setting the side for Player

The player will need to appear on the left or the right of the tower. Notice the cat's right paw is raised. When the cat is 
on the right side we want the left paw to raised. You can make this happen easily by setting scaleX of the Player sprite to 
-1. Imagine the player sprite is a papaer card, think of setting the scale x like flipping the card over so it is inverted.

An easy way to move the player to the left or right of the tower is to set the anchor point. Normally you would want to use 
position.x to move a sprite. In this case the anchor works well because we also want flip the player. In your case the anchor
point will be in the center, the player will be offset to the left of it's anchor, and setting the scale x to -1 will flip
the player around the center making it appear on the right when the art inverted (scale x -1). 

To make this process easy to work with you will setup the Player class to handle it all. You will give the player a computed 
property "side" setting the valeu of side to a new value will also set the scale x of the cat. 

> [action]
> Add the following to Player `init()`.
>
```
anchorPoint.x = 1
```
>
> Testing at this point you should see the cat to the left of the sushi piece. 
>
> Now add the following to the top of the Player class. 
> 
```
/* Character side */
var side: Side = .left {
    didSet {
        if side == .left {
            xScale = 1
        } else {
            /* An easy way to flip an asset horizontally is to invert the X-axis scale */
            xScale = -1
        }
        
    }
}
> 
> Imagine the didSet block here is trigger when the value of side is set to a new value. 
> 

## Adding a touch event

To test the system above and get us closer to making the game function add a touch event to set the player side. Tap on 
the left side of the screen set the player.side to left, tap on the right set player.side to right. 

> [action]
> Add the following to the GameScene class. 
> 
```
// MARK: - Touch Events
>
override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
    // Get a touch
    let touch = touches.first!
    // Find the location of the touch
    let location = touch.location(in: self)
    // Was touch on left/right hand side of screen?
    if location.x > size.width / 2 {
        player.side = .right
    } else {
        player.side = .left
    }
}
```
>
> Here you got the first touch, there may have been more than one, then found the location of that touch in this scene. 
> Next you looked at the location.x to figure if the touch was on the left or the right and set the player.side to match. 
> 
> Test your work. Tapping on the left or right should make the player appear on the matching side. 
> 

## Adding some action

SKAction is a class that is ridiculously versatile. You can use it to animate objects on the screen, run code blocks, 
set up timed events, play sounds, and more. Look at the assets.atlas folder you will see character1-3, notice images 2 and 3
show the cat throwing a punch. Imagine when you tap the screen and set the side you want the cat to throw a punch. You can
do that with SKAction.animate(). 

> [action]
> Add a property to the top of the Player class.
>
```
let punch: SKAction
```
>
> This property will hold an action you can reuse. Now define that action in init() above `super.init()`
>
```
let punchTexture1 = SKTexture(imageNamed: "character2")
let punchTexture2 = SKTexture(imageNamed: "character3")
punch = SKAction.animate(with: [punchTexture1, punchTexture2, punchTexture1], timePerFrame: 0.05, resize: true, restore: true)
```
>
> Last, call your action in side didSet. Add the following at the end of the didSet block after the if else but before the
> closing brace.
>
```
run(punch)
```
>
> Test your work. This time tapping a side should produce an animated punch. 
> 

## Adding chopsticks to Sushi Rolls

The Sushi Rolls need to some times display chop sticks on either the left or the right. To make this work, you will use 
similar techniques to what you used with the Player. In this case though the chaopsticks will be a child of the SushiPiece. 

> [action]
> Add the following to the top of the SushiPiece class. 
> 
```
/* Chopstick objects */
let rightChopstick: SKSpriteNode
let leftChopstick: SKSpriteNode
```
> 
> Now that you have declared new variables, you will need to initialize them. Add the following above `super.init()`. 
>
```
rightChopstick = SKSpriteNode(imageNamed: "chopstick")
leftChopstick = SKSpriteNode(imageNamed: "chopstick")
```
>
> Remember, you will need to use `addChild()` to add these new sprites to a node that displayed in the scene. In this 
> case adding them to the SushiPiece is okay since the chopsticks will then be displayed when the SushiPiece is added to 
> GameScene. 
> 
> Add a setup method to this class. Add the following to the bottom of the class.
> 
```
// MARK: - Setup
>
func setup() {
    addChild(rightChopstick)
    addChild(leftChopstick)
>    
    print(leftChopstick.position.x)
    print(leftChopstick.isHidden)
>    
    rightChopstick.xScale = -1
    rightChopstick.anchorPoint.x = 1.4
    leftChopstick.anchorPoint.x = 1.4
>    
    rightChopstick.position.y = 35
    leftChopstick.position.y = 35
}
```
>
> Now call `setup()` after `super.init()`. 
>
```
setup()
```
> 
> Test your work. You should the suhipiece now has two chopsticks, one on the left and one on the right. 
>
> That good but you need to be able to set the side a chopstick will appear. Add the following to the top of the 
> SushiPiece class.
> 
```
/* Sushi type */
var side: Side = .none {
    didSet {
        switch side {
        case .left:
            /* Show left chopstick */
            leftChopstick.isHidden = false
        case .right:
            /* Show right chopstick */
            rightChopstick.isHidden = false
        case .none:
            /* Hide all chopsticks */
            leftChopstick.isHidden = true
            rightChopstick.isHidden = true
        }
    }
}
```
> 
> Notice this is very similar to what you did with the Player class. In this case setting side hides one or both 
> chopsticks. 
> 
> Set the default value to .none at the end of `setup()`.
> 
```
side = .none
```
> 
> Try setting the value here to .left, or .right and testing your project. 
> 

















