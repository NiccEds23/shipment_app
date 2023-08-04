<template>
  <div class="content">
    <div class="payment-process">
      <ul class="breadcrumb">
        <li class="delivery done">
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
        <DeliveryDetails
          v-if="paymentState == 'delivery'"
          @setDropshipper="setDropshipper"
          @setFormDeliveryStat="setFormDeliveryStat"
        />
        <ShipmentPayment
          v-if="paymentState == 'payment'"
          @setShipment="setShipment"
          @setPayment="setPayment"
          @changeState="changeState"
        />
        <ThankYou v-if="paymentState == 'finish'" @changeState="changeState" />
      </div>
      <!-- Summary Section -->
      <div class="summary">
        <div class="title-sec">
          <h2>Summary</h2>
          <p>10 items purchase</p>
        </div>
        <div class="delivery-est" :class="{ hidden: !shipmentData.courier }">
          <div class="decoration" />
          <p class="title">Delivery estimation</p>
          <p class="estimation">{{ shipmentData.estimation }} by {{ shipmentData.courier }}</p>
        </div>
        <div class="payment-method" :class="{ hidden: !paymentMethod }">
          <div class="decoration" />
          <p class="title">Payment method</p>
          <p class="method">{{ paymentMethod }}</p>
        </div>
        <div class="calculation">
          <div class="cost-goods">
            <p class="text">Cost of goods</p>
            <p class="price">{{ setDelimeter(goodsPrice) }}</p>
          </div>
          <div class="dropship-fee" v-if="dropshipper">
            <p class="text">Dropshipping Fee</p>
            <p class="price">{{ setDelimeter(dropshipPrice) }}</p>
          </div>
          <div class="shipment" v-if="shipmentData.courier">
            <p class="text">
              <b>{{ shipmentData.courier }}</b> shipment
            </p>
            <p class="price">{{ setDelimeter(shipmentData.price) }}</p>
          </div>
          <div class="total">
            <h2 class="text">Total</h2>
            <h2 class="price">{{ setDelimeter(totalPrice) }}</h2>
          </div>
        </div>
        <div class="action-btn">
          <button
            class="primary delivery"
            :disabled="!formDeliveryCompleted"
            v-if="paymentState == 'delivery'"
            @click="changeState('payment')"
          >
            Continue to Payment
          </button>
          <button
            class="primary payment"
            :disabled="!shipmentPaymentCompleted"
            v-if="paymentState == 'payment'"
            @click="changeState('finish')"
          >
            <p v-if="paymentMethod">
              Pay with <b>{{ paymentMethod }}</b>
            </p>
            <p v-else>(Please select payment method first)</p>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import DeliveryDetails from '@/components/FormComponents/DeliveryDetails.vue'
import ShipmentPayment from '@/components/FormComponents/ShipmentPayment.vue'
import ThankYou from '@/components/FormComponents//ThankYou.vue'
import numeral from 'numeral'
export default {
  data() {
    return {
      paymentState: '',
      dropshipper: false,
      formDeliveryCompleted: false,
      goodsPrice: 500000,
      dropshipPrice: 0,
      shipmentPrice: 0,
      shipmentData: {
        estimation: '',
        courier: '',
        price: 0
      },
      paymentMethod: ''
    }
  },
  components: { DeliveryDetails, ShipmentPayment, ThankYou },
  mounted() {
    if (!localStorage.getItem('paymentState')) {
      localStorage.setItem('paymentState', 'delivery')
    }
    this.paymentState = localStorage.getItem('paymentState')
    this.dropshipper = localStorage.getItem('dropshipCheck') == 'true' ? true : false
    this.dropshipPrice = localStorage.getItem('dropshipPrice') ?? 0
    this.shipmentData.courier = localStorage.getItem('shipmentCourier') ?? ''
    this.shipmentData.estimation = localStorage.getItem('shipmentEstimation') ?? ''
    this.shipmentData.price = localStorage.getItem('shipmentPrice') ?? 0
    this.paymentMethod = localStorage.getItem('paymentMethod') ?? ''
    this.tempShipmentPrice = localStorage.getItem('shipmentPrice') ?? 0
    if (this.dropshipper) {
      this.setDropshipper(true)
    }
  },
  computed: {
    shipmentPaymentCompleted() {
      if (this.shipmentData.courier && this.paymentMethod) return true
      return false
    },
    totalPrice() {
      let result =
        this.goodsPrice + parseInt(this.dropshipPrice) + parseInt(this.shipmentData.price)
      return result
    }
  },
  methods: {
    setDelimeter(val) {
      return numeral(val).format('(0,0[.]000)')
    },
    setDropshipper(val) {
      this.dropshipper = val
      this.dropshipPrice = 5900
    },
    setFormDeliveryStat(val) {
      this.formDeliveryCompleted = val
    },
    setShipment(val) {
      this.shipmentData.estimation = val.estimation
      this.shipmentData.courier = val.courier
      this.shipmentData.price = val.price
    },
    setPayment(val) {
      this.paymentMethod = val
    },
    changeState(val) {
      this.paymentState = val
      localStorage.setItem('paymentState', val)
    }
  },
  watch: {
    dropshipper(val) {
      localStorage.setItem('dropshipPrice', val)
    }
  }
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
      margin-top: 4vh;
      padding: 0 3vh;
      width: 26%;
      border-left: 0.5px solid #ff8a00;
      .title-sec {
        p {
          margin-top: -10px;
          font-family: 'InterUIRegular';
          font-size: 14px;
        }
      }
      .delivery-est {
        visibility: visible;
        .decoration {
          margin: 3vh 0;
          background-color: #eeeeee;
          height: 2px;
          width: 100px;
        }
        .title {
          margin-bottom: 0;
          font-family: 'InterUIRegular';
        }
        .estimation {
          margin-top: 0;
          font-family: 'InterUIMedium';
          color: #1bd97b;
        }
      }
      .delivery-est.hidden {
        visibility: hidden;
      }
      .payment-method {
        visibility: visible;
        .decoration {
          margin: 3vh 0;
          background-color: #eeeeee;
          height: 2px;
          width: 100px;
        }
        .title {
          margin-bottom: 0;
          font-family: 'InterUIRegular';
        }
        .method {
          margin-top: 0;
          font-family: 'InterUIMedium';
          color: #1bd97b;
        }
      }
      .payment-method.hidden {
        visibility: hidden;
      }
      .calculation {
        margin-top: 6vh;
        .cost-goods {
          display: flex;
          justify-content: space-between;
          .text {
            font-family: 'InterUIRegular';
          }
          .price {
            font-family: 'InterUIBold';
          }
        }
        .dropship-fee {
          display: flex;
          justify-content: space-between;
          .text {
            font-family: 'InterUIRegular';
          }
          .price {
            font-family: 'InterUIBold';
          }
        }
        .shipment {
          display: flex;
          justify-content: space-between;
          .text {
            font-family: 'InterUIRegular';
          }
          .price {
            font-family: 'InterUIBold';
          }
        }
        .total {
          display: flex;
          justify-content: space-between;
        }
      }
    }
  }
}
</style>
