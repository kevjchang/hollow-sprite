## hollow-sprite

ECE243 final project inspired by Hollow Knight. A single level platformer game written in C, designed for the DE1-SOC board

Project members: Ellen Shi, Kevin Chang

Our game is a 2D single player platforming game, where the main character is controlled through a keyboard using the PS2 interface and can move around, attack enemies, dash forward, and climb/hold onto walls.


# Player Controls:
WASD / Arrow keys: Move left/right, or jump
Player will move left or right as long as keys are held down, if the player reaches a wall it will stop. If the player continues to hold on to the key when it’s touching the wall the knight will “cling” onto the wall, allowing for hanging in the air and wall jumping.

Player must press and release the jump key to perform a jump. If player is holding onto the wall when the jump key is released, then the player will jump a set distance up the wall but stay attached to it.

Dash: player can press and release C to dash. Player will dash for a set distance ( 45 pixels) in the direction they are currently facing towards, if the player is within 45 pixels from a wall, they will stop at the wall.

Attack: Player can press and release the X key to stab. Attack will be performed in the direction the player is currently facing, and all enemies whose hitboxes intersect with the attack hitbox will be dealt damage.

<img width="409" alt="image" src="https://github.com/kevjchang/hollow-sprite/assets/108161055/63123337-c04b-4c49-a71d-bd9fe9c3a9fe">

# Enemies:
Various enemies fly through the air and shoot projectiles at the player whilst slowly approaching the player. There’s a baby that doesn’t spit but flies faster, which also follows the player. Slime enemies travel along the ground and deal melee damage. All enemies deal damage when touching the player. We use red to indicate the player or enemies taking damage.

There are also stationary obstacles, similar to spikes which stay in the same position and deal damage to the player when touched.

<img width="417" alt="image" src="https://github.com/kevjchang/hollow-sprite/assets/108161055/56a57888-f70d-4f93-b2f2-dc5e461a639f">


# Game Overlays:
A health bar is drawn at the bottom of the screen and is updated when the player takes damage.
Text drawn to the center of the screen states when the player has died, or has passed a stage of the level.

<img width="388" alt="image" src="https://github.com/kevjchang/hollow-sprite/assets/108161055/0ef7302c-ab9d-4836-b91f-533e40ba2cdb">

# How it works:

We use PS2 keyboard interrupts to receive player input, with some commands activated upon a make code (move), and some commands using break codes (jump, attack, dash).

We used two interval timers (oxFF202000 and oxFF202020) for a cooldown between damages taken and a fixed duration of attacks.

All game entities except for spikes (enemies, projectiles, tiles, background) are stored as nodes in a linked list, which is updated every frame where unnecessary nodes (dead enemies, projectiles which have hit the player, etc.) are removed and freed. Each node has an invisible bounding box which is used to detect collisions/attacks.


The game objects and player are drawn as sprites, which are loaded from C integer arrays and pre-processed into a 2D short int array of rgb values when the game is initialized and written into the backbuffer when updating the frame. We drew all the sprites except for tiles and the background.

The map is constructed from a 2d array of zeros and ones, with a one signifying the presence of a 60x60 pixel tile. This array is also used to quickly calculate player collisions with the map by converting player location into an array index.

The character buffer was used to display the message “DIE” when player loses and “STAGE CLEAR” when player eliminates all enemies. The health value is also displayed next to the bar.

Ellen: Character design/creation, game logic, internal timer interrupts
Kevin: Keyboard interrupts interface, background, bug fixes for game logic



