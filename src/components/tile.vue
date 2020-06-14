<template>
  <div class="tile">
    <span class="tile__label">{{ label }}</span>
    <span class="tile__reflection"></span>
  </div>
</template>

<script>
import { gsap } from "gsap";
const DURATION = 0.3;

export default {
  name: "Tile",

  props: {
    label: {
      type: String,
      required: false,
      default: "Label"
    },
    color: {
      required: false,
      default: "#bbb"
    }
  },

  methods: {
    tiltTile(el, progress) {
      const tiltMin = -20;
      const tiltMax = 20;

      const tiltRange = tiltMax - tiltMin;
      const tiltX = tiltMin + tiltRange * progress.y;
      const tiltY = tiltMin + tiltRange * progress.x;

      gsap.to(el, {
        "--tilt-x": `${tiltX}deg`,
        duration: DURATION
      });
      gsap.to(el, {
        "--tilt-y": `${-tiltY}deg`,
        duration: DURATION
      });
    },

    shiftLabel(el, progress) {
      const shiftMin = -5;
      const shiftMax = 5;

      const shiftRange = shiftMax - shiftMin;
      const shiftX = shiftMin + shiftRange * progress.x;
      const shiftY = shiftMin + shiftRange * progress.y;

      gsap.to(el, {
        "--shift-x": `${-shiftX}px`,
        duration: DURATION
      });
      gsap.to(el, {
        "--shift-y": `${-shiftY}px`,
        duration: DURATION
      });
    },

    shiftReflection(el, progress) {
      const shiftMin = {
        x: 0,
        y: -200
      };

      const shiftMax = {
        x: el.clientWidth,
        y: el.clientHeight * 0.75
      };

      const shiftRange = {
        x: shiftMax.x - shiftMin.x,
        y: shiftMax.y - shiftMin.y
      };

      const shift = {
        x: shiftRange.x - shiftRange.x * progress.x,
        y: shiftMin.y + shiftRange.y * progress.y
      };

      el.style.setProperty("--reflection-x", `${shift.x}px`);
      el.style.setProperty("--reflection-y", `${shift.y}px`);
    }
  },

  mounted() {
    this.$el.style.setProperty("--color", this.color);

    this.$el.onmousemove = event => {
      const progress = {
        x: event.offsetX / this.$el.clientWidth,
        y: event.offsetY / this.$el.clientHeight
      };

      this.tiltTile(this.$el, progress);
      this.shiftLabel(this.$el, progress);
      this.shiftReflection(this.$el, progress);
    };

    this.$el.onmouseout = () => {
      gsap.to(this.$el, {
        "--tilt-x": "0deg",
        duration: 0.6
      });
      gsap.to(this.$el, {
        "--tilt-y": "0deg",
        duration: 0.6
      });
      gsap.to(this.$el, {
        "--shift-x": 0,
        duration: 0.6
      });
      gsap.to(this.$el, {
        "--shift-y": 0,
        duration: 0.6
      });

      this.$el.style.setProperty(
        "--reflection-x",
        `${this.$el.clientWidth / 2}px`
      );
      this.$el.style.setProperty("--reflection-y", 0);
    };
  }
};
</script>

<style lang="scss" scoped>
.tile {
  --color: #bbb;
  --tilt-x: 0;
  --tilt-y: 0;
  --shift-x: 0;
  --shift-y: 0;
  --reflection-x: center;
  --reflection-y: -40px;

  height: 20rem;

  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;

  border-radius: 2rem;
  background-color: var(--color);
  box-shadow: 0 4px 8px rgba(#000, 0.1);
  transform: perspective(800px) rotateX(var(--tilt-x)) rotateY(var(--tilt-y));

  overflow: hidden;
  cursor: pointer;
}

.tile__label {
  font-size: 3.2rem;
  font-weight: lighter;
  color: #fff;
  transform: perspective(800px) translate(var(--shift-x), var(--shift-y));

  pointer-events: none;
}

.tile__reflection {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;

  background: radial-gradient(
    circle at var(--reflection-x) var(--reflection-y),
    rgba(#fff, 0.3) 0%,
    rgba(#fff, 0) 100%
  );
}
</style>
