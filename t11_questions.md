# T11: The Legend of Tuna: Breath of the Catnip

## Instructions

Please replace each `**Replace This Text With Your Response**` with your answer.

___

## SECTION 1

1.a. First, discuss with your team and assign yourselves roles. Try to pick the role you’ve had the least experience in.

```
    |                 | Monday | Wednesday | Friday |
    |-----------------|--------|-----------|--------|
    | Driver          |Faryal  | Faryal    |Faryal  |
    | Navigator       |Faryal  | Faryal    |Faryal  |
    | Quality Control |Faryal  | Faryal    |Faryal  |
```

___

## SECTION 2

2.a. Look at the three Python files in the repository. Identify below all of the classes, and a brief description of
    what each one represents:

```
   The three classes in thses files are Game class, NPC class and the player class.
   The Game class sets up the environment for the game and players. The NPC class includes all the functions, necessary to handle 
   the position of the NPC and the Player class includes all the functions necessary to handle the movements of the player.
   
```

2.b. Look more closely at the **t11_game.py** file. There are 8 lines; identify if they are 
    a) instance parameters
    b) method calls within the class
    c) method calls to another class or library

(Some are more than one answer!)

```
1. self.size = 800, 600                     # a) instance parameter
2. self.running = True                      # a) instance parameter
3. pygame.init()                            # c) method call to another library (pygame)
4. self.screen = pygame.display.set_mode(self.size)     # c) method call to another library (pygame), with result stored as an instance parameter
5. self.screen.fill('#9CBEBA')             # b) method call within the class (calling method on an instance parameter)
6. self.clock = pygame.time.Clock()        # c) method call to another library (pygame), with result stored as an instance parameter
7. self.tuna = Player(self.size)           # c) method call to another class (Player), with result stored as an instance parameter
8. self.tacocat = NPC(self.size)           # c) method call to another class (NPC), with result stored as an instance parameter

```

2.c. Parse through the `run()` method of **t11_game.py**. In particular, note how the game handles 
    a) collisions between the player and NPC,
    b) moving the player and NPC around the screen, 
    c) redrawing the player and NPC after they move,
    d) how often the game updates the screen

In your own words, describe how the four items above are accomplished in the Game class:

```
a) Collisions between the player and NPC are detected using pygame.sprite.spritecollide(self.tuna, [self.tacocat], False). If a collision occurs, a message saying "Taco, you caught me!!" is rendered and displayed on the screen.

b) Movement of the player and NPC is handled by calling their respective movement() methods. The player's movement is controlled by keyboard input, passed in using pygame.key.get_pressed(), while the NPC moves based on its own logic.

c) Redrawing the player and NPC after they move is done by first clearing the screen with self.screen.fill('#9CBEBA'), and then drawing the player and NPC at their updated positions using blit() calls.

d) The screen is updated using pygame.display.update() at the end of each frame. The game runs at a controlled speed of 24 frames per second, regulated by self.clock.tick(24) to ensure smooth animation and gameplay.
```

_Return to the Google Doc to continue the assignment._

---

## SECTION 3

3.a: Take a look at the **t11_player.py** file. What class does the `Player` class inherit functionality from? 
     How do you know?

```
    The Player class in t11_player.py inherits functionality from pygame.sprite.Sprite. This is evident from the class definition, which is written as class Player(pygame.sprite.Sprite):.
    By inheriting from pygame.sprite.Sprite, the Player class gains built-in sprite functionality provided by Pygame, such as having a rect attribute and being compatible with functions like pygame.sprite.spritecollide(), which is used in the main game loop to detect collisions.
```

3.b. Sprites need two attributes to function: A surface and a rectangle. The surface (implemented in a `Surface` 
     class inside **pygame**) represents the drawing that will be rendered to the screen. The rectangle 
     (implemented in the `Rect` class in **pygame**) represents the area where the surface will be drawn on the screen, 
     including its width, height, and position. Find the lines of code that implement these two ideas, 
     and explain what each line does. 

```
    In the Player class, sprites require two key attributes to function: a surface and a rectangle. These are implemented using Pygame’s Surface and Rect classes.
    The line self.surf = pygame.Surface((50, 50)) creates a surface that is 50 pixels wide and 50 pixels tall, representing the visual appearance of the sprite that will be drawn on the screen. 
    The line self.rect = self.surf.get_rect() generates a rectangle that matches the dimensions of the surface. This rectangle determines the sprite’s position and area on the screen and is used for positioning and collision detection.
    Together, these lines set up the visual and spatial behavior of the sprite in the game
```

3.c. The `Player` class has only one method so far. Parse that code and docstring, and describe what it does:

```
    The Player class currently has one method called movement(), which controls how the player moves based on keyboard input. 
    According to its docstring, the method takes a list of currently pressed keys (provided by pygame.key.get_pressed()) and updates the player's position accordingly. Inside the method, it checks whether specific arrow keys (up, down, left, right) are being pressed. If so, it adjusts the player's rectangle (self.rect) by moving it 5 pixels in the appropriate direction using move_ip(). For example, pressing the UP key moves the player 5 pixels upward. 
    This method allows the player to navigate the game screen using the keyboard.
```

3.d. Similarly, the `NPC` class in **t11_NPC.py** also inherits the `Sprite` class from **pygame**, 
     but it does a little more than our `Player` class. Compare the two classes, and identify/describe the differences:

```
Both the Player and NPC classes inherit from pygame.sprite.Sprite, but the NPC class includes additional logic that makes it behave differently from the player.

First, while the Player class uses a simple colored surface (created using pygame.Surface()), the NPC class loads an image (tacocat.png) using pygame.image.load() to represent its visual appearance. 

The NPC also uses convert_alpha() and set_colorkey() to handle transparency in the image.

Second, the NPC class introduces autonomous movement. 
It doesn't rely on keyboard input like the player. Instead, it chooses a direction randomly at creation (self.path = random.choice(self.directions)) and moves in that direction each frame using the movement() method. The NPC changes direction in two ways: either when it reaches the edge of the screen (handled by the get_direction() method) or randomly 5% of the time. This gives the NPC more dynamic behavior.

Finally, the NPC keeps track of its logical position in a separate self.position list, whereas the player only uses self.rect for positioning.

In summary, while the Player class allows for user-controlled movement using keyboard input, the NPC class moves independently, uses an image for appearance, includes logic to avoid screen boundaries, and can randomly change directions to simulate autonomous behavior.
```

3.e. Of particular interest is how we keep the `NPC` on the screen. Describe how we're using 
    the `self.rect` attribute in the `get_direction()` method to keep the `NPC` visible.  

```
 In the get_direction() method of the NPC class, the self.rect attribute is used to check the NPC's position relative to the screen boundaries. self.rect contains the coordinates and dimensions of the NPC's image, allowing the program to determine if the NPC is about to move off-screen.
 The method uses conditionals to detect when any edge of the NPC touches or crosses the screen limits:

If self.rect.bottom is greater than or equal to the screen height, it means the NPC is at the bottom edge, so its direction is changed to "north".

If self.rect.top is less than or equal to 0, it's at the top edge, so the direction changes to "south".

If self.rect.left is less than or equal to 0, it's at the left edge, so the direction changes to "east".

If self.rect.right is greater than or equal to the screen width, it's at the right edge, so the direction changes to "west".

Additionally, there's a small chance (5%) each frame that the NPC will randomly change direction, adding unpredictability to its movement. This logic ensures the NPC stays visible on the screen while moving around.
```

_Return to the Google doc to continue the assignment._ 

---

## SECTION 4

Using **t11_NPC.py** as a starting point, create a new class called `Good_NPC` (you can do this in the **t11_NPC.py** 
file, or create a new file; your choice). Have the new class inherit from the `NPC` class that I gave you, 
including calling the parent class's initializer. Convert **t11_game.py** so that it spawns Taco Cat as a `Good_NPC` 
instead of an NPC. Debug any errors you get; the program should work, at this point. 

4.a. How hard was it to create the child class, given the parent?

```
It was not very hard to create the Good_NPC child class because most of the functionality was already implemented in the parent NPC class. All I had to do was define the new class, make it inherit from NPC, and call the parent’s initializer using super().__init__(screen_size). 
Since Taco Cat's behavior didn’t need to change, I could reuse the same image and movement code. The process was straightforward and helped me understand how inheritance allows code reuse and organization.
```

The parent class `NPC` currently holds attributes like the image used, which are actually more specific to 
`Good_NPC` now. Refactor the code so that you can indicate the image for Good_NPCs and Evil NPCs (coming next)
inside the child classes, instead of the parent class. There are multiple ways to accomplish this; discuss with your 
partner first how you would like to approach this problem. 

Next, implement another new class called `Bad_NPC` (again, you can do this in the **t11_NPC.py** 
file, or create a new file; your choice). Our bad NPC (Whiskers) is going to march around the screen in a different way
than our friend Taco Cat; he should move like a Boustrophedon, working his way across the entire screen, before 
moving up or down. Because this NPCs movement is significantly different from the Good NPCs movement, we should 
make a design choice. We could:
    a) keep the `movement` method in NPC, and override it inside `Bad_NPC` with a new method.
    b) remove `movement` from NPC, and implement separate `movement` functions in each child class.
    c) refactor `movement` in NPC, so it can handle both child class options.

4.b. Discuss with your partner your design choice above, including their pros and cons. Document your 
     choice and why: 

```
    **Replace This Text With Your Response**
```

Finally, we need to create our enemy object, Whiskers. Update **t11_game.py** to:
    a) spawn `whiskers` at the beginning of the game
    b) make `whiskers` move around the screen
    c) handle collisions between Tuna and Whiskers, which ends the game
    d) (optional) handle collisions between Taco Cat and Whiskers, which kills Whiskers and spawns a new evil NPC

---

## SECTION 5

5.a. Inheritance allows us to produce special cases of a class, extending their functionality. Describe
    what challenges you faced while implementing the child classes that extended the `NPC` class. 
    How did you overcome them?

```
    **Replace This Text With Your Response**
```