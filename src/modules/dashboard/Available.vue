<template>
  <div class="available-container-item">
    <label class="header"><b>Available Funds</b></label>
    <label class="content" v-if="data !== null">{{auth.displayAmount(data)}}</label>
  </div>
</template>
<style scoped>
.available-container-item{
  width: 100%;
  float: left;
  min-height: 50px;
  overflow-y: hidden;
  border: solid 1px #ddd;
  border-radius: 5px;
  margin-bottom: 10px;
}
.available-container-item label{
  width: 96%;
  float: left;
  height: 40px;
  line-height: 40px;
  margin-left: 2%;
  margin-right: 2%;
  padding: 0px;
  margin-bottom: 0px;
}
</style>
<script>
import ROUTER from '../../router'
import AUTH from '../../services/auth'
import CONFIG from '../../config.js'
export default{
  mounted(){
  },
  data(){
    return {
      user: AUTH.user,
      auth: AUTH
    }
  },
  props: ['data'],
  methods: {
  }

}
</script>
