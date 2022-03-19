<template v-if="form">
  <div class="card justify-content-center">
    <div class="card-body ">
      <div class="row mb-3"><h3>Simula tu préstamo</h3></div>

      <div class="row mb-1"><i class="icon fas fa-hand-holding-dollar p-0"/></div>
      <div class="row align-self-center"><p class="questions">¿Cuánta plata necesitas?</p></div>
      <div class="row mb-3 justify-content-center">
        <div class="col-11 col-sm-7 col-md-5">
          <input type="number" class="form-control number" id="amount" v-model="form.amount" placeholder="0" :min="amountLimits.min" :max="amountLimits.max" :step="amountLimits.step" @keyup="FixNumber()">
        </div>
      </div>
      <div class="row"><p class="notes">Ingresa tu valor de {{NumberFormat(amountLimits.step)}} en {{NumberFormat(amountLimits.step)}}</p></div>
      <div class="row mb-2">
        <div class="col-3 col-sm-3 mt-1"><p class="values">{{NumberFormat(amountLimits.min)}}</p></div>
        <div class="col-6 col-sm-6">
          <input type="range" class="slider" :min="amountLimits.min" :max="amountLimits.max" :step="amountLimits.step" id="amountRange" v-model="form.amount">
        </div>
        <div class="col-3 col-sm-3 mt-1"><p class="values">{{NumberFormat(amountLimits.max)}}</p></div>
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
          <div class="col-7 col-sm-6 align-self-center">
            <p class="label-informative text-end mb-1">La plata que necesitas</p>
          </div>
          <div class="col-5 col-sm-3">
            <input type="text" readonly class="form-control-plaintext informative pt-0 pb-0" id="amount" :value="NumberFormat(form.amount)">
          </div>
        </div>
        <div class="row">
          <div class="col-7 col-sm-6 align-self-center">
            <p class="label-informative text-end mb-1">Los intereses</p>
          </div>
          <div class="col-5 col-sm-3">
            <input type="text" readonly class="form-control-plaintext informative pt-0 pb-0" id="interest" :value="NumberFormat(form.interest)">
          </div>
        </div>
        <div class="row">
          <div class="col-7 col-sm-6 align-self-center">
            <p class="label-informative text-end fw-bold mb-1">Lo que pagarías en total</p>
          </div>
          <div class="col-5 col-sm-3">
            <input type="text" readonly class="form-control-plaintext informative pt-0 pb-0" id="total" :value="NumberFormat(form.total)">
          </div>
        </div>
        <div class="row">
          <div class="col-7 col-sm-6 align-self-center">
            <p class="label-informative text-end fw-bold mb-1">Cantidad de cuotas</p>
          </div>
          <div class="col-5 col-sm-3">
            <input type="text" readonly class="form-control-plaintext informative pt-0 pb-0" id="installments" :value="form.installments">
          </div>
        </div>
        <div class="row">
          <div class="col-7 col-sm-6 align-self-center">
            <p class="label-informative text-end fw-bold mb-1">Fecha límite del pago total</p>
          </div>
          <div class="col-5 col-sm-5">
            <input type="text" readonly class="form-control-plaintext informative pt-0 pb-0" id="limitDate" :value="form.limitDate">
          </div>        
        </div>
      </div>

      <button class="btn btn-primary" type="submit">Ir al plan de pago</button>

      <div class="container row align-self-center p-3"><p class="notes mb-0">Los resultados de esta simulación son de uso informativo y aproximado. Los valores podrán variar ya que los intereses se liquidarán con la tasa vigente al momento del desembolso.</p></div>
      
    </div>
  </div>
</template>

<script>
  import Datepicker from 'vue3-datepicker'
  import { ref } from "vue";

  export default {
    name: 'Simulation',
    components: {
      Datepicker
    },
    setup() {
      let amountLimits = ref({ min: 100000, max: 25000000, step: 50000 });
      let form = ref({ amount:100000, total:12700000, interest:100000, installments: 24, limitDate:"18/06/2024" });
      let picked = ref(new Date());

      return { form, picked, amountLimits };
    },
    computed: {
      NumberFormat() { // Method to add points to the thousands
        return (v)=>{
          return "$ " + v.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".")
        }
      },
      FixNumber() { // Method to fix numbers related to amount
        return ()=>{
          clearTimeout(undefined);
          setTimeout(()=>{
            let result = Math.round(this.form.amount/this.amountLimits.step)*this.amountLimits.step;
            if (result>this.amountLimits.max)
              this.form.amount = this.amountLimits.max;
            else if (result<this.amountLimits.min)
              this.form.amount = this.amountLimits.min;
            else
              this.form.amount = result;
          }, 4000);
        }
      }
    }
  }
</script>

<style lang="scss">
  :root {
    --vdp-hover-bg-color : #9dc9dd;
    --vdp-selected-bg-color: #9dc9dd;
  }
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
    font-size:calc(10px + 0.7vw) !important;
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
    font-size:calc(8px + 0.72vw);
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
