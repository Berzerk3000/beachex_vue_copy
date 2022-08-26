<template>
  <div>
    <TheHeader :title="polyStats.name"></TheHeader>
    <div class="toggleData">
      <p>Datenansicht:</p>
      <div class="toggle">
        <label>
          <span
          :class="{ activeData: activeData === 'amount' }"
          @click="toggleData('amount')">Anzahl der Arten</span>
          <span :class="{ activeData: activeData === 'count' }" @click="toggleData('count')">Anzahl der Fundmeldungen</span>
        </label>
      </div>
    </div>
    <ChartsBundle
      :speciesClassification="speciesClassification"
      :speciesIDs="speciesIDs"
      :mainCategories="mainCategories"
      :activeData="activeData"
      v-if="speciesIDs.length > 0 && speciesClassification.length > 0 && mainCategories.length > 0">
    </ChartsBundle>
    <div class="detailWrap">
      <DeterminationDetails
        v-for="cat of mainCategories"
        :key="cat"
        :category="cat"
        :speciesClassification="speciesClassification"
        :determinationIDs="determinationIDs"
      ></DeterminationDetails>
    </div>
    <div class="noDetermination" v-if="determinationIDs.length === 0 && noDeterminationFound"><p>Noch keine Fundmeldung vorhanden.</p></div>
    <TheFooter :dataLoaded="speciesClassification.length > 0 || noDeterminationFound === true"></TheFooter>
  </div>
</template>

<script>
import axios from "axios";
import Routing from "../../js/routing";
import TheHeader from "./PolygonInfo/TheHeader";
import ChartsBundle from "./PolygonInfo/ChartsBundle";
import DeterminationDetails from "./PolygonInfo/DeterminationDetails";
import TheFooter from "./PolygonInfo/TheFooter";
export default {
  name: "PolygonInfo",
  components: { TheHeader, ChartsBundle, DeterminationDetails, TheFooter },
  props: ["polygonId"],
  data: () => {
    return {
      // stats for Polygon
      polyStats: [],
      // speciesClassification[speciesID] =  {name: x, parentID: x, speciesID: x, count:0}
      speciesClassification: [],
      // Tiere id 4 / Pflanzen id 134 / Müll id 192
      // leave empty to get all top level classifications
      filterMainCategories: [4,134,192],
      // just the species IDs of the determinations from polygon
      speciesIDs: [],
      // for determination link generation
      determinationIDs: [],
      noDeterminationFound: false,
      // toggle display of data ( amount and determinations ) default set to amount
      activeData: 'amount',
    };
  },
  computed: {
    mainCategories() {
      if(this.filterMainCategories.length > 0) return this.filterMainCategories;
      return this.speciesClassification
          .filter((species) => species.parentID === undefined)
          .map((species) => species.id); 
    },
  },
  async mounted() {
    await Routing.load();
    // make a list of found speciesIDs
    this.speciesIDs = await this.getSpeciesIDs(this.polygonId);
    // get Polygon stats
    this.polyStats = await this.getPolyStats();
    // classifications
    await this.getClassification(this.speciesIDs);
  },
  methods: {
    async getSpeciesIDs(polygonId) {
      const response = await axios.get(
        Routing.generate("api_2_polygon_detail_determinations", {
          id: polygonId,
        })
      );
      const data = response.data;
      this.determinationIDs = data;
      // done this way so it only shows when nothing is found and not when its loading
      if (!this.determinationIDs.length) this.noDeterminationFound = true;
      return data.map((determination) => {
        return { speciesID:determination.species, amount:determination.amount};
      });
    },
    async getPolyStats() {
      const polyStats = await axios.get(
        Routing.generate("api_2_polygon_detail_stats", {
          id: this.polygonId,
        })
      );
      return polyStats.data;
    },
    countSpecies(speciesIDs, speciesClassification) {
      speciesIDs.forEach((species) => {
        if (speciesClassification[species.speciesID]) {
          speciesClassification[species.speciesID].count++;
          speciesClassification[species.speciesID].amount.push(species.amount);
          speciesClassification = this.countParent(
            speciesClassification[species.speciesID].parentID,
            speciesClassification[species.speciesID].amount[speciesClassification[species.speciesID].amount.length-1],
            speciesClassification,            
          );
        }
        // recursive function takes long - don't know a better way to count parent classifications..
      });
      return speciesClassification;
    },
    countParent(parentID, amount, speciesClassification) {
      if (speciesClassification[parentID]) {
        speciesClassification[parentID].count++;
        speciesClassification[parentID].amount.push(amount);
        this.countParent(
          speciesClassification[parentID].parentID,
          speciesClassification[parentID].amount[speciesClassification[parentID].amount.length-1],
          speciesClassification
        );
      }
      return speciesClassification;
    },
    removeUnfoundClassifications(speciesClassification) {
      let cleanSpeciesClassification = [];
      speciesClassification.forEach((species) => {
        if (species.count > 0) {
          cleanSpeciesClassification[species.id] = species;
        }
      });
      return cleanSpeciesClassification;
    },
    async getClassification(speciesIDs) {
      let tempSpeciesClassification = [];
      try {
        const resp = await axios.get(
          Routing.generate("api_2_classifications_index")
        );
        const allSpeciesData = resp.data;
        allSpeciesData.forEach((species) => {
          tempSpeciesClassification[species.id] = {
            id: species.id,
            parentID: species.parent_id,
            name: species.name,
            count: 0,
            subSpecies: species.species,
            amount: [],
          };
          if (species.species.length > 0) {
            species.species.forEach((subSpecies) => {
              tempSpeciesClassification[subSpecies.id] = {
                id: subSpecies.id,
                parentID: species.id,
                name: subSpecies.name,
                count: 0,
                subSpecies: null,
                amount: [],
              };
            });
          }
        });
        tempSpeciesClassification = this.countSpecies(
          speciesIDs,
          tempSpeciesClassification
        );
        this.speciesClassification = this.removeUnfoundClassifications(
          tempSpeciesClassification
        );
      } catch (e) {
        // error handling
        console.log("Problem with getClassification(speciesIDs) [" + e + "]");
      }
      return this.speciesClassification;
    },
    toggleData(data){
      this.activeData = data;
    }
  },
};
</script>

<style scoped>
.toggleData{
  width: 100%;
  text-align: left;
  padding: 1em;
  margin-bottom: 1px;
  background-color: var(--color-dark);
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  border-bottom-right-radius: 2px;
  border-bottom-left-radius: 2px;
  color: #f9f8f5;
  display: grid;
  grid-template-columns: 1fr 2.5fr;
  align-items: center;
}
.toggleData p {
  margin: 0;
  padding: 0;
}
.toggleData label{
  background-color: #fff;
  width: 100%;
  height: 100%;
  display: grid;
  grid-template-columns: 1fr;
  padding: .5em;
  cursor: pointer;
}
.toggleData span {
  padding: 0 1em;
  background-color: #085b7e;
  color: #f9f8f5;
}
.activeData {
  background-color: #ffad00!important;
  color: #000!important;
}
.activeData:after {
  content: "(aktiv)";
  padding-left: 1em;
}
@media only screen and (max-width: 639px){
  .toggleData {
    grid-template-columns: 1fr;
  }
}
</style>