<template v-if="form">
  <div class="card justify-content-center">
    <div class="card-body">
      <div class="row mb-3"><h3>Simula tu préstamo</h3></div>

      <div class="row mb-1"><i class="icon fas fa-hand-holding-dollar p-0"/></div>
      <div class="row align-self-center"><p class="questions">¿Cuánta plata necesitas?</p></div>
      <div class="row mb-3 justify-content-center">
        <div v-if="!controls.amount" class="col-11 col-sm-7 col-md-5">
          <label @click="changeControlStatus('amount')" class="form-control number" id="amountShow">
            {{NumberFormat(form.amount)}}
          </label>
        </div>
        <div v-else class="col-11 col-sm-7 col-md-5">
          <input v-model="form.amount" type="number" class="form-control number" id="amount" placeholder="0" :min="amountLimits.min" :max="amountLimits.max" :step="amountLimits.step" @keyup="FixNumberAmount()" @change="FixNumberAmount()">
        </div>
      </div>
      <div class="row"><p class="notes">Ingresa tu valor de {{NumberFormat(amountLimits.step)}} en {{NumberFormat(amountLimits.step)}}</p></div>
      <div class="row mb-3">
        <div class="col-3 px-0 slider-content">
          <span class="values">{{NumberFormat(amountLimits.min)}}</span>
        </div>
        <div class="col-6 px-1 slider-content">
          <input v-model="form.amount" type="range" class="slider" :min="amountLimits.min" :max="amountLimits.max" :step="amountLimits.step" id="amountRange">
        </div>
        <div class="col-3 px-0 slider-content">
          <span class="values">{{NumberFormat(amountLimits.max)}}</span>
        </div>
      </div>

      <div class="row mb-1"><i class="icon fas fa-calendar-days p-0"/></div>
      <div class="row align-self-center"><p class="questions">¿Cuándo y cuánto te queda fácil pagar en cada cuota?</p></div>
      <div class="row mb-3 justify-content-center">
        <div class="col-12 col-sm-6">
          <label class="notes mb-1">Fecha primera cuota</label>
          <div class="row justify-content-center">
            <div class="col-1 mt-auto"><i class="icon fas fa-calendar-check"/></div>
            <div class="col-6"><datepicker v-model="form.firstDate" inputFormat="dd/MM/yyyy"/></div>   
          </div>
        </div>
        <div class="col-12 col-sm-6">
          <label class="notes mb-1">Valor máximo a pagar por cuota</label>
          <div class="row justify-content-center">
            <div class="col-1 mt-auto"><i class="icon fas fa-money-bill-1"/></div>
            <div v-if="!controls.maxInstPayment" class="col-6 mt-auto">
              <label @click="changeControlStatus('maxInstPayment')" class="form-control number p-0" id="maxInstPaymentShow">
                {{NumberFormat(form.maxInstPayment)}}
              </label>
            </div>
            <div v-else class="col-6 mt-auto">
              <input v-model="form.maxInstPayment" type="number" class="form-control number p-0" id="maxInstPayment" placeholder="0" :min="amountLimits.min" :max="form.amount" :step="amountLimits.maxInstPaymentStep"  @keyup="FixNumberMaxInstPayment()" @change="FixNumberMaxInstPayment()">
            </div>
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
            <input :value="NumberFormat(form.amount)" type="text" readonly class="form-control-plaintext informative py-0" id="amountFinal">
          </div>
        </div>
        <div class="row">
          <div class="col-7 col-sm-6 align-self-center">
            <p class="label-informative text-end mb-1">Los intereses</p>
          </div>
          <div class="col-5 col-sm-3">
            <input :value="NumberFormat(form.interest)" type="text" readonly class="form-control-plaintext informative py-0" id="interest">
          </div>
        </div>
        <div class="row">
          <div class="col-7 col-sm-6 align-self-center">
            <p class="label-informative text-end fw-bold mb-1">Lo que pagarías en total</p>
          </div>
          <div class="col-5 col-sm-3">
            <input :value="NumberFormat(form.total)" type="text" readonly class="form-control-plaintext informative py-0" id="total">
          </div>
        </div>
        <div class="row">
          <div class="col-7 col-sm-6 align-self-center">
            <p class="label-informative text-end fw-bold mb-1">Cantidad de cuotas</p>
          </div>
          <div class="col-5 col-sm-3">
            <input :value="form.installments" type="text" readonly class="form-control-plaintext informative py-0" id="installments">
          </div>
        </div>
        <div class="row">
          <div class="col-7 col-sm-6 align-self-center">
            <p class="label-informative text-end fw-bold mb-1">Fecha límite del pago total</p>
          </div>
          <div class="col-5 col-sm-5">
            <input :value="form.limitDate" type="text" readonly class="form-control-plaintext informative py-0" id="limitDate">
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
      let amountLimits = ref({ min: 100000, max: 25000000, step: 1000, maxInstPaymentStep: 1000 });
      let form = ref({ amount: 100000, total: 12700000, maxInstPayment: 0 , firstDate: new Date(), interest: 100000, installments: 24, limitDate: "18/06/2024" });
      let controls = ref({ amount: false, maxInstPayment: false});

      function changeControlStatus(controlName) {
        if (controlName === "amount") {
          controls.value.amount = !controls.value.amount;
        } else if (controlName === "maxInstPayment") {
          controls.value.maxInstPayment = !controls.value.maxInstPayment;
        }   
      }

      var timeoutID;
      function FixNumberAmount() {  // Method to fix numbers related to amount
        if (typeof timeoutID === 'number') {
          cancel(timeoutID);
        }
        timeoutID = setTimeout(function() {
          let result = Math.round(form.value.amount/amountLimits.value.step)*amountLimits.value.step;
          if (result>amountLimits.value.max)
            form.value.amount = amountLimits.value.max;
          else if (result<amountLimits.value.min)
            form.value.amount = amountLimits.value.min;
          else
            form.value.amount = result;

          controls.value.amount = false;
          timeoutID = undefined;
        }.bind(this), 2500, '');
      }

      var timeoutID2;
      function FixNumberMaxInstPayment() { // Method to fix numbers related to max installment payment field
          if (typeof timeoutID2 === 'number') {
            cancel(timeoutID2);
          }
          timeoutID2 = setTimeout(function() {
            if (form.value.maxInstPayment>form.value.amount)
              form.value.maxInstPayment = form.value.amount;
            
            controls.value.maxInstPayment = false;
            timeoutID2 = undefined;
          }.bind(this), 2500, '');
      }

      function cancel(id) {
        clearTimeout(id);
      }

      return { amountLimits, form, controls, changeControlStatus, FixNumberAmount, FixNumberMaxInstPayment };
    },
    computed: {
      NumberFormat() { // Method to add points to the thousands
        return (v)=>{
          return "$" + v.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".")
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
    cursor: pointer;
    border: 1px #F1F1F1 !important;
    border-bottom-style: solid !important;
    text-align: center !important;
    font-size:calc(8px + 1vw);
  }
  .form-control-plaintext.informative, .values {
      font-size:calc(7px + 1vw);
      white-space: nowrap;
      overflow: hidden;
  }
  .slider-content {
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .slider {
    -webkit-appearance: none;
    width: 100%;
    height: calc(5px + 0.75vw);
    border-radius: 5px;  
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
    height: calc(10px + 1.5vw);
    border-radius: 50%; 
    background: #9dc9dd;
    cursor: pointer;
  }
  .slider::-moz-range-thumb {
    width: calc(10px + 1.5vw);
    height: calc(10px + 1.5vw);
    border-radius: 50%; 
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
    font-size:calc(9px + 0.9vw);
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
