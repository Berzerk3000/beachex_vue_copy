<template>
  <div class="svgChartWrap">
    <svg viewBox="0 0 50 50" width="50" height="50">
      <circle r="20" cx="25" cy="25" fill="#085b7e" />
      <circle r="12" cx="25" cy="25" fill="#efefef" />
      <text class="number" text-anchor="middle" alignment-baseline="central">
        <tspan x="26" dy="27" fill="#085b7e">{{ percent }}%</tspan>
      </text>
      <circle
        fill="transparent"
        r="16"
        cx="25"
        cy="25"
        stroke="#efefef"
        stroke-width="6"
        stroke-dasharray="calc(100 * (3.1416 * 32)/100) calc(3.1416 * 32)"
        transform="rotate(-90) translate(-50)"
      />
      <circle
        fill="transparent"
        r="16"
        cx="25"
        cy="25"
        stroke="#b0b1b5"
        stroke-width="5"
        stroke-dasharray="calc(100 * (3.1416 * 32)/100) calc(3.1416 * 32)"
        transform="rotate(-90) translate(-50)"
      />
      <circle
        fill="transparent"
        r="16"
        cx="25"
        cy="25"
        stroke="#ffad00"
        stroke-width="5"
        :stroke-dasharray="dasharray"
        transform="rotate(-90) translate(-50)"
      />
    </svg>
    <div class="circle_number">{{ formatNmbr(amount) }}</div>
    <p class="top_three_title">{{ name }}</p>
  </div>
</template>

<script>
export default {
  props: ["amount", "total", "name"],
  computed: {
    percent(){
      return this.percentage(this.total, this.amount);
    },
    dasharray() {
      return "calc(" + this.percent + " * (3.1416 * 32)/100) calc(3.1416 * 32)";
    },
  },
  methods: {
    percentage(total, amount) {
      return (amount / (total / 100)).toFixed(1);
    },
  },
};
</script>

<style scoped>
svg {
  width: 100%;
  height: 100%;
}
.top_three_title {
  text-transform: uppercase;
  text-align: center;
  height: 60px;
  font-size: 1.5em;
  overflow-wrap: break-word;
  word-wrap: break-word;
  hyphens: auto;
  color: var(--color-dark);
}
.circle_number {
  height: 1.5em;
  margin: auto;
  text-align: center;
  font-size: 2.5em;
  position: relative;
  color: var(--color-dark-highlight);
}
.number {
  font-size: 8px;
  color: var(--color-alert);
}
.small {
  font-size: 0.5em;
  color: rgba(53, 53, 53, 0.24);
}
.svg_chart_wrap {
  width: 30%;
  height: 100%;
  padding: 2em 1em;
  border-radius: 8px;
  margin-bottom: 1em;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.12);
  /* background-color: #efefef; */
  background: rgb(245, 246, 247);
  background: -moz-radial-gradient(
    circle,
    rgba(245, 246, 247, 1) 81%,
    rgba(231, 235, 236, 1) 100%
  );
  background: -webkit-radial-gradient(
    circle,
    rgba(245, 246, 247, 1) 81%,
    rgba(231, 235, 236, 1) 100%
  );
  background: radial-gradient(
    circle,
    rgba(245, 246, 247, 1) 81%,
    rgba(231, 235, 236, 1) 100%
  );
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#f5f6f7",endColorstr="#e7ebec",GradientType=1);
}
@media only screen and (max-width: 780px) {
  .top_three_title{
    font-size: 1.1em;
    height: 50px;
  }
  .circle_number{
    font-size: 1.3em;
  }
}
@media only screen and (max-width: 400px){
 
  .circle_number{
    font-size: 1em;
  }
}
</style>