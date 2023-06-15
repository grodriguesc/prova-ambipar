<template>
  <div class="container">
    <InputDragAndDrop @data-calculated="updateData" />
    <HuntAnalyzer :data-calculated="dataCalculated" />
  </div>
</template>

<script>
import HuntAnalyzer from "~/components/huntAnalyzer/huntAnalyzer.vue";
import InputDragAndDrop from "~/components/inputs/inputDragAndDrop.vue";

export default {
  components: { InputDragAndDrop, HuntAnalyzer },
  data() {
    return {
      dataCalculated: {
        hitpointsHealed: 0,
        damageTaken: { total: 0, byCreatureKind: {} },
        blackKnightHitpoints: [],
        blackKnightHitpoints: 0,
        unknownDamage: 0,
        experienceGained: 0,
        loot: {},
      },
    };
  },
  methods: {
    updateData(newData) {
      console.log(newData);
      this.dataCalculated = newData;
      console.log(this.dataCalculated);
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
