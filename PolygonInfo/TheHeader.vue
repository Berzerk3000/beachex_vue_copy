<template>
  <div class="heightOfTitleArea">
    <div class="headerTitle">
      <h1>{{ title }}</h1>
      <p>Fundmeldungen:</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["title"],
};
</script>

<style scoped>
.card-wrapper {
  width: 100%;
}
.headerTitle {
  position: relative;
  border-top-left-radius: 4px;
  border-top-right-radius: 4px;
  width: 100%;
  transition: all 0.6s cubic-bezier(0.53, 0.05, 0.56, 1.17);
  text-align: left;
}
.headerTitle h1{
  line-height:0.9em;
  /* with background color it should be 1 em to align with back button */
}
@media screen and (max-width:830px){
  .headerTitle h1 {
    font-size: 2.5em;
  }
}
@media screen and (max-width:700px){
  .headerTitle h1 {
    font-size: 2em;
  }
}
</style>