<template>
  <div class="edit-account">
    <h1>Change password</h1>

      <input v-model="npassword" type="password" placeholder="New Password"> <br>
      <input v-model="rnpassword" type="password" placeholder="Repeat New Password"> <br>

      <p v-if="npassword != rnpassword">New Password and Repeat New Password have to match!</p>
      <p>{{ emptyPassword }}</p>
      
      <button v-on:click="editPassword">Save changes</button>

  </div>
</template>



<script>
document.title = "Change password"
import { log } from 'util'
export default {
  props: ["user"],
  data() {
          return {
            npassword: "",
            rnpassword: "",
            emptyPassword: ""
          }
  },
  methods: {
    editPassword(){
      if (this.npassword === this.rnpassword && this.npassword != "") {
        const client = require('../secrets-client')

        const account = {
          id: this.user.id,
          newpassword: this.npassword
        }

        client.updateAccountById(account, (errors) => {
          if (errors.length == 0) {
            console.log("Your account has been updated")
            this.$router.push("/account/" + account.id)
          }
        })
      } else {
        console.log("We could not update your account, please check if all the fields are filled correctly.");
        this.emptyPassword = "The new password can not be empty"
      }
      
    }
  }
}
</script>

<style >
  form p {
    color: red;
  }
</style>