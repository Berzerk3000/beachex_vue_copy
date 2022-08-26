<template>
  <div class="subSpecies">
    <button
      class="subListTab"
      v-for="(species, index) of subSpecies"
      :key="species.name + index"
      @click="switchSubList(species.id)"
    >
      {{ species.name }}
      <div class="count">{{ species.count }}</div>
    </button>
  </div>
</template>

<script>
export default {
  props: ["subSpecies"],
  methods: {
    switchSubList(id) {
      this.$emit("clicked", id);
    },
  },
};
</script>

<style></style>