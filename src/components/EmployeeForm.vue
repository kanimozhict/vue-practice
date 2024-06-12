<template>
    <div id = "form">
      <!---v-on is use to save the text that we are getting from the input form--->
      <!---submit is use to handle the submit event and submit.prevent is use to prevent the browser from getting reloaded--->
      <!--@submit === v-on:submit-->
      <form @submit.prevent = "handleSubmit">
      <label>Employee Name</label>
        <!---v-model will help to store the input data from the form to the particula tage-->
        <!---@Foucus=whenever you are clicking the particular box it will cll the method assigned too-->
        <!-- div is to entire form-->
        <!---:class ois for the particular input--->
        <!---ref is used to add the reference to the app--->
        <input type="text"
               v-model = "inputEmployee.name"
               :class="{'has-error':submitting&&invalidName}"
               @focus="clearStatus"
               @keypress="clearStatus"
               ref="input"

        />
      <label>Employee Age</label>
        <input type="number" v-model = "inputEmployee.Age">
      <label>Employee experience</label>
        <input type="number" v-model="inputEmployee.Yearsofexperience"/>
      <label>Employee DOJ</label>
        <input type="date" v-model="inputEmployee.DOJ"/>
        <p v-if="submitting&&error" class="error-message">Please fill out the required details for submission!</p>
        <p v-if="success" class="success-message">Successfully submitted!</p>
      <button>Add Employee</button>
      </form>
    </div>
</template>
<script>
export default {
  name : 'employee-form',
  data(){
    return{
      submitting: false,
      success: false,
      error: false,
      inputEmployee:{
        name:'',
        Age:'',
        Yearsofexperience:'',
        DOJ:'',
      }
    }
  },
 methods:{
    handleSubmit(){
      this.submitting = true;

      if(this.invalidName){
        this.error = true;
        this.resetForm();
        return;
      }

      this.$emit('add:employee',this.inputEmployee);
      this.clearStatus();
      this.resetForm();
      this.$refs.input.focus();
      this.success = true;
      this.submitting=false;
    },
   clearStatus(){
      this.success = false;
      this.error = false;
   },
   resetForm(){
      this.inputEmployee.name="",
          this.inputEmployee.Age="",
          this.inputEmployee.Yearsofexperience="",
          this.inputEmployee.DOJ=""
   }
 },
  computed:{
    invalidName() {
      return this.inputEmployee.name === "";
    }
  }
}
</script>
<style scoped>

 form{
   margin-bottom: 2rem;
 }
 button{
   padding: 10px 20px;
   border-radius: 8px;
   background-color: #1cb864;
   border-color: #1cb864;
 }
 /*<!----this will denote the pat of the style that starts with the name--->*/
 [class*='-message']{
   font-weight: 500;
 }
 .error-message{
   color : #d33c40;
   font-weight: bold;
 }
 .success-message{
   color: #1cb864;
   font-weight: bolder;
 }

</style>