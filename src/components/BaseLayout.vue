<template>
  <div class="content">
    <div class="payment-process">
      <ul class="breadcrumb">
        <li
          class="delivery"
          :class="paymentState == 'delivery' || paymentState == 'payment' ? 'done' : ''"
        >
          <div class="number">1</div>
          Delivery
        </li>
        <li
          class="payment"
          :class="paymentState == 'payment' || paymentState == 'finish' ? 'done' : ''"
        >
          <div class="number">2</div>
          Payment
        </li>
        <li class="Finish" :class="paymentState == 'finish' ? 'done' : ''">
          <div class="number">3</div>
          Finish
        </li>
      </ul>
    </div>
    <div class="main-content">
      <div class="form-components">
        <DeliveryDetails v-if="paymentState == 'delivery'" />
        <ShipmentPayment v-if="paymentState == 'payment'" />
        <ThankYou v-if="paymentState == 'finish'" />
      </div>
      <div class="summary">
        <div class="title-sec">
          <h2>Summary</h2>
          <p>10 items purchase</p>
        </div>
        <div class="delivery-est" v-if="paymentState == 'payment'">
          <p class="title">Delivery estimation</p>
          <p class="estimation"></p>
        </div>
        <div class="payment-method" v-if="paymentState == 'finish'">
          <p class="title">Payment method</p>
          <p class="method"></p>
        </div>
        <div class="calculation">
          <div class="cost-goods"></div>
          <div class="dropship-fee"></div>
          <div class="shipment"></div>
          <div class="total"></div>
        </div>
        <div class="action-btn">
          <button class="delivery"></button>
          <button class="payment"></button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import DeliveryDetails from '@/components/FormComponents/DeliveryDetails.vue'
import ShipmentPayment from '@/components/FormComponents/ShipmentPayment.vue'
import ThankYou from '@/components/FormComponents//ThankYou.vue'
export default {
  data() {
    return {
      paymentState: ''
    }
  },
  mounted() {
    console.log(localStorage.getItem('paymentState'))
    if (!localStorage.getItem('paymentState')) {
      localStorage.setItem('paymentState', 'delivery')
    }
    this.paymentState = localStorage.getItem('paymentState')
  },
  components: { DeliveryDetails, ShipmentPayment, ThankYou }
}
</script>

<style lang="scss">
.content {
  .payment-process {
    display: flex;
    justify-content: center;
    .breadcrumb {
      background-color: #fffae6;
      display: flex;
      height: 7vh;
      min-height: 50px;
      padding: 0 3vw;
      z-index: 2;
      position: absolute;
      border-radius: 50vw;
      li {
        display: inline-flex;
        align-items: center;
        line-height: 2;
        color: #ff8a00;
        font-family: 'InterUIMedium';
        .number {
          padding: 0 12px;
          margin-right: 1vw;
          background-color: rgba(#ff8a00, 0.2);
          border-radius: 20px;
        }
      }

      .done {
        .number {
          background-color: rgba(#ff8a00, 1);
          color: #fff;
        }
      }

      li + li::before {
        padding: 0 1vw;
        content: 'keyboard_arrow_right';
        font-family: 'MaterialIconsRegular';
      }
    }
  }

  .main-content {
    background-color: #fff;
    margin: 4.5vh;
    padding: 3vh 0;
    display: flex;
    flex-wrap: wrap;
    .form-components {
      padding-left: 4.5vh;
      padding-right: 3vh;
      width: 64%;
    }
    .summary {
      margin-top: 3vh;
      padding-left: 3vh;
      width: 25%;
      border-left: 0.5px solid #ff8a00;
    }
  }
}
</style>
