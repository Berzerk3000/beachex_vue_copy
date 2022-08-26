<template>
  <div class="wrap" v-if="speciesClassification.length > 0 && speciesClassification[category]">
    <div class="categoryRow">
      <div class="mainTab" @click="handleView(category)">
        <div class="count">{{ speciesClassification[category].count }}</div>
        <h3>{{ speciesClassification[category].name }}</h3>
      </div>
      <Breadcrump
        v-if="active"
        :brdcrmp="brdcrmp"
        @clickBrdcrmp="updateBrdcrmp"
      ></Breadcrump>
      <DeterminationLinkList
        :determinationLinkIDs="determinationLinkIDs"
        v-if="showDeterminationLinks"
        :species="lastSpecies"
        :amount="amount"
      ></DeterminationLinkList>
      <SubList :subSpecies="subSpecies" @clicked="switchSubList"></SubList>
    </div>
  </div>
</template>

<script>
import Breadcrump from "./Breadcrump";
import DeterminationLinkList from "./DeterminationLinkList";
import SubList from "./SubList";
export default {
  data() {
    return {
      subSpecies: [],
      brdcrmp: [],
      active: false,
      lastSpecies: null,
      amount: null,
      showDeterminationLinks: false,
      determinationLinkIDs: [],
    };
  },
  
  props: ["speciesClassification", "category", "determinationIDs"],
  components: {
    Breadcrump,
    DeterminationLinkList,
    SubList,
  },
  methods: {
    handleView(id) {
      this.showDeterminationLinks = false;
      this.subSpecies = this.getChildSpecies(id);
      this.active = !this.active;
      if (!this.active) {
        this.subSpecies.length = 0;
        this.brdcrmp.length = 0;
      } else {
        this.brdcrmp.push({
          id: id,
          name: this.speciesClassification[id].name,
        });
      }
    },
    setInactive(id) {
      if (this.category != id) {
        this.active = !this.active;
      }
    },
    updateBrdcrmp(id) {
      this.showDeterminationLinks = false;
      var clickedExisting = 0;
      this.brdcrmp.forEach((species) => {
        if (species.id == id) {
          this.brdcrmp.length = clickedExisting;
        }
        clickedExisting++;
      });
      this.brdcrmp.push({
        name: this.speciesClassification[id].name,
        id: id,
      });
      this.subSpecies = this.getChildSpecies(id);
    },
    switchSubList(id) {
      this.subSpecies = this.getChildSpecies(id);
      /// if no children show determination links
      if (!this.subSpecies.length) {
        // this.brdcrmp.length = 0;
        this.lastSpecies = this.speciesClassification[id].name;
        this.amount = this.speciesClassification[id].count;
        this.getDeterminationIDs(id);
        this.showDeterminationLinks = true;
      } else {
        this.updateBrdcrmp(id);
      }
    },
    getChildSpecies(id) {
      const children = this.speciesClassification.filter(
        (species) => species.parentID == id
      );
      return children;
    },
    getDeterminationIDs(id) {
      this.determinationLinkIDs = this.determinationIDs.filter(
        (determination) => determination.species == id
      );
    },
  },
};
</script>

<style scoped></style>