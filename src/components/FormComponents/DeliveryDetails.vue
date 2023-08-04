<template>
  <div class="back-btn">
    <button class="outlined">
      <p class="icon">arrow_back</p>
      Back to cart
    </button>
  </div>
  <div class="content-heading">
    <div class="title">
      <h1>Delivery details</h1>
      <div class="title-ornamen" />
    </div>
    <div class="dropshipper">
      <label class="container" for="checkbox-dropship">
        Send as dropshipper
        <input type="checkbox" checked="checked" id="checkbox-dropship" v-model="dropshipCheck" />
        <span class="checkmark"></span>
      </label>
    </div>
  </div>
  <div class="delivery-form">
    <form>
      <!-- Email -->
      <div class="field-space email">
        <div class="input-wrap">
          <input
            type="text"
            :class="{
              valid: deliveryData.email && v$.deliveryData.email.email.$response,
              invalid: deliveryData.email && !v$.deliveryData.email.email.$response
            }"
            v-model="deliveryData.email"
            @input="v$.deliveryData.email.$touch"
            placeholder="Email"
          />
          <p
            class="input-status"
            :class="{
              valid: deliveryData.email && v$.deliveryData.email.email.$response,
              invalid: deliveryData.email && !v$.deliveryData.email.email.$response
            }"
          >
            {{
              deliveryData.email && v$.deliveryData.email.email.$response
                ? 'check'
                : deliveryData.email && !v$.deliveryData.email.email.$response
                ? 'clear'
                : ''
            }}
          </p>
        </div>
        <span class="error-message" v-if="!v$.deliveryData.email.email.$response">
          Value is not a valid email address
        </span>
      </div>
      <!-- Dropshipper Name -->
      <div class="field-space dropshipper-name" v-if="dropshipCheck">
        <div class="input-wrap">
          <input
            type="text"
            :class="{
              valid: deliveryData.dropshipperName
            }"
            v-model="deliveryData.dropshipperName"
            placeholder="Dropshipper Name"
          />
          <p
            class="input-status"
            :class="{
              valid: deliveryData.dropshipperName
            }"
          >
            {{ deliveryData.dropshipperName ? 'check' : '' }}
          </p>
        </div>
      </div>
      <!-- Phone Number -->
      <div class="field-space phone-number">
        <div class="input-wrap">
          <input
            type="text"
            :class="{
              valid: deliveryData.phoneNumber && !v$.deliveryData.phoneNumber.$error,
              invalid: deliveryData.phoneNumber && v$.deliveryData.phoneNumber.$error
            }"
            @input="v$.deliveryData.phoneNumber.$touch"
            @keypress="isPhoneNumber($event)"
            v-model="deliveryData.phoneNumber"
            placeholder="Phone Number"
          />
          <p
            class="input-status"
            :class="{
              valid: deliveryData.phoneNumber && !v$.deliveryData.phoneNumber.$error,
              invalid: deliveryData.phoneNumber && v$.deliveryData.phoneNumber.$error
            }"
          >
            {{
              deliveryData.phoneNumber && !v$.deliveryData.phoneNumber.$error
                ? 'check'
                : deliveryData.phoneNumber && v$.deliveryData.phoneNumber.$error
                ? 'clear'
                : ''
            }}
          </p>
        </div>
        <span class="error-message" v-if="!v$.deliveryData.phoneNumber.minLength.$response">
          Phone Number should be at least 6 characters long
        </span>
        <span class="error-message" v-if="!v$.deliveryData.phoneNumber.maxLength.$response">
          Maximum Phone Number length allowed is 20
        </span>
      </div>
      <!-- Dropshipper Phone Number -->
      <div class="field-space dropshipper-phone-number" v-if="dropshipCheck">
        <div class="input-wrap">
          <input
            type="text"
            :class="{
              valid:
                deliveryData.dropshipperPhoneNumber &&
                !v$.deliveryData.dropshipperPhoneNumber.$error,
              invalid:
                deliveryData.dropshipperPhoneNumber && v$.deliveryData.dropshipperPhoneNumber.$error
            }"
            @input="v$.deliveryData.dropshipperPhoneNumber.$touch"
            @keypress="isPhoneNumber($event)"
            v-model="deliveryData.dropshipperPhoneNumber"
            placeholder="Dropshipper Phone Number"
          />
          <p
            class="input-status"
            :class="{
              valid:
                deliveryData.dropshipperPhoneNumber &&
                !v$.deliveryData.dropshipperPhoneNumber.$error,
              invalid:
                deliveryData.dropshipperPhoneNumber && v$.deliveryData.dropshipperPhoneNumber.$error
            }"
          >
            {{
              deliveryData.dropshipperPhoneNumber && !v$.deliveryData.dropshipperPhoneNumber.$error
                ? 'check'
                : deliveryData.dropshipperPhoneNumber &&
                  v$.deliveryData.dropshipperPhoneNumber.$error
                ? 'clear'
                : ''
            }}
          </p>
        </div>
        <span
          class="error-message"
          v-if="!v$.deliveryData.dropshipperPhoneNumber.minLength.$response"
        >
          Dropshipper Phone Number should be at least 6 characters long
        </span>
        <span
          class="error-message"
          v-if="!v$.deliveryData.dropshipperPhoneNumber.maxLength.$response"
        >
          Maximum Dropshipper Phone Number length allowed is 20
        </span>
      </div>
      <!-- Delivery Address -->
      <div class="field-space delivery-address">
        <div class="input-wrap">
          <textarea
            type="text"
            :class="{
              valid: deliveryData.address && !v$.deliveryData.address.$error,
              invalid: deliveryData.address && v$.deliveryData.address.$error
            }"
            @input="v$.deliveryData.address.$touch"
            v-model="deliveryData.address"
            placeholder="Delivery Address"
          />
          <p
            class="input-status"
            :class="{
              valid: deliveryData.address && !v$.deliveryData.address.$error,
              invalid: deliveryData.address && v$.deliveryData.address.$error
            }"
          >
            {{
              deliveryData.address && !v$.deliveryData.address.$error
                ? 'check'
                : deliveryData.address && v$.deliveryData.address.$error
                ? 'clear'
                : ''
            }}
          </p>
          <p
            class="char-counter"
            :class="{
              valid: deliveryData.address.length > 0 && deliveryData.address.length <= 120,
              invalid: deliveryData.address.length > 120
            }"
          >
            {{ deliveryData.address.length }} / 120
          </p>
        </div>
        <span class="error-message" v-if="!v$.deliveryData.address.required.$response">
          Delivery Address is required
        </span>
        <span class="error-message" v-if="!v$.deliveryData.address.maxLength.$response">
          Delivery Address length allowed is 120 characters
        </span>
      </div>
    </form>
  </div>
</template>

<script>
import useValidate from '@vuelidate/core'
import { required, email, maxLength, minLength } from '@vuelidate/validators'

export default {
  name: 'delivery-details-component',
  emits: ['setDropshipper', 'setFormDeliveryStat'],
  data() {
    return {
      v$: useValidate(),
      deliveryData: {
        email: '',
        phoneNumber: '',
        address: '',
        dropshipperName: '',
        dropshipperPhoneNumber: ''
      },
      dropshipCheck: false
    }
  },
  validations() {
    if (this.dropshipCheck == false) {
      return {
        deliveryData: {
          email: { email },
          phoneNumber: { minLength: minLength(6), maxLength: maxLength(20) },
          address: { required, maxLength: maxLength(120) }
        }
      }
    } else {
      return {
        deliveryData: {
          email: { email },
          phoneNumber: { minLength: minLength(6), maxLength: maxLength(20) },
          address: { required, maxLength: maxLength(120) },
          dropshipperPhoneNumber: { minLength: minLength(6), maxLength: maxLength(20) }
        }
      }
    }
  },
  mounted() {
    this.deliveryData.email = localStorage.getItem('email') ?? ''
    this.deliveryData.phoneNumber = localStorage.getItem('phoneNumber') ?? ''
    this.deliveryData.address = localStorage.getItem('address') ?? ''
    this.deliveryData.dropshipperName = localStorage.getItem('dropshipperName') ?? ''
    this.deliveryData.dropshipperPhoneNumber = localStorage.getItem('dropshipperPhoneNumber') ?? ''
    this.dropshipCheck = localStorage.getItem('dropshipCheck') == 'true' ? true : false
  },
  watch: {
    dropshipCheck(val) {
      if (val == false) {
        this.deliveryData.dropshipperName = ''
        this.deliveryData.dropshipperPhoneNumber = ''
      }
      localStorage.setItem('dropshipCheck', val)
      this.$emit('setDropshipper', val)
    },
    'deliveryData.email'(val) {
      localStorage.setItem('email', val)
    },
    'deliveryData.phoneNumber'(val) {
      localStorage.setItem('phoneNumber', val)
    },
    'deliveryData.address'(val) {
      localStorage.setItem('address', val)
    },
    'deliveryData.dropshipperName'(val) {
      localStorage.setItem('dropshipperName', val)
    },
    'deliveryData.dropshipperPhoneNumber'(val) {
      localStorage.setItem('dropshipperPhoneNumber', val)
    },
    v$() {
      this.v$.$validate()
      if (!this.v$.$error) {
        this.$emit('setFormDeliveryStat', true)
      } else {
        this.$emit('setFormDeliveryStat', false)
      }
    }
  },
  methods: {
    isPhoneNumber(evt) {
      evt = evt ? evt : window.event
      var charCode = evt.which ? evt.which : evt.keyCode
      // charCode == 40 => (
      // charCode == 41 => )
      // charCode == 43 => -
      // charCode == 45 => +
      // charCode == 48 => 1
      // charCode == 57 => 0
      if (
        (charCode > 47 && charCode <= 57) ||
        charCode == 40 ||
        charCode == 41 ||
        charCode == 43 ||
        charCode == 45
      ) {
        return true
      } else {
        evt.preventDefault()
      }
    }
  }
}
</script>

<style lang="scss">
.content-heading {
  display: flex;
  justify-content: space-between;
  .title {
    font-size: 1.2em;
    width: fit-content;
    z-index: 1;
    .title-ornamen {
      position: absolute;
      z-index: -1;
      background-color: #eeeeee;
      height: 8px;
      width: 270px;
      margin-top: -35px;
    }
  }
  .dropshipper {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    padding-right: 2vw;
    /* Customize the label (the container) */
    .container {
      font-family: 'InterUIRegular';
      margin-left: 5px;
      display: block;
      position: relative;
      padding-left: 35px;
      cursor: pointer;
      font-size: 16px;
      -webkit-user-select: none;
      -moz-user-select: none;
      -ms-user-select: none;
      user-select: none;

      /* Hide the browser's default checkbox */
      input {
        position: absolute;
        opacity: 0;
        cursor: pointer;
        height: 0;
        width: 0;
      }

      /* Create a custom checkbox */
      .checkmark {
        position: absolute;
        top: 0;
        left: 0;
        height: 16px;
        width: 16px;
        border: solid #ff8a00;
      }
      /* Create the checkmark/indicator (hidden when not checked) */
      .checkmark:after {
        content: '';
        position: absolute;
        display: none;
      }
      /* Style the checkmark/indicator */
      .checkmark:after {
        left: 5px;
        width: 4px;
        height: 10px;
        border: solid #1bd97b;
        border-width: 0 2px 2px 0;
        -webkit-transform: rotate(45deg);
        -ms-transform: rotate(45deg);
        transform: rotate(45deg);
      }
    }

    /* On mouse-over, add a grey background color */
    .container:hover input ~ .checkmark {
      background-color: #eee;
    }

    /* When the checkbox is checked, add a blue background */
    .container input:checked ~ .checkmark {
      background-color: #fff;
      border: solid #1bd97b;
    }

    /* Show the checkmark when checked */
    .container input:checked ~ .checkmark:after {
      display: block;
    }
  }
}
.delivery-form {
  margin-top: 20px;
  form {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    .field-space {
      width: 48%;
      margin-top: 10px;
      .error-message {
        color: #ff8a00;
        font-family: 'InterUIRegular';
        font-size: 13px;
      }
    }
  }
}

@media (max-width: 975px) {
  .content-heading {
    flex-wrap: wrap;
    .title {
      order: 1;
      font-size: 1.1rem;
    }
    .dropshipper {
      order: 2;
      .container {
        font-size: 1em;
      }
    }
  }
}

@media (max-width: 600px) {
  .content-heading {
    .title {
      width: 100%;
      .title-ornamen {
        width: 50vw;
      }
    }
    .dropshipper {
      justify-content: end;
      width: 100%;
      .container {
        font-size: 1.2em;
      }
    }
  }
  .delivery-form {
    form {
      justify-content: center;
      .field-space {
        width: 100%;
      }
    }
  }
}
</style>
