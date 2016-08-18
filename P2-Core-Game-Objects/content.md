---
title: Adding core game objects
slug: core-game-objects
---

#Clear GameScene.swift

The default project adds some extra code we don't need to GameScene.swift delete this. 

> [action]
> Delete everything in the GameScene Class, GameScene.swift should look like this when you are done: 
> 
```
class GameScene: SKScene {
>
}
```
>

#Define some variables to hold game objects

Our game will use the following objects. 

- player
- sushiBasePiece
- playButton
- healthBar
- scoreLabel

> [action]
> Add the following to the GameScene Class
>
```
/* Game objects */
let player: Player
let sushiBasePiece: SushiPiece
let playButton: ButtonNode
let healthBar: SKSpriteNode
let scoreLabel: SKLabelNode
```
>

#Initialize your variables 

You'll see an error at this point because you have declared variables and then need to be initialized! Add an initializer.
You are going to override init(size:) because you are initializing your GameScene with a size in GameViewController. You 
need to include init(coder aDecoder:) because SKScene conforms the NSCoding protocol and requires it. 

You'll also need to call super.init(), this is required. 

> [action]
> Add init(size: CGSize)
```
// MARK: - Init
>
override init(size: CGSize) {
 >   
    super.init(size: size)
>
}
>
required init?(coder aDecoder: NSCoder) {
    fatalError("init(coder:) has not been implemented")
}
>

##Note:

You will still see errors. There are tow problems so far. 

First, we have defined some types that don't exist yet: Player, SushiPiece, and ButtonNode.

Second, we have declared variables but have not given them any values, yet. 

#Create stub classes 

To happify the compiler you will add some stub classes (empty classes). You will add code to these classes later in 
the tutorial. 

> [action]
> Create a new swift file. Choose file > New > File... 
> Save your file as: Player.swift. Add the following to Player.swift
>
```
import SpriteKit
>
class Player: SKSpriteNode {
>    
}
```
>

> [action]
> Create a new swift file. Save this file as: SushiPiece.swift. Add the following to SushiPiece.swift: 
> 
```
import SpriteKit
>
class SushiPiece: SKSpriteNode {
>    
}
```
>

> [action]
> let's do that again! Create a new Swift file, and name it: ButtonNode. Add the following: 
> 
```
import SpriteKit
>
class ButtonNode: SKSpriteNode {
>   
}
```
>

That should quiet some of the errors. You'll see an error telling you that you haven't initialized some of your variables
before calling super.init(). This situation is true for all classes! You'll need to do the same for the new classes you just
created. 

##SkSpriteNode

A sprite is a game object on the screen. SKSpriteNode is a class that creates game object with a picture or a color. 
Use this class anytime you want to create a game object that will be represented by an image, or a rectangle of color, 
on the screen. 

SKSpriteNode provides lots different initializers. This allows you to initialize a new sprite in many different ways. For 
example with a size and a color, or with a named image. 

When subclassing SKSpriteNode, which is what you are doing, you must call the Designated intializer! The designated
intializer for SKSpriteNode is: 

```
SKSpriteNode(texture: SKTexture?, color: UIColor, size: CGSize)
```

> [action]
> Add an initilizer to Player. Add the following to the player class:
>
```
// MARK: - Init
>
init() {
    let texture = SKTexture(imageNamed: "character1")
>   
    // Must implement the Designated Initializer
    super.init(texture: texture, color: UIColor.clear, size: texture.size())
>   
}
```
>
> Now any time you create an instance of Player it will appear with the image: character1. Notice the color is clear, or
> transparent, and the size is the same size as the texture. 












