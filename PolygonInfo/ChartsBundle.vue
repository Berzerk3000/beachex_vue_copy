<template>
  <div class="chart_menu" v-if="topList.length > 0">
    <div class="chartTitle">
      <h3>
        Die {{ topList.length }} häufigsten {{ activeData == 'amount' ? 'Arten' : 'Fundmeldungen' }} (von
        {{ new Intl.NumberFormat('de-DE').format(total) }} insgesamt)
      </h3>
    </div>
    <BigChart :topList="topList" :activeData="activeData"></BigChart>
    <div class="chartTitle">
      <h3>Anteil der Hauptkategorien (Angaben in % und Stückzahl)</h3>
    </div>
    <BarGraph
      :total="total"
      :topCategories="topCategories"
      v-if="topCategories.length > 0 && total > 0"
      :activeData = "activeData"
    ></BarGraph>
    <div class="chartTitle">
      <h3>Anteil der Hauptkategorien:</h3>
    </div>
    <div class="top3" v-if="topCategories.length > 0 && total > 0">
      <PieChartSVG
        v-for="(cat, index) in topCategories"
        :key="cat.name + index"
        :total="total"
        :amount="cat[activeData]"
        :name="cat.name"
      ></PieChartSVG>
    </div>
    <div class="chartTitle">
      <h3>Liste aller Fundmeldungen:</h3>
    </div>
  </div>
</template>

<script>
import BarGraph from "./BarGraph";
import PieChartSVG from "./PieChartSVG";
import BigChart from "./BigChart";
export default {
  props: ["speciesClassification", "speciesIDs", "mainCategories", "activeData"],
  components: {
    BarGraph,
    PieChartSVG,
    BigChart,
  },
  data() {
    return {
      total: 0,
    };
  },
  computed: {
    topCategories() {
      let topC = [];
      this.mainCategories.forEach((cat) => {
        if (this.speciesClassification[cat]) {
          topC.push({
            name: this.speciesClassification[cat].name,
            count: this.speciesClassification[cat].count,
            amount: this.speciesClassification[cat].amount.reduce((a, b) => a + b, 0),
          });
        }
      });
      return topC;
    },
    topList() {
      const reducer = (accumulator, currentValue) => accumulator + currentValue;
      const uniqueSpecies = Array.from(new Set(this.speciesIDs.map(a => a.speciesID)));
      const topList = [];
      uniqueSpecies.forEach((speciesID) => {
        if (this.speciesClassification[speciesID]) {
          topList.push({
            count: this.speciesClassification[speciesID].count,
            name: this.speciesClassification[speciesID].name,
            id: speciesID, // not really (yet) required but good for debugging
            amount: this.speciesClassification[speciesID].amount.reduce(reducer),
          });
        }
      });
      this.total = this.getSum(topList, this.activeData);
      return topList.sort(this.compare).reverse().slice(0, 10);
    },
    
  },
  methods: {
    compare(a,b) {
      if (a[this.activeData] < b[this.activeData]) {
        return -1;
      }
      if (a[this.activeData] > b[this.activeData]) {
        return 1;
      }
      return 0;
    },
    getSum(topTen, prop) {
      let total = 0;
      for (let i = 0; i < topTen.length; i++) {
        total += topTen[i][prop];
      }
      return total;
    }
  },
};
</script>

<style scoped></style>