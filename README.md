
# Classic Arcade Game Clone Project

## Table of Contents

- [Instructions](#instructions)
- [Contributing](#contributing)

## Instructions

Use this [rubric](https://review.udacity.com/#!/rubrics/15/view) for self-checking your submission.

Make sure the functions you write are **object-oriented** - either class functions (like `Player` and `Enemy`) or class prototype functions such as `Enemy.prototype.checkCollisions`. Also make sure that the keyword `this` is used appropriately within your class and class prototype functions to refer to the object the function is called upon.

Your **README.md** file should be updated with instructions on both how to 1. Run and 2. Play your arcade game.

For detailed instructions on how to get started, check out this [guide](https://docs.google.com/document/d/1v01aScPjSWCCWQLIpFqvg3-vXLH2e8_SZQKC8jNO0Dc/pub?embedded=true).

## Contributing

This repository is the starter code for _all_ Udacity students. Therefore, we most likely will not accept pull requests.

# Classic Arcade Game Clone Project

## Table of Contents

- [Instructions](#instructions)
- [Contributing](#contributing)

## Instructions

Use this [rubric](https://review.udacity.com/#!/rubrics/15/view) for self-checking your submission.

Make sure the functions you write are **object-oriented** - either class functions (like `Player` and `Enemy`) or class prototype functions such as `Enemy.prototype.checkCollisions`. Also make sure that the keyword `this` is used appropriately within your class and class prototype functions to refer to the object the function is called upon.

Your **README.md** file should be updated with instructions on both how to 1. Run and 2. Play your arcade game.

For detailed instructions on how to get started, check out this [guide](https://docs.google.com/document/d/1v01aScPjSWCCWQLIpFqvg3-vXLH2e8_SZQKC8jNO0Dc/pub?embedded=true).

## Contributing

This repository is the starter code for _all_ Udacity students. Therefore, we most likely will not accept pull requests.


# CAG - My Added Code

## Variables

Variables created for Game State, Lives, and Score

```
// Variables ------------------------------

let game = true;
let playerScore = 0;
let playerLives = 3;
```
Variables for the DOM classes

```
let lives = document.querySelector(".lives");
let score = document.querySelector(".score");
```
Updates the text content

```
lives.textContent = `Lives: ${playerLives}`;
score.textContent = `Score: ${playerScore}`;
```
Enemy function

```
// Enemies our player must avoid
var Enemy = function Enemy(x, y) {
    // Variables applied to each of our instances go here,
    // we've provided one for you to get started

    // The image/sprite for our enemies, this uses
    // a helper we've provided to easily load images
    this.sprite = 'images/enemy-bug.png';
    this.x = x;
    this.y = y;
    this.speed = 100;
};
```

Updates the Enemies position and add collision with the player when overlapping the enemy.

```
Enemy.prototype.update = function(dt) {

    //Moves Enemies and resets their position when reaching the end of the canvas
    this.x += this.speed * dt;
    if (this.x > 707) {
        this.x = -100;
        var randomSpeed = Math.floor(Math.random() * 4 + 1);
        this.speed = 150 * randomSpeed;
    };
    var enemyLeftX = this.x - 60;
    var enemyRightX = this.x + 60;
    var enemyTopY = this.y - 60;
    var enemyBottomY = this.y + 60;


    if (player.x > enemyLeftX && player.x < enemyRightX && player.y > enemyTopY && player.y < enemyBottomY) {
      
        player.resetPosition();
        playerLives -= 1;
        console.log(playerLives);
        lives.textContent = `Lives: ${playerLives}`;

    };
};

```
## Render the graphics

```
// Draw the enemy on the screen, required method for game
Enemy.prototype.render = function() {
    ctx.drawImage(Resources.get(this.sprite), this.x, this.y);
};
```

Variable for player images.
The player function randomizes the image every new game.

```
// Player Images
var playerSelect = [
    'images/char-boy.png',
    'images/char-cat-girl.png',
    'images/char-horn-girl.png',
    'images/char-pink-girl.png',
    'images/char-princess-girl.png'
];



// This class requires an update(), render() and
// a handleInput() method.

const Player = function Player(x, y, sprite) {
    this.sprite = playerSelect[(Math.floor(playerSelect.length * Math.random()))]; // Randomizes player selection
    this.x = x;
    this.y = y;
    this.h_step = 101;
    this.v_step = 83;
    console.log(this.sprite);
};
```
Checks if the player reaches the water, adds +100 to score and runs the playerWin function.
If the player uses all lives then it is game over.

```
Player.prototype.update = function(dt) {

    if (game && player.y < 40) {
        game = false;
        player.resetPosition();
        score.textContent = `Score: ${(playerScore += 100)}`;
        console.log(`Game over: ${game} - Score: ${playerScore}`);
        playerWin();
    };

    if (playerLives <= 0) {
        gameOver();
    };

};
```
1. Resets player function to a specific x/y coordinate.
2. Renders player graphics
3. Creates enemieswith x/y coordinates on canvas.
4. Places player on convas as specified x/y coords.

```
Player.prototype.resetPosition = function() {
    this.x = 303;
    this.y = 650;
};



Player.prototype.render = function() {
    ctx.drawImage(Resources.get(this.sprite), this.x, this.y);
};



// Place all enemy objects in an array called allEnemies
var allEnemies = [new Enemy(700, 60), new Enemy(700, 143), new Enemy(700, 226),
    new Enemy(700, 309), new Enemy(700, 392), new Enemy(700, 475),
    new Enemy(700, 558)];



// Place the player object in a variable called player
const player = new Player(303, 650);
```

Player controls and also limits ability to leave the canvas.

```

Player.prototype.handleInput = function (movement) {

    const playerHorizontal = 101;
    const playerVertical = 83;

    if (movement === "left" && this.x - playerHorizontal >= 0) {
        this.x -= playerHorizontal;
    }else if (movement === "right" && this.x + playerHorizontal < ctx.canvas.width) {
        this.x += playerHorizontal;
    }else if (movement === "up" && this.y - playerVertical >= -83) {
        this.y -= playerVertical;
    }else if (movement === "down" && this.y + playerVertical < ctx.canvas.height -200) {
        this.y += playerVertical;
    };

    /* switch (dt) {
      case "up":
        this.y -= playerVertical;
        break;
      case "down":
        this.y += playerVertical;
        break;
      case "left":
        this.x -= playerHorizontal;
        break;
      case "right":
        this.x += playerHorizontal;
        break;
    } */
   };



// This listens for key presses and sends the keys to your
// Player.handleInput() method. You don't need to modify this.
document.addEventListener('keyup', function(e) {
    var allowedKeys = {
        37: 'left',
        38: 'up',
        39: 'right',
        40: 'down'
    };

    player.handleInput(allowedKeys[e.keyCode]);
});
```
Attempted to add gems but there is an error 

```
const gemSet=[{name:'blue', image:'images/Gem blue.png', value: 50 },
              {name:'green', image:'images/Gem green.png', value: 25},
              {name:'orange', image:'images/Gem Orange.png', value: 100}];

const x_blocks=[101,505];

const y_blocks=[100,200];

const gems = function gems(props){

//to set gems randomly

    var g = gemSet[Math.floor(Math.random()* gemSet.length) ];

    this.name = g.name;

    this.image = g.image; this.value = g.value;

    this.x = x_blocks[Math.floor(Math.random()*x_blocks.length)];

    this.y = y_blocks[Math.floor(Math.random()*x_blocks.length)];

};
```

Create Gems

```
// Gem Spawn
const Gem = function Gem(x, y, sprite) {
    this.sprite = sprite;
    this.x = x;
    this.y = y;
};
```

This creates the end goal, then the next function updates and finally the last renders it.

```
const EndGoal = function EndGoal(x, y) {
    this.sprite = 'images/Selector.png';
    this.x = x;
    this.y = y;
};



EndGoal.prototype.update = function() {

    
    var gemLeftX = this.x - 60;
    var gemRightX = this.x + 60;
    var gemTopY = this.y - 60;
    var gemBottomY = this.y + 60;


    if (player.x > gemLeftX && player.x < gemRightX && player.y > gemTopY && player.y < gemBottomY) {

        score.textContent = `Score: ${(playerScore += 25)}`;

    };
};

EndGoal.prototype.render = function() {

    ctx.drawImage(Resources.get(this.sprite), this.x, this.y);

};
```

Gem update function then render function afterwards.

```
// Goal image and feature
Gem.prototype.update = function() {

    
    var gemLeftX = this.x - 60;
    var gemRightX = this.x + 60;
    var gemTopY = this.y - 60;
    var gemBottomY = this.y + 60;


    if (player.x > gemLeftX && player.x < gemRightX && player.y > gemTopY && player.y < gemBottomY) {

        score.textContent = `Score: ${(playerScore += 25)}`;

    };
};



Gem.prototype.render = function() {

    ctx.drawImage(Resources.get(this.sprite), this.x, this.y);

};
```

This creates an array of coordinates and then randomized them to fit within the canvas and grid

```
let gemMoveX = [101, 202, 303, 404, 505, 606];
let gemMoveY = [133, 216, 299, 382, 465, 548];
let gemRandomLocX = gemMoveX[(Math.floor(gemMoveX.length * Math.random()))];
let gemRandomLocY = gemMoveY[(Math.floor(gemMoveY.length * Math.random()))];

function gemRand() {
    gemMoveX[(Math.floor(gemMoveX.length * Math.random()))];
}
 
// Creates new gems


const createGem = [new Gem(gemRand(), gemRandomLocY, 'images/Gem Blue.png'),
                    new Gem(gemRand(), gemRandomLocY, 'images/Gem Green.png'),
                    new Gem(gemRandomLocX, gemRandomLocY, 'images/Gem Orange.png')];
```

This is the trigger for the player reaching the end of the level, the water

```
const endTrigger = [new EndGoal(gemRandomLocX, -40)];
```                    

The win, reset, and game over functions   

```
// Player Wins function

var winMsg = document.getElementById("wingame");
winMsg.style.display = "none";

function playerWin() {

    function displayWin() {
        if (winMsg.style.display === "none") {
          winMsg.style.display = "block";
        } else {
          winMsg.style.display = "none";
        }
      }

      displayWin();

    resetGame();

};

// Reset the game
function resetGame() {

    allEnemies = [];

};

function gameOver() {
    alert('You Lose!');
    playerLives = 3;
    lives.textContent = `Lives: ${playerLives}`;
}
```
