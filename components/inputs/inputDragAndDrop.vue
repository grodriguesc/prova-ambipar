<template>
  <div class="input-size">
    <div
      id="drop-area"
      @dragover.prevent
      @dragenter="dragEnter"
      @dragleave="dragLeave"
      @mouseenter="mouseEnter"
      @mouseleave="mouseLeave"
      @drop="handleDrop"
    >
      <input
        type="file"
        id="fileElem"
        accept=".txt"
        @change="handleFiles"
        style="display: none"
      />
      <label class="button" for="fileElem">Selecione um arquivo</label>
      <p>ou arraste e solte aqui</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dragCounter: 0,
      mouseCounter: 0,
    };
  },
  methods: {
    handleFiles(e) {
      let files = e.target.files || e.dataTransfer.files;
      if (!files.length) return;
      this.createFile(files[0]);
    },

    handleDrop(e) {
      e.preventDefault();
      e.target.classList.remove("highlight");
      this.dragCounter = 0;
      let files = e.dataTransfer.files;
      if (!files.length) return;
      this.createFile(files[0]);
    },

    createFile(file) {
      if (file.type === "text/plain") {
        let reader = new FileReader();

        reader.onload = (e) => {
          const lines = e.target.result.split("\n");

          const result = {
            hitpointsHealed: 0,
            damageTaken: { total: 0, byCreatureKind: {}, creatureImages: {} },
            blackKnightHitpoints: [],
            blackKnightHitpoints: 0,
            unknownDamage: 0,
            experienceGained: 0,
            loot: {},
          };

          let knownDamage = 0;
          let currentBlackKnightDamage = 0;
          const creatureImages = {};

          lines.forEach((line) => {
            // Matches "You healed yourself for X hitpoints."
            const healed = line.match(
              /You healed yourself for (\d+) hitpoints./
            );
            if (healed) {
              result.hitpointsHealed += parseInt(healed[1]);
            }

            // Matches "You lose X hitpoints due to an attack by a Y."
            const damageByCreature = line.match(
              /You lose (\d+) hitpoints due to an attack by a ([^.]+)./
            );
            if (damageByCreature) {
              const damage = parseInt(damageByCreature[1]);
              const creature = damageByCreature[2];
              result.damageTaken.total += damage;
              knownDamage += damage;
              if (result.damageTaken.byCreatureKind[creature]) {
                result.damageTaken.byCreatureKind[creature] += damage;
              } else {
                result.damageTaken.byCreatureKind[creature] = damage;
                result.damageTaken.creatureImages[
                  creature
                ] = `https://static.tibia.com/images/library/${creature
                  .replace(/ /g, "")
                  .toLowerCase()}.gif`;
              }
            }

            // Matches "You lose X hitpoints."
            const damage = line.match(/You lose (\d+) hitpoints./);
            if (damage && !damageByCreature) {
              result.damageTaken.total += parseInt(damage[1]);
            }

            // Matches "You gained X experience points."
            const exp = line.match(/You gained (\d+) experience points./);
            if (exp) {
              result.experienceGained += parseInt(exp[1]);
            }

            // Matches "Loot of a Y: X."
            const lootStringMatch = line.match(/Loot of a .+: (.+)./);

            if (lootStringMatch) {
              const lootString = lootStringMatch[1];

              if (lootString === "nothing") {
                return;
              }

              const lootItems = lootString.split(","); // This will give us an array of loots

              lootItems.forEach((lootItem) => {
                const loot = lootItem.trim().match(/(\d*)\s*(.+)/);

                if (loot) {
                  const count = parseInt(loot[1]) || 1; // If no count is given, assume 1
                  let item = loot[2];

                  // Remove "a" or "an" prefix from the item
                  item = item.replace(/^(a|an)\s/, "");

                  if (result.loot[item]) {
                    result.loot[item] += count;
                  } else {
                    result.loot[item] = count;
                  }
                }
              });
            }

            const blackKnightDamage = line.match(
              /A Black Knight loses (\d+) hitpoints due to your attack./
            );
            if (blackKnightDamage) {
              currentBlackKnightDamage += parseInt(blackKnightDamage[1]);
            }

            // Matches "Loot of a Black Knight: X Y."
            const blackKnightLoot = line.match(/Loot of a Black Knight: .+/);
            if (blackKnightLoot) {
              result.blackKnightHitpoints = Math.max(
                result.blackKnightHitpoints,
                currentBlackKnightDamage
              );
              currentBlackKnightDamage = 0; // reset for the next Black Knight
            }
          });
          console.log(result.blackKnightHitpoints);
          result.unknownDamage = result.damageTaken.total - knownDamage;

          this.$emit("data-calculated", result);
          this.$emit("creature-images", creatureImages); // New event for creature images
        };

        reader.readAsText(file);
      } else {
        alert("Formato de arquivo não suportado");
      }
    },

    dragEnter(e) {
      this.dragCounter++;
      if (this.dragCounter === 1) {
        e.target.classList.add("highlight");
      }
    },

    dragLeave(e) {
      this.dragCounter--;
      if (this.dragCounter === 0) {
        e.target.classList.remove("highlight");
      }
    },
    mouseEnter(e) {
      this.mouseCounter++;
      if (this.mouseCounter === 1) {
        e.target.classList.add("highlight");
      }
    },

    mouseLeave(e) {
      this.mouseCounter--;
      if (this.mouseCounter === 0) {
        e.target.classList.remove("highlight");
      }
    },
  },
};
</script>

<style scoped>
.input-size {
  width: 80%;
}
#drop-area {
  height: 100px;
  border: 2px dashed var(--white-text);
  border-radius: 8px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

#drop-area.highlight {
  background-color: var(--white-text);
}
</style>
