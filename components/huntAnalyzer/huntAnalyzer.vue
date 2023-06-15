<template>
  <div class="hunt-analyzer pixel-font">
    <h1>Hunt Analyzer</h1>
    <table>
      <tr>
        <th>Monstro</th>
        <th>Dano Recebido</th>
      </tr>
      <tr
        v-for="(damage, monster) in dataCalculated.damageTaken.byCreatureKind"
        :key="monster"
      >
        <td class="container tibia-icon">
          <img
            :src="dataCalculated.damageTaken.creatureImages[monster]"
            :alt="monster"
          />
          <p>{{ monster }}</p>
        </td>
        <td>
          <p class="container">{{ damage }}</p>
        </td>
      </tr>
    </table>

    <h1>Loot Analyzer</h1>

    <table>
      <tr>
        <th>Loot</th>
        <th>Quantidade</th>
      </tr>
      <tr v-for="(quantity, loot) in dataCalculated.loot" :key="loot">
        <td class="container tibia-icon">
          <img
            v-if="getItemImageUrl(loot)"
            width="32"
            height="32"
            :src="getItemImageUrl(loot)"
            alt="Imagem do item"
          />
          <p>{{ loot }}</p>
        </td>
        <td>
          <p class="container">{{ quantity }}</p>
        </td>
      </tr>
    </table>
    <div>
      <div class="total-damage">
        <h2>Dano total recebido: {{ dataCalculated.damageTaken.total }}</h2>
      </div>

      <div class="unknown-damage" v-if="dataCalculated.unknownDamage">
        <h2>
          Dano de origens desconhecidas: {{ dataCalculated.unknownDamage }}
        </h2>
      </div>
      <h2>Experiência total ganha: {{ dataCalculated.experienceGained }}</h2>

      <h2
        class="black-knight-img"
        v-if="dataCalculated.blackKnightHitpoints > 0"
      >
        <img width="64" src="static\creatureGifs\Black_Knight.gif" />
        Vida total do Black Knight:
        {{ dataCalculated.blackKnightHitpoints }}
      </h2>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    dataCalculated: {
      type: Object,
      default: {
        hitpointsHealed: 0,
        damageTaken: { total: 0, byCreatureKind: {}, creatureImages: {} },
        blackKnightHitpoints: [],
        blackKnightHitpoints: 0,
        unknownDamage: 0,
        experienceGained: 0,
        loot: {},
      },
    },
  },
  data() {
    return {
      itemIds: {},
    };
  },
  mounted() {
    // Get item ID and build image URL
    fetch("http://localhost:3001/api/getItems/item-list")
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        this.itemIds = data;
      })
      .catch((e) => console.log(e));
  },

  methods: {
    getItemImageUrl(itemName) {
      const itemNameLower = itemName.toLowerCase();
      let itemId;

      // Verifique se o nome do item existe
      if (this.itemIds.hasOwnProperty(itemNameLower)) {
        itemId = this.itemIds[itemNameLower];
      } else {
        // Se não existir, adicione um 's' no final do nome do item
        const pluralItemName = itemNameLower + "s";
        itemId = this.itemIds[pluralItemName];
      }

      return itemId
        ? `https://static.tibia.com/images/charactertrade/objects/${itemId}.gif`
        : null;
    },
  },
};
</script>

<style scoped>
.hunt-analyzer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-top: 30px;
  width: 80%;
  background-image: url(static/huntAnalyzer/background-client.png);
  color: var(--white-text);
  padding: 1em;
  border: 2px;
  border-style: solid;
  border-color: var(--secondary-color);
}

table {
  width: 100%;
  margin-top: 1em;
  border-collapse: collapse;
}

.tibia-icon {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

td,
th {
  border: 1px solid white;
  padding: 0.5em;
}
@media (max-width: 768px) {
  .black-knight-img {
    margin-left: auto !important;
    margin-top: auto !important;
    display: flex;
    flex-direction: column-reverse;
    align-items: center;
    justify-content: center;
  }
}

.black-knight-img {
  margin-left: -75px;
  margin-top: -49px;
}
</style>
