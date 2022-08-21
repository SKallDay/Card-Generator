<template>
  <div class="card" :row="row + 1" :col="col + 1">
    <p>{{ msg }}</p>
  </div>
</template>

<script>
export default {
  name: "Card",
  props: {
    msg: String,
    row: Number,
    col: Number,
  },
};
</script>

<style scoped>
.card {
  width: 60px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #FFFFFF;
  font-family: 'Roboto Slab';
  font-style: normal;
  font-weight: 900;
  font-size: 36px;
  line-height: 47px;
  border: 4px solid black;
}

@media (min-width: 375px) {
  .card {
    width: 66px;
    height: 65px;
  }
}
</style>
