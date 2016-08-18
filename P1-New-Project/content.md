---
title: Setting up a new project
slug: new-spritekit-project
---

#Create a new SpriteKit project

> [action]
> Create a new *Game* project in Xcode named `SushiNeko` and check the *Language* is set to `Swift` and 
> *Game Technology* is set to `SpriteKit`.
> ![Xcode new project](../Tutorial-Images/xcode_new_project.png)
>

##Importing Resources

> [action]
> Download the [Sushi Neko Art Pack](../assets.atlas.zip) we created for you.
> Once the download is complete, unpack the folder and add it to your project.
> Ensure you have *Copy items if needed* and *Create groups*.
> ![Xcode file import options](../Tutorial-Images/xcode_adding_files_flags.png)
>

##Adding extra functionality

SpriteKit is missing some useful functionality, thankfully we have bundled some together for you.  

> [action]
> Download the [MakeSchool Utilities](../Utils.zip), unpack the folder and add it to your project as you
> did with the art pack.
>

Your project structure should hopefully look similar to this:

![Xcode project structure](../Tutorial-Images/xcode_project_structure_new.png)

##Delete GameScene.sks from your project

This project won't be using using scene file. A scene file is not required. 

> [action]
>  Select GameScene.sks in the project outline. Then hit delete, and click "Move to Trash".
> ![Delete-GameScene-sks](../Tutorial-Images/Delete-GameScene-sks.png)
>

##Initialize GameViewController.swift with a size

In this step you will modify GameScene.swift to initialize with a size. We can do this instead of initializing with an 
.sks file. In GameViewController.swift in the viewDidLoad() method you are creating a new instance of GameScene.swift. 
If we aren't initializing with an .sks file we need to initialize with a size. 

> [action]
> Modify this section of viewDidLoad in GameViewController.swift
>
```
override func viewDidLoad() {
  super.viewDidLoad()
>
  let scene = GameScene(size: view.frame.size)
  // Configure the view.
  let skView = self.view as! SKView
  skView.showsFPS = true
  skView.showsNodeCount = true
>
  /* Sprite Kit applies additional optimizations to improve rendering performance */
  skView.ignoresSiblingOrder = true
>   
  /* Set the scale mode to scale to fit the window */
  scene.scaleMode = .aspectFill
>   
  skView.presentScene(scene)
>   
}
```
>
> Notice this involves removing `if let scene = GameScene(fileNamed:...`, if let is used here because initializing with a 
> named file may return an optional, what if the file does not exist? Initializing with a size is always succesful. 
>

#Summary

Great, you now have everything in place to get started building our game.
