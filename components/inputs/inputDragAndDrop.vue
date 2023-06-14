<template>
  <div>
    <div id="drop-area" @dragover.prevent @dragenter.prevent @drop="handleDrop">
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
  methods: {
    handleFiles(e) {
      let files = e.target.files || e.dataTransfer.files;
      if (!files.length) return;
      this.createFile(files[0]);
    },

    handleDrop(e) {
      let files = e.dataTransfer.files;
      if (!files.length) return;
      this.createFile(files[0]);
    },

    createFile(file) {
      if (file.type === "text/plain") {
        let reader = new FileReader();

        reader.onload = (e) => {
          console.log(e.target.result);
          alert("cheguei aqui");
        };

        reader.readAsText(file);
      } else {
        console.log("Formato de arquivo não suportado");
      }
    },
  },
};
</script>

<style scoped>
#drop-area {
  width: 200px;
  height: 100px;
  border: 2px dashed var(--white-text);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  gap: 10px;
}
</style>
