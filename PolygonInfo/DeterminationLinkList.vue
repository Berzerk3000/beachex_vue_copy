<template>
  <div class="wrap">
    <div class="determinationLinkTitle">
      <h2>
        {{ species }} <span class="count">{{ amount }}</span>
      </h2>
    </div>
    <button
      class="subListTab"
      v-for="(determination, index) of determinationLinkIDs"
      :key="determination.id"
    >
      <a :href="getLink(determination.id)" target="_blank">
        zur Fundmeldung {{ species }} {{ index + 1 }}
      </a>
    </button>
  </div>
</template>

<script>
import Routing from "../../../js/routing";
export default {
  props: ["determinationLinkIDs", "species", "amount"],
  methods: {
    getLink(id) {
      return Routing.generate("be_determination_detail", { id });
    },
  },
};
</script>

<style></style>