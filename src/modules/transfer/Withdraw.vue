<template>
  <div class="modal fade" id="createWithdrawModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog" role="document">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel">Withdrawal</h5>
          <button type="button" class="close" @click="hideModal()" aria-label="Close">
            <span aria-hidden="true" class="text-primary">&times;</span>
          </button>
        </div>
        <div class="modal-body">
          <!-- Step 1 inputs -->
          <span v-if="errorMessage !== null" class="text-danger text-center">
              <label><b>Opps! </b>{{errorMessage}}</label>
          </span>

          <div v-if="step === 1">
            <br v-if="errorMessage !== null">
            <div class="form-group">
              <label for="exampleInputEmail1"><b>CHECKOUT OPTIONS</b></label>
              <br><br>
                    &nbsp&nbsp   <input type="radio" id="one" value="Banco De Oro" v-model="picked">
                          <label for="one" style="padding-right: 80px"><img width="150px" src="../../assets/img/bdo.png"/></label>
                          <input type="radio" id="two" value="Cebuana Lhuillier" v-model="picked">
                          <label for="two"><img width="150px" src="../../assets/img/cebuanalhuillier.png"/></label>
                          <br>
                    &nbsp&nbsp    <input type="radio" id="three" value="Palawan Pawnshop" v-model="picked">
                          <label for="three" style="padding-right: 80px"><img width="150px" src="../../assets/img/palawanpawnshop.png"/></label>
                          <input type="radio" id="four" value="GCASH" v-model="picked">
                          <label for="four"><img width="150px" src="../../assets/img/gcash.png"/></label>
                          <br><br>
                    &nbsp&nbsp      <input type="radio" id="five" value="Union Bank" v-model="picked">
                          <label for="five" style="padding-right: 80px"><img width="150px" src="../../assets/img/unionbank.png"/></label>
                          <input type="radio" id="six" value="M Lhuillier" v-model="picked">
                          <label for="six"><img width="150px" src="../../assets/img/mlhuillier.png"/></label>
                          <br><br><br>                                 
            </div>
          </div>
            <div v-if="step === 1 && picked !== null">
            <br v-if="errorMessage !== null">
            <div class="form-group">
              <label for="exampleInputEmail1"><b>PAYMENT DETAILS</b></label>
              <textarea class="form-control" style="height:200px" v-model="checkoutDetails.details"></textarea>                           
            </div>
          </div>

           


          <div v-if="step === 2">
            <br v-if="errorMessage !== null">
            <div class="form-group">
              <label for="exampleInputEmail1">Amount to withdraw</label>
              <input type="number" class="form-control" v-model="newInput.amount">
            </div>
          </div>

          <!-- Step 3 OTP -->

          <div v-if="step === 3">
            <div class="form-group text-center">
              <label for="exampleInputEmail1 text-gray">
                Please enter the OTP Code sent to your email address.
              </label>
              <input type="text" class="form-control" v-model="otp" placeholder="123456">
            </div>
            <div class="form-group text-center text-gray">
              <label>Didn't received an email? Click the link below.</label>
              <label class="text-center action-link text-primary">Resend</label>
            </div>
          </div>

          <!-- Step 3 Deduct -->
          <div v-if="step === 4" class="text-center">
            <i class="fa fa-check text-primary" style="font-size: 100px"></i>
            <label class="text-primary">Your transaction was succcessfully sent!</label>
          </div>

        </div>
        <div class="modal-footer">
            <button type="button" class="btn btn-danger" @click="hideModal()" v-if="loading === false">Cancel</button>
            <button type="button" class="btn btn-primary" @click="next()" v-if="step === 1 && loading === false">Next
            </button>
            <button type="button" class="btn btn-primary" @click="next()" v-if="step === 2 && loading === false">Withdraw
            </button>
            <button type="button" class="btn btn-primary" @click="next()" v-if="step === 3 && loading === false">
              Verify
            </button>
            <label v-if="loading === true" class="text-primary">
              Checking <i class="fas fa-circle-o-notch fa-spin"></i>
            </label>
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
.container {
  display: block;
  position: relative;
  padding-left: 35px;
  margin-bottom: 12px;
  cursor: pointer;
  font-size: 12px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

/* Hide the browser's default radio button */
.container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
}

/* Create a custom radio button */
.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  background-color: #eee;
  border-radius: 50%;
}

/* On mouse-over, add a grey background color */
.container:hover input ~ .checkmark {
  background-color: #ccc;
}

.form-radio-item {
padding-bottom: 15px;
padding-top: 15px;
}

/* When the radio button is checked, add a blue background */
.container input:checked ~ .checkmark {
  background-color: #2196F3;
}

/* Create the indicator (the dot/circle - hidden when not checked) */
.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

/* Show the indicator (dot/circle) when checked */
.container input:checked ~ .checkmark:after {
  display: block;
}

/* Style the indicator (dot/circle) */
.container .checkmark:after {
  top: 9px;
  left: 9px;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: white;
}
.form-control{
  min-height: 30px !important;
  overflow-y: hidden;
}
.input-group{
  margin-bottom: 15px !important;
}
.input-group-addon{
  width: 100px !important;
  background: #22b173 !important;
  color: #fff !important;
}
.modal{
  color: black !important;
}

.fa-spin{
  animation-duration: 0.50s;
}
</style>
<script>
import ROUTER from '../../router'
import AUTH from '../../services/auth'
import CONFIG from '../../config.js'
export default {
  data(){
    return {
      user: AUTH.user,
      config: CONFIG,
      imgIndex: 0,
      picked: null,
      checkoutDetails: {
        details: null
      },
      errorMessage: null,
      step: 1,
      newInput: {
        amount: 0
      },
      otpUseCounter: 0,
      otp: '',
      payload: null,
      loading: false
    }
  },
  props: ['item'],
  methods: {
    redirect(parameter){
      ROUTER.push(parameter)
    },
    hideModal(){
      $('#createWithdrawModal').modal('hide')
      this.step = 1
      this.checkoutDetails.details = null
      this.picked = null
      this.errorMessage = null
    },
    next(){
      if(this.step === 1){
        if(this.picked == null){
          this.errorMessage = 'Please select a service'
        }else if(this.checkoutDetails.details == null){
          this.errorMessage = null
          this.errorMessage = 'Please input payment details'
        }else{
          this.errorMessage = null
          this.step++
        }
      }else if(this.step === 2){
        this.invest(0)
      }else if(this.step === 3){
        this.verifyOtp()
      }
    },
    invest(otp){
      let amount = parseFloat(this.newInput.amount)
      let remainingAmount = parseFloat(this.item) - amount
      if(remainingAmount < this.config.MINIMUM_WITHDRAWAL && remainingAmount > 0){
        this.errorMessage = 'Remaining amount should not be less than the minimum amount of investment.'
      }else if(amount < this.config.MINIMUM_WITHDRAWAL){
        this.errorMessage = 'The minimum investment is PHP ' + this.config.MINIMUM_WITHDRAWAL + '.'
      }else if(amount <= parseFloat(this.item)){
        this.errorMessage = null
        this.newInput['payload_value'] = this.checkoutDetails.details
        this.newInput['payload'] = this.picked
        this.newInput['account_id'] = this.user.userID
        this.newInput['otp'] = otp
        this.loading = true
        this.APIRequest('withdrawals/create', this.newInput, response => {
          this.loading = false
          if(response.data !== null){
            // this.$parent.retrieve({column: 'created_at', value: 'asc'})
            this.step++
          }else if(response.otp === true){
            this.step++
          }else{
            this.errorMessage = response.error
          }
        }, () => {
          this.loading = false
        })
      }else{
        this.errorMessage = 'Amount must be less than to the borrowed amount.'
      }
    },
    verifyOtp(){
      let parameter = {
        condition: [{
          value: this.user.userID,
          column: 'account_id',
          clause: '='
        }, {
          value: this.otp,
          column: 'code',
          clause: '='
        }]
      }
      let flag = false
      if(isNaN(this.otp) || this.otp.length < 6){
        this.errorMessage = 'Invalid Code. Click resend to get new Code.'
      }
      this.otpUseCounter++
      if(this.otpUseCounter > 1){
        this.errorMessage = 'OTP Code can only be use once.'
      }
      this.loading = true
      this.APIRequest('notification_settings/retrieve', parameter).then(response => {
        if(response.data.length > 0){
          this.invest(1)
          this.step++
          this.hideModal()
        }else{
          this.errorMessage = 'Invalid Code. Click resend to get new code'
        }
      })
    }
  }
}
</script>
