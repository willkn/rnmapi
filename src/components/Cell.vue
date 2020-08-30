<template>
  <div class="cell">
    <img v-if="this.$store.state.loading == true" id="portal" src="@/assets/portal.png" />
    <img v-if="this.$store.state.loading == false" :src="object.img" />
    <div id="text">
      <p>ID: {{ object.id }}</p>
      <p>Name: {{ object.name }}</p>
      <p>Gender: {{ object.gender }}</p>
      <p>Location: {{ object.location.name }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Cell",
  props: ["object"],
};
</script>