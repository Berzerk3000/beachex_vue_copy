<template>
  <div class="big_chart_wrap">
    <ThePie
      :topList="topList"
      @activeItem="setActive"
      :active="activeItem"
      :activeData="activeData"
      v-if="topList.length > 0"
    ></ThePie>
    <TheTopList
      :topList="topList"
      @activeItem="setActive"
      :active="activeItem"
      :activeData="activeData"
    ></TheTopList>
  </div>
</template>

<script>
import ThePie from "./ThePie";
import TheTopList from "./TheTopList";

export default {
  props: ["topList", "activeData"],
  components: { ThePie, TheTopList },
  data() {
    return {
      activeItem: "null",
    };
  },
  methods: {
    setActive(value) {
      this.activeItem = value;
    },
  },
};
</script>

<style scoped>
.big_chart_wrap {
  display: grid;
  grid-template-columns: 1fr 1.5fr;
  width: 100%;
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

.active {
  background-color: #ffad00;
  padding-left: 2em;
}

@media only screen and (max-width: 780px) {
  .big_chart_wrap {
    grid-template-columns: 1fr;
    margin-bottom: 1em;
  }

  .big_chart_wrap div:nth-of-type(1) {
    order: 2;
    padding: 1em 0;
  }
  .big_chart_wrap div:nth-of-type(2) {
    order: 1;
  }
}
</style>
