<html>
<head>
    <script src="https://cdn.jsdelivr.net/npm/phaser@3.15.1/dist/phaser-arcade-physics.min.js"></script> <!-- Fetching the outside library--->
</head>
<body>

<script>
    var config = {
        type: Phaser.AUTO,
        width: 1500,
        height: 300,
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

    function preload ()  // load all the images
    {
        this.load.image('sky', '/N-TMtri3project-devops-/BasketballCourt.png');
        this.load.image('logo', '/N-TMtri3project-devops-/basketball-sprite3.png');
        this.load.image('red', '/N-TMtri3project-devops-/Red_Color.jpg');
    }

    function create () // create images 
    {
        sprite = this.add.image(400, 300, 'sky');
        sprite.setScale(2) //changes scale of img (self explanatory)

        var particles = this.add.particles('red');
        particles.setScale(0.00001) // i dont want particles but also dont want to delete the code so I just make it impossible to see them.

        var emitter = particles.createEmitter({ // particle code, not needed
            speed: 100,
            scale: { start: 1, end: 0 },
            blendMode: 'ADD'
        });

        var logo = this.physics.add.image(400, 100, 'logo'); // set basketball physics (movement)
        logo.setScale(0.1)
        logo.setVelocity(100, 200); 
        logo.setBounce(1, 1);
        logo.setCollideWorldBounds(true);

        emitter.startFollow(logo); //particle code 
    }
    </script>

</body>
</html>