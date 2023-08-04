<template>
  <div class="back-btn">
    <button class="outlined" @click="backToDelivery()">
      <p class="icon">arrow_back</p>
      Back to delivery
    </button>
  </div>
  <section class="shipment">
    <div class="content-heading">
      <div class="title">
        <h1>Shipment</h1>
        <div class="title-ornamen" />
      </div>
    </div>
    <div class="content-list">
      <div
        class="item"
        :class="{ active: item.picked }"
        v-for="item in shipmentList"
        :key="item.id"
        @click="pickShipment(item.id)"
      >
        <div class="item-content">
          <p class="info-1">{{ item.courier }}</p>
          <p class="info-2">{{ setDelimeter(item.price) }}</p>
        </div>
        <p class="picked-status" v-if="item.picked">check</p>
      </div>
    </div>
  </section>
  <section class="payment">
    <div class="content-heading">
      <div class="title">
        <h1>Payment</h1>
        <div class="title-ornamen" />
      </div>
    </div>
    <div class="content-list">
      <div
        class="item"
        :class="{ active: item.picked }"
        v-for="item in paymentList"
        :key="item.id"
        @click="pickPayment(item.id)"
      >
        <div class="item-content">
          <p class="info-1">{{ item.paymentMethod }}</p>
          <p class="info-2" v-if="item.info">{{ item.info }}</p>
        </div>
        <p class="picked-status" v-if="item.picked">check</p>
      </div>
    </div>
  </section>
</template>

<script>
import numeral from 'numeral'
export default {
  name: 'shipment-payment-component',
  emits: ['changeState', 'setShipment', 'setPayment'],
  data() {
    return {
      shipmentData: {
        courier: '',
        estimation: '',
        price: 0
      },
      paymentMethod: '',
      shipmentList: [
        {
          id: 1,
          courier: 'GO-SEND',
          estimation: 'today',
          price: 15000,
          picked: false
        },
        {
          id: 2,
          courier: 'JNE',
          estimation: '2 days',
          price: 9000,
          picked: false
        },
        {
          id: 3,
          courier: 'Personal Courier',
          estimation: '1 day',
          price: 29000,
          picked: false
        }
      ],
      paymentList: [
        {
          id: 1,
          paymentMethod: 'e-Wallet',
          info: '1,500,000 left',
          picked: false
        },
        {
          id: 2,
          paymentMethod: 'Bank Transfer',
          picked: false
        },
        {
          id: 3,
          paymentMethod: 'Virtual Account',
          picked: false
        }
      ]
    }
  },
  mounted() {
    this.shipmentData.courier = localStorage.getItem('shipmentCourier') ?? ''
    this.shipmentData.estimation = localStorage.getItem('shipmentEstimation') ?? ''
    this.shipmentData.price = localStorage.getItem('shipmentPrice') ?? 0
    this.paymentMethod = localStorage.getItem('paymentMethod') ?? ''
    if (this.shipmentData.courier) {
      this.shipmentList.forEach((element) => {
        if (element.courier == this.shipmentData.courier) {
          element.picked = true
        }
      })
    }
    if (this.paymentMethod) {
      this.paymentList.forEach((element) => {
        if (element.paymentMethod == this.paymentMethod) {
          element.picked = true
        }
      })
    }
  },
  watch: {
    'shipmentData.courier'(val) {
      localStorage.setItem('shipmentCourier', val)
    },
    'shipmentData.estimation'(val) {
      localStorage.setItem('shipmentEstimation', val)
    },
    'shipmentData.price'(val) {
      localStorage.setItem('shipmentPrice', val)
    },
    paymentMethod(val) {
      localStorage.setItem('paymentMethod', val)
    }
  },
  methods: {
    setDelimeter(val) {
      return numeral(val).format('(0,0[.]000)')
    },
    pickShipment(val) {
      this.shipmentList.forEach((element) => {
        if (element.id == val) {
          this.shipmentData.courier = element.courier
          this.shipmentData.estimation = element.estimation
          this.shipmentData.price = element.price
          element.picked = true
        } else {
          element.picked = false
        }
      })
      this.$emit('setShipment', this.shipmentData)
    },
    pickPayment(val) {
      this.paymentList.forEach((element) => {
        if (element.id == val) {
          this.paymentMethod = element.paymentMethod
          element.picked = true
        } else {
          element.picked = false
        }
      })
      this.$emit('setPayment', this.paymentMethod)
    },
    backToDelivery() {
      this.resetData()
      this.$emit('changeState', 'delivery')
    },
    resetData() {
      this.shipmentData.courier = ''
      this.shipmentData.estimation = ''
      this.shipmentData.price = 0
      this.paymentMethod = ''
    }
  }
}
</script>

<style lang="scss">
.shipment {
  .content-heading {
    .title {
      font-size: 1.5em;
      width: fit-content;
      z-index: 1;
      .title-ornamen {
        position: absolute;
        z-index: -1;
        background-color: #eeeeee;
        height: 8px;
        width: 250px;
        margin-top: -40px;
      }
    }
  }
}
.payment {
  .content-heading {
    .title {
      font-size: 1.5em;
      width: fit-content;
      z-index: 1;
      .title-ornamen {
        position: absolute;
        z-index: -1;
        background-color: #eeeeee;
        height: 8px;
        width: 250px;
        margin-top: -40px;
      }
    }
  }
}
</style>
