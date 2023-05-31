<html>
<head>
    <script src="https://cdn.jsdelivr.net/npm/phaser@3.15.1/dist/phaser-arcade-physics.min.js"></script> 
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

    function preload ()
    {
        this.load.image('sky', '/BasketballCourt.png');
        this.load.image('logo', '/basketball-sprite3.png');
        this.load.image('red', '/Red_Color.jpg');
    }

    function create ()
    {
        sprite = this.add.image(400, 300, 'sky');
        sprite.setScale(2)

        var particles = this.add.particles('red');
        particles.setScale(0.00001)

        var emitter = particles.createEmitter({
            speed: 100,
            scale: { start: 1, end: 0 },
            blendMode: 'ADD'
        });

        var logo = this.physics.add.image(400, 100, 'logo');
        logo.setScale(0.1)
        logo.setVelocity(100, 200);
        logo.setBounce(1, 1);
        logo.setCollideWorldBounds(true);

        emitter.startFollow(logo);
    }
    </script>

</body>
</html>