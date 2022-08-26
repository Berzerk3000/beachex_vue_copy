<template>
  <div class="svgPie">
    <svg
      :height="settings.h"
      :width="settings.w"
      viewBox="-1 -1 2 2"
      style="transform: rotate(-90deg)"
    >
      <path
        v-for="(pathData, index) of sliceData"
        :key="'slice' + index"
        :d="pathData.d"
        :fill="pathData.fill"
        :class="{ active: active == pathData.name }"
        @mouseover="sliceHover(pathData.name, pathData.count)"
        @mouseleave="sliceLeave()"
      ></path>
    </svg>
  </div>
</template>

<script>
export default {
  props: ["topList", "active", "activeData"],
  computed: {
    pieData(){
      const total = this.getSum(this.topList, this.activeData);
      const data = [];
      let percent = 0;
      let rotate = -90;
      let index = 0;
      this.topList.forEach(species => {
        percent = (species[this.activeData] / total).toFixed(2);
        let dasharray = "calc(" + percent + " * 31.42/100) 31.42";
        data.push({
          color: this.settings.colors[index],
          name: species.name,
          count: species[this.activeData],
          percent: percent,
          dasharray: dasharray,
          rotate: rotate,
          transform: "rotate(" + rotate + ") translate(-20)",
        });
      index++;
      rotate += parseFloat(percent);
      });
    return data
    },
    sliceData(){
      let cumulativePercent = 0;
      const sliceData = [];
      this.pieData.forEach((slice) => {
        // destructuring assignment sets the two variables at once
        const [startX, startY] = this.getCoordinatesForPercent(cumulativePercent);
        // each slice starts where the last slice ended, so keep a cumulative percent
        cumulativePercent += parseFloat(slice.percent);
        const [endX, endY] = this.getCoordinatesForPercent(cumulativePercent);
        // if the slice is more than 50%, take the large arc (the long way around)
        const largeArcFlag = slice.percent > 0.5 ? 1 : 0;
        // create an array and join it just for code readability
        const pathData = [
          `M ${startX} ${startY}`, // Move
          `A 1 1 0 ${largeArcFlag} 1 ${endX} ${endY}`, // Arc
          `L 0 0`, // Line
        ].join(" ");
        sliceData.push({
          d: pathData,
          fill: slice.color,
          name: slice.name,
          count: slice[this.activeData],
        });
      });
      return sliceData;
    }
  },
  data() {
    return {
      settings: {
        r: 1,
        h: 2,
        w: 2,
        colors: [
          "#0e9fdc",
          "#0b7faf",
          "#0a719c",
          "#09658b",
          "#085b7d",
          "#075372",
          "#074b67",
          "#06445f",
          "#053e55",
          "#05384d",
        ], // blue shade
      },
    };
  },
  methods: {
    getSum(topList, prop) {
      var total = 0;
      for (var i = 0, _len = topList.length; i < _len; i++) {
        total += topList[i][prop];
      }
      return total;
    },
    sliceHover(name) {
      // todo : panel für pie info box mit name und count
      this.emitActive(name);
    },
    emitActive(value) {
      this.$emit("activeItem", value);
    },
    getCoordinatesForPercent(percent) {
      const x = Math.cos(2 * Math.PI * percent);
      const y = Math.sin(2 * Math.PI * percent);
      return [x, y];
    },
    sliceLeave() {
      this.emitActive(null);
    },
  },
};
</script>

<style scoped>
.svg_pie {
  margin: auto;
  width: 200px;
  height: 200px;
}
.active {
  fill: #ffad00;
  transform: scale(1);
}
path {
  transition: all 0.2s cubic-bezier(0.53, 0.05, 0.56, 1.17);
  transform: scale(0.9);
  cursor: pointer;
}
</style>