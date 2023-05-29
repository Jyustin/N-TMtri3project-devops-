
<!--- 2nd sprite file --->
{% assign sprite_file = site.baseurl | append: "/" | append: "snes_animation.png" %}  <!--- Liquid concatentation --->
{% assign hash = site.data.kong_metadata %}  <!--- Liquid list variable created from file containing mario metatdata for sprite --->
{% assign pixels = 32 %} <!--- Liquid integer assignment --->



<!--- HTML for page contains <p> tag named "mario" and class properties for a "sprite"  -->
<p id="mario" class="sprite"></p>
  

<!--- Embedded Cascading Style Sheet (CSS) rules, defines how HTML elements look --->
<style>
  /* CSS style rules for the elements id and class above...
  */
  .sprite {
    height: {{32}}px;
    width: {{pixels}}px;
    background-image: url('{{sprite_file}}');
    background-repeat: no-repeat;
  }

  /* background position of sprite element */
  #mario {
    background-position: calc({{animations[0].col}} * {{pixels}} * -1px) calc({{animations[0].row}} * {{pixels}} * -1px);
  }
</style>

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
      this.interval = 200; //animation time interval
      this.columnPix = 32;
      this.obj = meta_data; 
      this.marioElement.style.position = "absolute";
    }

    animate(obj, speed) {
      let frame = 0;
      const row = obj.row * this.pixels; //row does not change
      this.currentSpeed = speed;

      this.tID = setInterval(() => {
        const col = (frame + obj.col) * this.columnPix; // set next column to goto
        this.marioElement.style.backgroundPosition = `-${col}px -${row}px`; 
        this.marioElement.style.left = `${this.positionX}px`;

        this.positionX += speed;
        frame = (frame + 1) % obj.frames; // mod the frame value set in .yml

        const viewportWidth = window.innerWidth;
        if (this.positionX > viewportWidth - this.pixels) { // if speed is more than
          document.documentElement.scrollLeft = this.positionX - viewportWidth + this.pixels; // moves left
        }
      }, this.interval);
    }

    startWalking() {
      this.stopAnimate();
      this.animate(this.obj["Walk"], 3);
    }

    startResting() {
      this.animate(this.obj["Rest"], 0);
    }

    stopAnimate() {
      clearInterval(this.tID);
    }
    startJumping() {
      this.stopAnimate();
      this.animate(this.obj["Jump"], 3);
    }
    crouch() {
      this.stopAnimate();
      this.animate(this.obj["Crouching"], 0)
    }
    walkLeft() {
      this.stopAnimate();
      this.animate(this.obj["WalkL"], 3)
    }
  }

  const mario = new Mario(mario_metadata);

  ////////// event control /////////


  window.addEventListener("keydown", (event) => {
    if (event.key === "ArrowRight") {
      event.preventDefault();
      if (event.repeat) {
        mario.startWalking();
      } else {
        if (mario.currentSpeed === 0) {
          mario.startWalking();
        } else if (mario.currentSpeed === 3) {
          mario.startWalking();
        }
      }
    } else if (event.key === "ArrowLeft") {
      event.preventDefault();
      mario.walkLeft();
      //mario.stopAnimate();
     // if (event.repeat) {
       // mario.stopAnimate();
     // } else {
       // mario.startWalking();
   //   }
    }
    else if (event.key === "ArrowUp") {
      event.preventDefault();
      mario.startJumping();
    }
    else if (event.key === "ArrowDown") {
      event.preventDefault();
      mario.crouch();
    }
  });
  //start animation on page load or page refresh
  document.addEventListener("DOMContentLoaded", () => {
    // adjust sprite size for high pixel density devices
    const scale = window.devicePixelRatio;
    const sprite = document.querySelector(".sprite");
    sprite.style.transform = `scale(${1 * scale})`;
    mario.startResting();
  });

</script>
