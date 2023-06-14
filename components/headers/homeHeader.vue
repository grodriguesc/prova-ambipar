<template>
  <div>
    <div class="box">
      <img
        src="static\header\corner-tl.gif"
        alt="corner"
        class="corner top-left"
      />
      <img
        src="static\header\corner-tr.gif"
        alt="corner"
        class="corner top-right"
      />
      <img
        src="static\header\corner-tr.gif"
        alt="corner"
        class="corner bottom-left"
      />
      <img
        src="static\header\corner-tl.gif"
        alt="corner"
        class="corner bottom-right"
      />
      <div alt="border" class="border"></div>
      <div class="border-bottom"></div>
      <div style="justify-self: center">Tibia Log Parser</div>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style>
header {
  color: var(--white-text);
  margin-bottom: 60px;
  position: relative;
  padding: 5px 3rem;
  display: flex;
  justify-content: center;
  align-items: center;
  background-repeat: repeat-x;
  background-size: contain;
  background-image: url("static/header/cacheinfo_background.gif");
  height: 50px;
}

header .content {
  z-index: 1;
}

.corner {
  position: absolute;
  width: 20px;
  height: 20px;
  z-index: 2;
}

.border {
  position: absolute;
  top: 0;
  background-image: url("static/header/border-1.gif");
  height: 6px;
  width: 100%;
  background-repeat: repeat-x;
  z-index: 1;
}

.border-bottom {
  position: absolute;
  bottom: 0;
  background-image: url("static/header/border-1.gif");
  height: 6px;
  width: 100%;
  background-repeat: repeat-x;
  z-index: 1;
}

.corner {
  position: absolute;
  width: 20px;
  height: 20px;
}

.top-left {
  top: 0;
  left: 0;
}

.top-right {
  top: 0;
  right: 0;
}

.bottom-left {
  -webkit-transform: rotate(180deg);
  bottom: 0;
  left: 0;
}

.bottom-right {
  -webkit-transform: rotate(180deg);
  bottom: 0;
  right: 0;
}
.border {
  position: absolute;
  top: 0;
  left: 0;
  background-image: url("static/header/border-1.gif");
  height: 6px;
  width: 100%;
  background-repeat: repeat-x;
  z-index: 1; /* Isso garante que as bordas apareçam abaixo do conteúdo e imagens de canto */
}

/* Adicione uma classe para a borda inferior */

.border-bottom {
  position: absolute;
  bottom: 0;
  left: 0;
  background-image: url("static/header/border-1.gif");
  height: 6px;
  width: 100%;
  background-repeat: repeat-x;
  z-index: 1;
}
</style>
