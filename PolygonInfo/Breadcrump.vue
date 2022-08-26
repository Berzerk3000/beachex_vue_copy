<template>
  <div class="brdcrmp">
    <ul>
      <li
        v-for="species of brdcrmp"
        @click="updateBrdcrmp(species.id)"
        :key="species.name"
      >
        {{ species.name }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ["brdcrmp"],
  methods: {
    updateBrdcrmp(id) {
      this.$emit("clickBrdcrmp", id);
    },
  },
};
</script>

<style></style>