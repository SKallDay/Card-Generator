<template>
  <div class="card flex items-center justify-center fw900 text-3xl " :row="row + 1" :col="col + 1">
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
  background-color: #FFFFFF;
  font-family: 'Roboto Slab';
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
