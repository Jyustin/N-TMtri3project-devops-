<html>
<!--- 2nd sprite file --->
{% assign sprite_file2 = site.baseurl | append: "/" | append: "luigi_animation.png" %}  <!--- Liquid concatentation --->
{% assign hash = site.data.new_metadata %}  <!--- Liquid list variable created from file containing mario metatdata for sprite --->
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
  }

  /* background position of sprite element */
  #luigi {
    background-position: calc({{animations[0].col}} * {{pixels2}} * -1px) calc({{animations[0].row}} * {{pixels2}} * -1px);
  }
</style>



<script>
  ////////// convert yml hash to javascript key value objects /////////

  var luigi_metadata = {}; //key, value object
  {% for key in hash %}  
  
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

</html>