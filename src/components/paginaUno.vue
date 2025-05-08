<script lang="ts" setup>
const nextPage = () => {
  console.log('entra')
}
</script>

<style scoped lang="scss">
.content-container {
  border: 1px solid violet;
  font-size: 65px;
  color: white;
  text-align: center;
  position: relative;
  height: 100%;

  img {
    position: absolute;
    left: -150px;
  }

  button {
    transition: 0.3s ease;
    position: absolute;
    right: 0;
    bottom: 0;

    border: none;
    background-color: violet;
    color: white;
    font-size: 20px;
    padding: 10px 20px;
    border-radius: 8px;
    cursor: pointer;

    &:hover {
      transition: 0.3s ease;
      transform: scale(1.1);
    }

    &:active {
      transition: 0.3s ease;
      transform: scale(1);
    }
  }
}
</style>

<template>
  <div class="content-container">
    <h1>añsdf</h1>
    <span> hola </span>
    <button class="next" @click="nextPage">Continuar</button>
    <img
      src="https://media.canva.com/v2/image-resize/format:PNG/height:800/quality:100/uri:ifs%3A%2F%2F%2F6f3bd683-4e6c-402f-ba6c-3b4edd4be1ac/watermark:F/width:800?csig=AAAAAAAAAAAAAAAAAAAAAPDUL3mrPVbzNQXQSBht4XGVesD2Fz3Jnqt5qdLS-W4W&exp=1746025048&osig=AAAAAAAAAAAAAAAAAAAAABil3c2JzGuAK9DFSIFxuInr9BY1Dap0nmthyJcIck0R&signer=media-rpc&x-canva-quality=screen"
      alt=""
    />
  </div>
</template>
