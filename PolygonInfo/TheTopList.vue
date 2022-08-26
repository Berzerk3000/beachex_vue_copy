<template>
  <div class="top_list_wrap">
    <div class="top_list_row">
      <div class="leftList">Fundmeldung</div>
      <div class="rightList">Anzahl</div>
    </div>
    <div
      class="top_list_row"
      v-for="(species, index) in topList"
      :key="species.name"
      :class="{ active: active == species.name }"
      @mouseover="listHover(species.name)"
      @mouseleave="listLeave()"
    >
      <div class="color_hint" :style="{ backgroundColor: colors[index] }"></div>
      <div class="top_list_anzahl">{{ species[activeData] }}</div>
      {{ species.name }}
    </div>
    <!-- footer row -->
    <div class="top_list_row"></div>
  </div>
</template>

<script>
export default {
  props: ["topList", "active", "activeData"],
  methods: {
    listHover(value) {
      this.$emit("activeItem", value);
    },
    listLeave() {
      this.listHover(null);
    },
  },
  data() {
    return {
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
      colors_mix: [
        "#47597b",
        "#acd768",
        "#a970ad",
        "#db4344",
        "#fde37f",
        "#68cae5",
        "#92a9e2",
        "#76e4bd",
        "#c47447",
        "#ff0596",
      ],
    };
  },
};
</script>

<style scoped>
.leftList {
  float: left;
  padding-left: 1em;
}
.rightList {
  float: right;
}
.top_list_wrap {
  padding: 0.3em;
  border-radius: 2px;
  border: 1px solid rgba(0, 0, 0, 0.2);
}
.top_list_anzahl {
  font-family: 'Courier New', Courier, monospace;
  position: absolute;
  font-weight: bold;
  right: 2px;
}
.top_list_row {
  width: 100%;
  height: 1.2em;
  position: relative;
  font-size: 1em;
  padding: 0 1.2em;
  border-radius: 4px;
  border-bottom: 1px dashed rgba(0, 0, 0, 0.2);
  margin-bottom: 0.2em;
  text-overflow: ellipsis;
  white-space: nowrap;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.17, 0.8, 0.83, 0.67);
}
.top_list_row:hover {
  padding-left: 2em;
}
.top_list_row:first-of-type,
.top_list_row:last-of-type {
  padding: 0.3em;
  border: none;
  background-color: #085b7e;
  color: #fff;
  height: 1.8em;
}
.top_list_row:first-of-type {
  margin-bottom: 0.4em;
  border-top-right-radius: 2px;
  border-top-left-radius: 2px;
}
.top_list_row:last-of-type {
  margin-top: 0.4em;
  border-bottom-right-radius: 2px;
  border-bottom-left-radius: 2px;
}
.active {
  background-color: #ffad00;
}
.color_hint {
  width: 1em;
  height: calc(100% + 1px);
  position: absolute;
  left: 0;
  margin-right: 1em;
  border-top-left-radius: 2px;
  border-bottom-left-radius: 2px;
}
</style>