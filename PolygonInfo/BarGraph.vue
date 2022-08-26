<template>
  <div class="barGraph">
    <div class="barRow" v-for="cat in categoryData" :key="cat.name">
      <p>{{ cat.name }} ({{ cat.percent }}%)</p>
      <div class="bar">
        <div class="amount">{{ formatNmbr(cat[activeData]) }}</div>
        <div class="progress" :style="{ width: cat.percent + '%' }"></div>
      </div>
    </div>
    <div class="total">
      Meldungen gesamt: <span> {{ new Intl.NumberFormat('de-DE').format(total) }}</span>
    </div>
  </div>
</template>

<script>
export default {
  props: ["total", "topCategories", "activeData"],
  computed: {
    categoryData() {
      const data = this.topCategories.map((cat) => {
        return {
          percent: this.percentage(this.total, cat[this.activeData]),
          count: cat.count,
          name: cat.name,
          amount: cat.amount,
        };
      });
      return data;
    },
  },
  methods: {
    percentage(total, count) {
      return (count / (total / 100)).toFixed(1);
    },
  },
};
</script>

<style scoped>
.barGraph {
  display: grid;
  width: 100%;
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

.barRow {
  display: grid;
  grid-template-rows: 1fr 1fr;
  grid-template-columns: 1fr;
}

.barRow p {
  padding: 0;
  margin: 0;
  font-size: 1.2em;
  color: var(--color-dark);
  color: #085b7e;
  text-transform: uppercase;
}

.bar {
  width: 100%;
  height: 70%;
  background-color: rgb(175, 175, 175);
  border-radius: 6px;
  justify-content: center;
  position: relative;
  overflow: hidden;
  /* border-bottom: 1px solid #7e20081c; */
}

.progress {
  background-color: #ffad00;
  background-color: var(--color-alert);
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
}

.amount {
  float: right;
  position: relative;
  font-weight: bold;
  height: 100%;
  display: grid;
  align-items: center;
  padding: 0 0.3em;
  color: var(--color-dark);
  color: #085b7e;
  z-index: 2;
}

.total {
  margin-top: 1em;
  width: 100%;
  text-align: right;
  color: #085b7e;
  font-weight: bold;
}
.total span {
  text-decoration: underline;
  font-size: 1.2em;
  border-bottom: 1px double #085b7e;
  margin-left: 1em;
}
</style>
