<template>
  <div class="card-wrapper">
    <div class="cardEnd">
      <img
        :src="require('../../../img/103.svg')"
        alt="loading animation"
        v-if="!dataLoaded"
      />
    </div>
  </div>
</template>

<script>
export default {
  props: ["dataLoaded"],
};
</script>

<style scoped>
.card-wrapper {
  width: 100%;
}
.cardEnd {
  position: relative;
  border-bottom-left-radius: 4px;
  border-bottom-right-radius: 4px;
  border: 0;
  width: 100%;
  min-height: 40px;
  transition: all 0.6s cubic-bezier(0.53, 0.05, 0.56, 1.17);
  text-align: center;
  padding: 1em;
  cursor: auto;
  display: grid;
  justify-content: center;
  align-items: center;
  color: #f9f8f5;
  color: var(--color-light);
  border-bottom: 1px solid rgba(28, 31, 32, 0.9);
  border-top: 1px solid rgba(163, 222, 240, 0.9);
  background: -moz-linear-gradient(
    270deg,
    rgba(9, 156, 189, 1) 0%,
    rgba(8, 91, 126, 1) 74%,
    rgba(6, 77, 107, 1) 100%
  ); /* ff3.6+ */
  background: -webkit-gradient(
    linear,
    left top,
    left bottom,
    color-stop(0%, rgba(9, 156, 189, 1)),
    color-stop(74%, rgba(8, 91, 126, 1)),
    color-stop(100%, rgba(6, 77, 107, 1))
  ); /* safari4+,chrome */
  background: -webkit-linear-gradient(
    270deg,
    rgba(9, 156, 189, 1) 0%,
    rgba(8, 91, 126, 1) 74%,
    rgba(6, 77, 107, 1) 100%
  ); /* safari5.1+,chrome10+ */
  background: -o-linear-gradient(
    270deg,
    rgba(9, 156, 189, 1) 0%,
    rgba(8, 91, 126, 1) 74%,
    rgba(6, 77, 107, 1) 100%
  ); /* opera 11.10+ */
  background: -ms-linear-gradient(
    270deg,
    rgba(9, 156, 189, 1) 0%,
    rgba(8, 91, 126, 1) 74%,
    rgba(6, 77, 107, 1) 100%
  ); /* ie10+ */
  background: linear-gradient(
    180deg,
    rgba(9, 156, 189, 1) 0%,
    rgba(8, 91, 126, 1) 74%,
    rgba(6, 77, 107, 1) 100%
  ); /* w3c */
  filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#099CBD', endColorstr='#064D6B',GradientType=0 ); /* ie6-9 */
}
</style>