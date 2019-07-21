<template>
  <div class="note-display" data-note="7.00">
    <svg width="100%" height="100%" class="circle__svg" viewBox="0 0 41 41">
      <circle cx="21" cy="21" r="16" class="circle__progress circle__progress--path"></circle>
      <circle cx="21" cy="21" r="16" class="circle__progress circle__progress--fill"></circle>
    </svg>
    <div class="percent">
      <span class="percent__int">70%</span>
    </div>
  </div>
</template>
<script>
export default {
  methods: {
    animateDonut() {
      const display = document.querySelector('.note-display');
      const transitionDuration = 900;
      const progress = display.querySelector('.circle__progress--fill');
      const radius = progress.r.baseVal.value;
      const circumference = 2 * Math.PI * radius;
      const note = parseFloat(display.dataset.note);
      const offset = circumference * (10 - note) / 10;
      progress.style.setProperty('--transitionDuration', `${transitionDuration}ms`);
      progress.style.setProperty('--initialStroke', circumference);
      setTimeout(() => { progress.style.strokeDashoffset = offset; }, 100);
    },
  },
  mounted() {
    this.animateDonut();
  },
};
</script>
<style scoped>
@import url('https://fonts.googleapis.com/css?family=Nixie+One|Raleway:200');

.note-display {
  display: flex;
  align-items: center;
  font-family: 'Nixie One', cursive;
  position: relative;
}
.circle__progress {
  fill: none;
  stroke-width: 2;
  stroke-opacity: 0.3;
  stroke-linecap: round;
}
.note-display .circle__progress { stroke: white; }
.percent {
  width: 100%;
  top: 50%;
  left: 50%;
  position: absolute;
  font-weight: bold;
  text-align: center;
  line-height: 28px;
  transform: translate(-50%, -50%);
}
.percent__int { font-size: 16px; }
.circle__progress--fill {
  --initialStroke: 0;
  --transitionDuration: 0;
  stroke-opacity: 1;
  stroke-dasharray: var(--initialStroke);
  stroke-dashoffset: var(--initialStroke);
  transition: stroke-dashoffset var(--transitionDuration) ease;
}
.circle__svg {
  transform: rotate(-90deg);
}
</style>
