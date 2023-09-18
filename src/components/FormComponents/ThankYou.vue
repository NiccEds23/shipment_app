<template>
  <div class="canvas">
    <div>
      <div class="content-heading" v-if="countdown == 0 || paymentMethod == 'e-Wallet'">
        <div class="title">
          <h1>Thank you</h1>
          <div class="title-ornamen" />
        </div>
      </div>
      <section
        class="transfer-sec"
        v-if="
          countdown != 0 && (paymentMethod == 'Bank Transfer' || paymentMethod == 'Virtual Account')
        "
      >
        <div class="timer">
          <h1>{{ countdown }} s</h1>
        </div>
        <div class="recipient">
          <p v-if="paymentMethod == 'Bank Transfer'">Bank Istimewa. {{ bankNumber }}</p>
          <p v-else>Virtual Account. {{ vaNumber }}</p>
          <p>a.n. Nicholas Edson</p>
        </div>
        <div class="notes"><p>Please complete the payment before the times run out.</p></div>
      </section>
      <section class="order-sec" v-if="countdown == 0 || paymentMethod == 'e-Wallet'">
        <div class="order-id">
          <p>Order ID: {{ orderId }}</p>
        </div>
        <div class="courier">
          <p>
            Your order will be delivered {{ shipmentData.estimation }} with
            {{ shipmentData.courier }}
          </p>
        </div>
      </section>
      <div class="back-btn" v-if="countdown == 0 || paymentMethod == 'e-Wallet'">
        <button class="outlined" @click="backToDelivery()">
          <p class="icon">arrow_back</p>
          Go to homepage
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Thankyou-component',
  emits: ['changeState'],
  data() {
    return {
      orderId: '',
      vaNumber: '',
      bankNumber: '',
      countdown: 30,
      shipmentData: {
        courier: '',
        estimation: ''
      },
      paymentMethod: ''
    }
  },
  mounted() {
    this.shipmentData.courier = localStorage.getItem('shipmentCourier') ?? ''
    this.shipmentData.estimation = localStorage.getItem('shipmentEstimation') ?? ''
    this.paymentMethod = localStorage.getItem('paymentMethod') ?? ''
    if (this.paymentMethod == 'Bank Transfer' || this.paymentMethod == 'Virtual Account') {
      this.countdownTimer()
    }
    this.randomOrderID(5)
    this.bankNumber = this.randomNumber(10)
    this.vaNumber = this.randomNumber(15)
  },
  methods: {
    countdownTimer() {
      if (this.countdown > 0) {
        setTimeout(() => {
          this.countdown -= 1
          this.countdownTimer()
        }, 1000)
      }
    },
    randomOrderID(length) {
      let result = ''
      const characters = 'ABCDEFGHJKLMNPQRSTUVWXYZ23456789'
      const charactersLength = characters.length
      let counter = 0
      while (counter < length) {
        result += characters.charAt(Math.floor(Math.random() * charactersLength))
        counter += 1
      }
      this.orderId = result
    },
    randomNumber(length) {
      let result = ''
      const characters = '0123456789'
      const charactersLength = characters.length
      let counter = 0
      while (counter < length) {
        result += characters.charAt(Math.floor(Math.random() * charactersLength))
        counter += 1
      }
      return result
    },
    backToDelivery() {
      this.$emit('changeState', 'delivery')
      localStorage.clear()
      location.reload()
    }
  }
}
</script>

<style lang="scss">
.canvas {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  div {
    .transfer-sec {
      .timer {
        font-size: 2em;
        text-align: center;
        h1 {
          margin-bottom: 0;
        }
      }
      .recipient {
        // color: #bbb;
        text-align: center;
        font-family: 'InterUIBold';
        font-size: 18px;
      }
      .notes {
        color: #ff8a00;
        text-align: center;
        font-family: 'InterUIMedium';
      }
    }
    .order-sec {
      font-family: 'InterUIRegular';
      .courier {
        color: rgba($color: #000000, $alpha: 0.6);
      }
    }
    .back-btn {
      margin-top: 5vh;
    }
  }
}
</style>
