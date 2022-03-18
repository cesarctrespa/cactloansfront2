<template>
  <div class="card justify-content-center">
    <div class="card-body ">
      <div class="row mb-3"><h3>Simula tu préstamo</h3></div>

      <div class="row mb-1"><i class="icon fas fa-hand-holding-dollar p-0"/></div>
      <div class="row align-self-center"><p class="questions">¿Cuánta plata necesitas?</p></div>
      <div class="row mb-3 justify-content-center">
        <div class="col-11 col-sm-7 col-md-5">
          <input type="number" class="form-control number" id="amount" placeholder="000000" step="100000" min="100000" max="25000000">
        </div>
      </div>
      <div class="row"><p class="notes">Ingresa tu valor de $100.000 en $100.000</p></div>
      <div class="row mb-2">
        <div class="col-3 col-sm-3 mt-1"><p class="values">$100.000</p></div>
        <div class="col-6 col-sm-6">
          <input type="range" min="1" max="100" value="50" class="slider" id="myRange">
        </div>
        <div class="col-3 col-sm-3 mt-1"><p class="values">$25.000.000</p></div>
      </div>

      <div class="row mb-1"><i class="icon fas fa-calendar-days p-0"/></div>
      <div class="row align-self-center"><p class="questions">¿Cuándo y cuánto te queda fácil pagar en cada cuota?</p></div>
      <div class="row mb-3 justify-content-center">
        <div class="col-12 col-sm-6">
          <label class="notes mb-1">Fecha primera cuota</label>
          <div class="row justify-content-center">
            <div class="col-1 mt-auto"><i class="icon fas fa-calendar-check"/></div>
            <div class="col-6"><datepicker v-model="picked" inputFormat="dd/MM/yyyy"/></div>   
          </div>
        </div>
        <div class="col-12 col-sm-6">
          <label class="notes mb-1">Valor máximo a pagar</label>
          <div class="row justify-content-center">
            <div class="col-1 mt-auto"><i class="icon fas fa-money-bill-1"/></div>
            <div class="col-6 mt-auto"><input type="number" class="form-control number p-0" id="payment" placeholder="000000" step="10000" min="100000" max="25000000"></div>
          </div>
        </div>
      </div>

      <div class="row mb-1"><i class="icon fas fa-circle-info p-0"/></div>
      <div class="row align-self-center"><p class="questions">Así quedaría tu préstamo</p></div>
      <div class="row mb-3 justify-content-center">
        <div class="row">
          <div class="col-7 col-sm-6 align-self-center"><p class="label-informative text-end mb-1">La plata que necesitas</p></div>
          <div class="col-5 col-sm-3"><input type="number" readonly class="form-control-plaintext informative pt-0 pb-0" id="amount" value="00000"></div>
        </div>
        <div class="row">
          <div class="col-7 col-sm-6 align-self-center"><p class="label-informative text-end mb-1">Los intereses</p></div>
          <div class="col-5 col-sm-3"><input type="number" readonly class="form-control-plaintext informative pt-0 pb-0" id="interest" value="00000"></div>
        </div>
        <div class="row">
          <div class="col-7 col-sm-6 align-self-center"><p class="label-informative text-end fw-bold mb-1">Lo que pagarías en total</p></div>
          <div class="col-5 col-sm-3"><input type="number" readonly class="form-control-plaintext informative pt-0 pb-0" id="total" value="00000"></div>
        </div>
        <div class="row">
          <div class="col-7 col-sm-6 align-self-center"><p class="label-informative text-end fw-bold mb-1">Fecha límite del pago total</p></div>
          <div class="col-5 col-sm-5"><input type="date" readonly class="form-control-plaintext informative pt-0 pb-0" id="limitDate" value="2022-06-18"></div>        
        </div>
      </div>

      <button class="btn btn-primary" type="submit">Ir al plan de pago</button>

      <div class="container row align-self-center p-3"><p class="notes mb-0">Los resultados de esta simulación son de uso informativo y aproximado. Los valores podrán variar de acuerdo a las políticas de estudio, los intereses se liquidarán con la tasa vigente al momento del desembolso.</p></div>
      
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Simulation',
  }

  /*var slider = document.getElementById("myRange");
  var output = document.getElementById("demo");
  output.innerHTML = slider.value; // Display the default slider value

  // Update the current slider value (each time you drag the slider handle)
  slider.oninput = function() {
    output.innerHTML = this.value;
  }*/
</script>
<script setup>
  import Datepicker from 'vue3-datepicker'
  import { ref } from 'vue'
  const picked = ref(new Date())
</script>

<style>
  :root {
    --vdp-hover-bg-color : #9dc9dd;
    --vdp-selected-bg-color: #9dc9dd;
  }
</style>

<style lang="scss">
  .card {
    border-radius: 1.5rem !important;
    -webkit-box-shadow: 0 0 5px 4px rgba(0, 0, 0, 0.1);
    -moz-box-shadow: 0 0 5px 4px rgba(0, 0, 0, 0.1);
    box-shadow: 0 0 5px 4px rgba(0, 0, 0, 0.1);
  }
  h3 {
    font-weight: 700;
  }
  .btn-primary {
    background: linear-gradient(to right, #ED1826 50%, #da1623);
    border-radius: 1.5rem !important;
    border: none !important;
    font-family: 'Roboto' !important;
    font-size:calc(7px + 0.7vw) !important;
  }
  .btn-primary:focus {   
    border: none !important;
    box-shadow: 0 1px 1px rgba(0, 0, 0, 0.075) inset, 0 0 8px #ED1826 !important;
    outline: 0 none !important;
  }
  .questions {
    font-weight: 300;
  }
  .label-informative {
    font-weight: 300;
    font-size:calc(7px + 0.75vw);
  }
  .notes {
    font-weight: 300;
    font-size: 0.7rem;
  }
  .form-control.number {
    border: 1px #F1F1F1;
    border-bottom-style: solid;
    text-align: center;
    font-size:calc(8px + 1vw);
  }
  .form-control-plaintext.informative, .values {
      font-size:calc(5px + 1vw);
  }
  .slider {
    -webkit-appearance: none;
    width: 100%;
    height: 25px;
    background: #F1F1F1;
    outline: none;
    opacity: 0.7;
    -webkit-transition: .2s;
    transition: opacity .2s;
  }
  .slider:hover {
    opacity: 1;
  }
  .slider::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    width: calc(10px + 1.5vw);
    height: 25px;
    background: #9dc9dd;
    cursor: pointer;
  }
  .slider::-moz-range-thumb {
    width: calc(10px + 1.5vw);
    height: 25px;
    background: #9dc9dd;
    cursor: pointer;
  }
  
  .v3dp__datepicker{
    input{
      border: 1px #F1F1F1;
      border-bottom-style: solid;
      text-align: center; 
      font-size:calc(8px + 1vw);
      margin-top: auto;
      width: 100%;
    }
    font-size:calc(6px + 0.72vw);
  }

  input[type="text"]:focus,
  input[type="number"]:focus {   
    border-color: #9dc9dd;
    box-shadow: 0 1px 1px rgba(0, 0, 0, 0.075) inset, 0 0 8px #9dc9dd;
    outline: 0 none;
  }

  .fa-calendar-check, .fa-money-bill-1 {
      color: #80B3BA;
  }
</style>
