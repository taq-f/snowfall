<template>
  <canvas ref="canvas" class="snow-fall"></canvas>
</template>

<script>
function getRandomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

class Snow {
  context;
  maxWidth;
  maxHeight;
  x;
  y;
  size;
  speed;
  wind;

  constructor(maxWidth, maxHeight, context) {
    this.context = context;
    this.maxWidth = maxWidth;
    this.maxHeight = maxHeight;
    this.x = getRandomInt(0, maxWidth);
    this.y = getRandomInt(0, maxHeight);
    this.size = getRandomInt(1, 4);
    this.speed = getRandomInt(1, 2);
    this.wind = getRandomInt(0, 0.5);
  }

  draw() {
    const ctx = this.context;
    const style = ctx.createRadialGradient(
      this.x,
      this.y,
      this.size,
      this.x,
      this.x,
      this.size
    );

    style.addColorStop(0,'rgba(225, 225, 225, 0.8)');
    style.addColorStop(0.5,'rgba(225, 225, 225, 0.2)');
    style.addColorStop(1,'rgba(225, 225, 225, 0.1)');
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
    ctx.fillStyle = style;
    ctx.fill();
    ctx.closePath();
  }

  move() {
    this.x += this.wind + Math.sin(this.y / 20) * 0.3;
    this.y += this.speed;

    if (this.y > this.context.canvas.height) {
      this.y = 0;
      this.x = getRandomInt(0, this.maxWidth); 
    }
  }
}

export default {
  props: {
    density: {
      type: Number,
      default: 30
    },
    snowing: {
      type: Boolean,
      default: true
    }
  },
  watch: {
    snowing(newVal) {
      if (newVal) {
        this.startAnimation()
      } else {
        this.stopAnimation()
      }
    }
  },
  data() {
    return {
      context: undefined,
      maxWidth: window.innerWidth,
      maxHeight: window.innerHeight,
      animationHandle: undefined,
      snows: new Set(),
    }
  },
  methods: {
    startAnimation() {
      this.context.clearRect(0, 0, this.maxWidth, this.maxHeight);
      for (const snow of this.snows) {
        snow.draw();
        snow.move();
      }
      this.animationHandle = requestAnimationFrame(this.startAnimation)
    },
    stopAnimation() {
      this.context.clearRect(0, 0, this.maxWidth, this.maxHeight);
      cancelAnimationFrame(this.animationHandle)
    }
  },
  mounted() {
    this.context = this.$refs.canvas.getContext('2d')
    this.context.canvas.width = this.maxWidth;
    this.context.canvas.height = this.maxHeight;

    for (let i = 0; i < this.density; i++) {
      const snow = new Snow(this.maxWidth, this.maxHeight, this.context)
      this.snows.add(snow)
    }

    if (this.snowing) {
      this.startAnimation();
    }
  },
  beforeDestroy() {
    this.snows.clear();
  }
}
</script>

<style scoped>
.snow-fall {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  top: 0;
  /* height: 100vh; */
  /* width: 100vw; */

  pointer-events: none;
}
</style>


