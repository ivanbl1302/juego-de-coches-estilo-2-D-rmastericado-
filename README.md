// Configuración del juego
var config = {
  type: Phaser.AUTO,
  width: 800,
  height: 600,
  scene: {
    preload: preload,
    create: create,
    update: update
  }
};

// Inicializar el juego
var game = new Phaser.Game(config);

// Variables globales
var player;
var cursors;
var obstacles;
var score = 0;
var scoreText;

// Precarga de assets
function preload() {
  this.load.image('car', 'assets/car.png');
  this.load.image('obstacle', 'assets/obstacle.png');
}

// Creación del juego
function create() {
  // Fondo del juego
  this.add.image(0, 0, 'background').setOrigin(0);

  // Jugador (coche)
  player = this.physics.add.sprite(400, 500, 'car');
  player.setCollideWorldBounds(true);
  
  // Obstáculos
  obstacles = this.physics.add.group();

  // Puntuación
  scoreText = this.add.text(16, 16, 'Score: 0', { fontSize: '32px', fill: '#000' });

  // Teclas de flecha para controlar el coche
  cursors = this.input.keyboard.createCursorKeys();

  // Colisión entre el jugador y los obstáculos
  this.physics.add.collider(player, obstacles, gameOver, null, this);
}

// Actualización del juego
function update() {
  // Movimiento del coche
  if (cursors.left.isDown) {
    player.setVelocityX(-200);
  } else if (cursors.right.isDown) {
    player.setVelocityX(200);
  } else {
    player.setVelocityX(0);
  }

  // Generar obstáculos aleatoriamente
  if (Phaser.Math.Between(0, 100) < 2) {
    var obstacle = obstacles.create(Phaser.Math.Between(50, 750), 0, 'obstacle');
    obstacle.setVelocityY(Phaser.Math.Between(100, 300));
  }

  // Actualizar la puntuación
  score += 1;
  scoreText.setText('Score: ' + score);
}

// Fin del juego
function gameOver() {
  this.physics.pause();
  player.setTint(0xff0000);
  // Aquí puedes agregar acciones adicionales para el final del juego
}
