<template>
  <div class="bloc-modale" v-if="alertOn">
    <div class="overlay" v-on:click="alertOff"></div>
    <div class="modale card">
      <div v-on:click="alertOff" class="btn-modale btn btn-danger">x</div>
      <h2>Information incorrecte !</h2>
    </div>
  </div>
</template>

<script>

export default {
  name: 'ModaleAlert',
  //porps permet de reçevoir les dennées depuis les composents
  props: ['alertOn', 'alertOff']
}
</script>

<style>

.bloc-modale{
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.overlay{
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}

.modale{
  background: #f1f1f1;
  color: #333;
  padding: 50px;
  position: fixed;
  top: -10%;
}

.btn-modale{
  position: absolute;
  top: 10px;
  right: 10px;
}
</style>