<html>
<table summary="Tournament Bracket" class="bracket">
<style>
   table.bracket {
    border-collapse: collapse;
    border: none;
}

.bracket td {
    vertical-align: middle;
    width: 40em;
    margin: 0;
    padding: 10px 0px 10px 0px;
}

.bracket td p {
    border-bottom: solid 1px black;
    border-top: solid 1px black;
    border-right: solid 1px black;
    margin: 0;
    padding: 5px 5px 5px 5px;
}

.bracket th{
    text-align:center;
}
</style>

<tr>
    <th>game 1<br>date</th>
    <th>game 2<br>date</th>
    <th>game 3<br>date</th>
<tr>
    <td>
    <select name="teams1" id="selection 1">  <!--- Create the team dropdown menus. --->
        <option value="team 1">team 1</option> 
        <option value="team 2">team 2</option> 
        <option value="team 3">team 3</option> 
        <option value="team 4">team 4</option> 
    </select></td>
    <td rowspan="2" id="selection 2"><select name="teams2"> 
        <option value="team 1">team 1</option> 
        <option value="team 2">team 2</option> 
        <option value="team 3 ">team 3</option> 
        <option value="team 4">team 4</option> </td>
    <td rowspan="4"><select name="teams1" id="selection 1"> 
        <option value="team 1">team 1</option> 
        <option value="team 2">team 2</option> 
        <option value="team 3">team 3</option> 
        <option value="team 4">team 4</option> 
    </select></td>
<tr>
    <td><select name="teams1" id="selection 1"> 
        <option value="team 1">team 1</option> 
        <option value="team 2">team 2</option> 
        <option value="team 3">team 3</option> 
        <option value="team 4">team 4</option> 
    </select></td>
<tr>
    <td><select name="teams1" id="selection 1"> 
        <option value="team 1">team 1</option> 
        <option value="team 2">team 2</option> 
        <option value="team 3">team 3</option> 
        <option value="team 4">team 4</option> 
    </select></td>
    <td rowspan="2"><select name="teams1" id="selection 1"> 
        <option value="team 1">team 1</option> 
        <option value="team 2">team 2</option> 
        <option value="team 3">team 3</option> 
        <option value="team 4">team 4</option> 
    </select></td>
<tr>
    <td><select name="teams1" id="selection 1"> 
        <option value="team 1">team 1</option> 
        <option value="team 2">team 2</option> 
        <option value="team 3">team 3</option> 
        <option value="team 4">team 4</option> 
    </select></td>


<p id="mario" class="sprite"></p>
<p id="mario2" class="sprite3"></p>

{% assign sprite_file = site.baseurl | append: "/" | append: "mario_animation.png" %}  <!--- Liquid concatentation --->
{% assign hash = site.data.mario_metadata %}  <!--- Liquid list variable created from file containing mario metatdata for sprite --->
{% assign pixels = 256 %} <!--- Liquid integer assignment --->


<!--- HTML for page contains <p> tag named "mario" and class properties for a "sprite"  -->
  

<!--- Embedded Cascading Style Sheet (CSS) rules, defines how HTML elements look --->
<style>
  /* CSS style rules for the elements id and class above...
  */
  .sprite {
    height: {{pixels}}px;
    width: {{pixels}}px;
    background-image: url('{{sprite_file}}');
    background-repeat: no-repeat;
  }

  /* background position of sprite element */
  #mario {
    background-position: calc({{animations[0].col}} * {{pixels}} * -1px) calc({{animations[0].row}} * {{pixels}} * -1px);
  }

</style>

<!--- Embedded executable code--->
<script>
  ////////// convert yml hash to javascript key value objects /////////

  var mario_metadata = {}; //key, value object
  {% for key in hash %}  
  
  var key = "{{key | first}}"  //key
  var values = {} //values object
  values["row"] = {{key.row}}
  values["col"] = {{key.col}}
  values["frames"] = {{key.frames}}
  mario_metadata[key] = values; //key with values added

  {% endfor %}

  ////////// animation control object /////////

  class Mario {
    constructor(meta_data) {
      this.tID = null;  //capture setInterval() task ID
      this.positionX = 0;  // current position of sprite in X direction
      this.currentSpeed = 0;
      this.marioElement = document.getElementById("mario"); //HTML element of sprite
      this.pixels = {{pixels}}; //pixel offset of images in the sprite, set by liquid constant
      this.interval = 100; //animation time interval
      this.obj = meta_data;
      this.marioElement.style.position = "absolute";
    }

    animate(obj, speed) {
      let frame = 0;
      const row = obj.row * this.pixels;
      this.currentSpeed = speed;

      this.tID = setInterval(() => {
        const col = (frame + obj.col) * this.pixels;
        this.marioElement.style.backgroundPosition = `-${col}px -${row}px`;
        this.marioElement.style.left = `${this.positionX}px`;

        this.positionX += speed;
        frame = (frame + 1) % obj.frames;

        const viewportWidth = window.innerWidth;
        if (this.positionX > viewportWidth - this.pixels) {
          document.documentElement.scrollLeft = this.positionX - viewportWidth + this.pixels;
        }
      }, this.interval);
    }

    startWalking() {
      this.stopAnimate();
      this.animate(this.obj["Walk"], 3);
    }

    startRunning() {
      this.stopAnimate();
      this.animate(this.obj["Run1"], 6);
    }

    startPuffing() {
      this.stopAnimate();
      this.animate(this.obj["Puff"], 0);
    }

    startCheering() {
      this.stopAnimate();
      this.animate(this.obj["Cheer"], 0);
    }

    startFlipping() {
      this.stopAnimate();
      this.animate(this.obj["Flip"], 0);
    }

    startResting() {
      this.stopAnimate();
      this.animate(this.obj["Rest"], 0);
    }

    stopAnimate() {
      clearInterval(this.tID);
    }
  }

  const mario = new Mario(mario_metadata);

  ////////// event control /////////

  window.addEventListener("keydown", (event) => {
    if (event.key === "ArrowRight") {
      event.preventDefault();
      if (event.repeat) {
        mario.startCheering();
      } else {
        if (mario.currentSpeed === 0) {
          mario.startWalking();
        } else if (mario.currentSpeed === 3) {
          mario.startRunning();
        }
      }
    } else if (event.key === "ArrowLeft") {
      event.preventDefault();
      if (event.repeat) {
        mario.stopAnimate();
      } else {
        mario.startPuffing();
      }
    }
  });

  //touch events that enable animations
  window.addEventListener("touchstart", (event) => {
    event.preventDefault(); // prevent default browser action
    if (event.touches[0].clientX > window.innerWidth / 2) {
      // move right
      if (currentSpeed === 0) { // if at rest, go to walking
        mario.startWalking();
      } else if (currentSpeed === 3) { // if walking, go to running
        mario.startRunning();
      }
    } else {
      // move left
      mario.startPuffing();
    }
  });

  //stop animation on window blur
  window.addEventListener("blur", () => {
    mario.stopAnimate();
  });

  //start animation on window focus
  window.addEventListener("focus", () => {
     mario.startFlipping();
  });

  //start animation on page load or page refresh
  document.addEventListener("DOMContentLoaded", () => {
    // adjust sprite size for high pixel density devices
    const scale = window.devicePixelRatio;
    const sprite = document.querySelector(".sprite");
    sprite.style.transform = `scale(${.2 * scale})`;
    mario.startResting();
  });

</script>

{% assign sprite_file2 = site.baseurl | append: "/" | append: "boo_animation.png" %}  <!--- Liquid concatentation --->
{% assign hash2 = site.data.new_metadata %}  <!--- Liquid list variable created from file containing mario metatdata for sprite --->
{% assign pixels2 = 32 %} <!--- Liquid integer assignment --->

<p id="luigi" class="sprite2"></p>

<style>
  /* CSS style rules for the elements id and class above...
  */
  .sprite2 {
    height: {{pixels2}}px;
    width: {{pixels2}}px;
    background-image: url('{{sprite_file2}}');
    background-repeat: no-repeat;
    margin-top: 120px; /* Adjust the value as needed */

  }

  /* background position of sprite element */
  #luigi {
    background-position: calc({{animations[0].col}} * {{pixels2}} * -1px) calc({{animations[0].row}} * {{pixels2}} * -1px);
  }
</style>



<script>
  ////////// convert yml hash to javascript key value objects /////////

  var luigi_metadata = {}; //key, value object
  {% for key in hash2 %}  
  
  var key = "{{key | first}}"  //key
  var values = {} //values object
  values["row"] = {{key.row}}
  values["col"] = {{key.col}}
  values["frames"] = {{key.frames}}
  luigi_metadata[key] = values; //key with values added

  {% endfor %}

  ////////// animation control object /////////

  class Luigi {
    constructor(meta_data) {
      this.tID = null;  //capture setInterval() task ID
      this.positionX = 0;  // current position of sprite in X direction
      this.currentSpeed = 0;
      this.luigiElement = document.getElementById("luigi"); //HTML element of sprite
      this.pixels = {{pixels2}}; //pixel offset of images in the sprite, set by liquid constant
      this.interval = 200; //animation time interval
      this.columnPix = 32;
      this.obj = meta_data; 
      this.luigiElement.style.position = "absolute";
    }

    animate(obj, speed) {
      let frame = 0;
      const row = obj.row * this.pixels2; //row does not change
      this.currentSpeed = speed;

      this.tID = setInterval(() => {
        const col = (frame + obj.col) * this.columnPix; // set next column to goto
        this.luigiElement.style.backgroundPosition = `-${col}px -${row}px`; 
        this.luigiElement.style.left = `${this.positionX}px`;

        this.positionX += speed;
        frame = (frame + 1) % obj.frames; // mod the frame value set in .yml

        const viewportWidth = window.innerWidth;
        if (this.positionX > viewportWidth - this.pixels2) { // if speed is more than
          document.documentElement.scrollLeft = this.positionX - viewportWidth + this.pixels2; // moves left
        }
      }, this.interval);
    }

    startWalking() {
      this.stopAnimate();
      this.animate(this.obj["Walk2"], 3);
    }

    startResting2() {
      this.animate(this.obj["Rest2"], 0);
    }

    startRight() {
      this.animate(this.obj["Right"], 0)
    }


    stopAnimate() {
      clearInterval(this.tID);
    }
  }

  const luigi = new Luigi(luigi_metadata);

  ////////// event control /////////


  window.addEventListener("keydown", (event) => {
    if (event.key === "ArrowRight") {
      event.preventDefault();
      if (event.repeat) {
        luigi.startWalking();
      } else {
        if (luigi.currentSpeed === 0) {
          luigi.startWalking();
        } else if (luigi.currentSpeed === 3) {
          luigi.startWalking();
        }
      }
    }
    else if (event.key === "ArrowLeft") {
        luigi.stopAnimate();
        luigi.startRight();
      } 
  });
  //start animation on page load or page refresh
  document.addEventListener("DOMContentLoaded", () => {
    // adjust sprite size for high pixel density devices
    const scale = window.devicePixelRatio;
    const sprite = document.querySelector(".sprite2");
    sprite.style.transform = `scale(${1 * scale})`;
    luigi.startResting2();
  });

</script>



<!--- 2nd sprite file (2nd Mario created. ALL CODE BELOW IS FOR 2ND Mario, Not 1st) --->

<style>
  /* CSS style rules for the elements id and class above...
  */
  
  .sprite3 {
    height: {{pixels}}px;
    width: {{pixels}}px;
    background-image: url('{{sprite_file}}');
    background-repeat: no-repeat;
    margin-top: 90px; /* Adjust the value as needed */
  }

  /* background position of sprite element */

  #mario2 {
    background-position: calc({{animations[0].col}} * {{pixels}} * -1px) calc({{animations[0].row}} * {{pixels}} * -1px);
  }

</style>

<!--- Embedded executable code--->
<script>
  ////////// convert yml hash to javascript key value objects /////////

  var mario_metadata2 = {}; //key, value object
  {% for key in hash %}  
  
  var key = "{{key | first}}"  //key
  var values = {} //values object
  values["row"] = {{key.row}}
  values["col"] = {{key.col}}
  values["frames"] = {{key.frames}}
  mario_metadata2[key] = values; //key with values added

  {% endfor %}

  ////////// animation control object /////////

  class Mario2 {
    constructor(meta_data) {
      this.tID = null;  //capture setInterval() task ID
      this.positionX = 0;  // current position of sprite in X direction
      this.currentSpeed = 0;
      this.marioElement = document.getElementById("mario2"); //HTML element of sprite
      this.pixels = {{pixels}}; //pixel offset of images in the sprite, set by liquid constant
      this.interval = 100; //animation time interval
      this.obj = meta_data;
      this.marioElement.style.position = "absolute";
    }

    animate(obj, speed) {
      let frame = 0;
      const row = obj.row * this.pixels;
      this.currentSpeed = speed;

      this.tID = setInterval(() => {
        const col = (frame + obj.col) * this.pixels;
        this.marioElement.style.backgroundPosition = `-${col}px -${row}px`;
        this.marioElement.style.left = `${this.positionX}px`;

        this.positionX += speed;
        frame = (frame + 1) % obj.frames;

        const viewportWidth = window.innerWidth;
        if (this.positionX > viewportWidth - this.pixels) {
          document.documentElement.scrollLeft = this.positionX - viewportWidth + this.pixels;
        }
      }, this.interval);
    }

    startWalking() {
      this.stopAnimate();
      this.animate(this.obj["Walk"], 3);
    }

    startRunning() {
      this.stopAnimate();
      this.animate(this.obj["Run1"], 6);
    }

    startPuffing() {
      this.stopAnimate();
      this.animate(this.obj["Puff"], 0);
    }

    startCheering() {
      this.stopAnimate();
      this.animate(this.obj["Cheer"], 0);
    }

    startFlipping() {
      this.stopAnimate();
      this.animate(this.obj["Flip"], 0);
    }

    startResting() {
      this.stopAnimate();
      this.animate(this.obj["Rest"], 0);
    }

    stopAnimate() {
      clearInterval(this.tID);
    }
  }

  const mario2 = new Mario2(mario_metadata);

  ////////// event control /////////

  window.addEventListener("keydown", (event) => {
    if (event.key === "ArrowRight") {
      event.preventDefault();
      if (event.repeat) {
        mario2.startCheering();
      } else {
        if (mario2.currentSpeed === 0) {
          mario2.startWalking();
        } else if (mario2.currentSpeed === 3) {
          mario2.startRunning();
        }
      }
    } else if (event.key === "ArrowLeft") {
      event.preventDefault();
      if (event.repeat) {
        mario2.stopAnimate();
      } else {
        mario2.startPuffing();
      }
    }
  });

  //touch events that enable animations
  window.addEventListener("touchstart", (event) => {
    event.preventDefault(); // prevent default browser action
    if (event.touches[0].clientX > window.innerWidth / 2) {
      // move right
      if (currentSpeed === 0) { // if at rest, go to walking
        mario2.startWalking();
      } else if (currentSpeed === 3) { // if walking, go to running
        mario2.startRunning();
      }
    } else {
      // move left
      mario2.startPuffing();
    }
  });

  //stop animation on window blur
  window.addEventListener("blur", () => {
    mario2.stopAnimate();
  });

  //start animation on window focus
  window.addEventListener("focus", () => {
     mario2.startFlipping();
  });

  //start animation on page load or page refresh
  document.addEventListener("DOMContentLoaded", () => {
    // adjust sprite size for high pixel density devices
    const scale = window.devicePixelRatio;
    const sprite = document.querySelector(".sprite3");
    sprite.style.transform = `scale(${.2 * scale})`;
    mario2.startResting();
  });

</script>

<!--- 2ND MARIO CODE ENDS HERE! --->

<div id="sprite-container">
  <div id="sprite-image">
  </div>
</div>

<style> 
#sprite-image {
  height: 32px;
  width: 32px;
  background: url("/N-TMtri3project-devops-/luigi_animation.png")
    0px 0px;
}
</style>

<script>
var animationInterval;
var spriteSheet = document.getElementById("sprite-image");
var widthOfSpriteSheet = 2000;
var widthOfEachSprite = 32;

function stopAnimation() {
  clearInterval(animationInterval);
}

function startAnimation() {
  var position = widthOfEachSprite; //start position for the image
  const speed = 100; //in millisecond(ms)
  const diff = widthOfEachSprite; //difference between two sprites

  animationInterval = setInterval(() => {
    spriteSheet.style.backgroundPosition = `-${position}px 0px`;

    if (position < widthOfSpriteSheet) {
      position = position + diff;
    } else {
      //increment the position by the width of each sprite each time
      position = widthOfEachSprite;
    }
    //reset the position to show first sprite after the last one
  }, speed);
}

//Start animation
startAnimation();

</script>
<html>
<head>
    <script src="https://cdn.jsdelivr.net/npm/phaser@3.15.1/dist/phaser-arcade-physics.min.js"></script> 
</head>
<body>

<script>
    var config = {
        type: Phaser.AUTO,
        width: 400,
        height: 200,
        physics: {
            default: 'arcade',
            arcade: {
                gravity: { y: 200 }
            }
        },
        scene: {
            preload: preload,
            create: create
        }
    };

    var game = new Phaser.Game(config);

    function preload ()
    {
        this.load.image('sky', '/N-TMtri3project-devops-/BasketballCourt.png');
        this.load.image('logo', '/N-TMtri3project-devops-/basketball-sprite.png');
        this.load.image('red', '/N-TMtri3project-devops-/Red_Color.jpg');
    }

    function create ()
    {
        sprite = this.add.image(400, 300, 'sky');
        sprite.setScale(1)

        var particles = this.add.particles('red');
        particles.setScale(0.00001)

        var emitter = particles.createEmitter({
            speed: 100,
            scale: { start: 1, end: 0 },
            blendMode: 'ADD'
        });

        var logo = this.physics.add.image(400, 100, 'logo');
        logo.setScale(0.05)
        logo.setVelocity(100, 200);
        logo.setBounce(1, 1);
        logo.setCollideWorldBounds(true);

        emitter.startFollow(logo);
    }
    </script>

</body>
</html>