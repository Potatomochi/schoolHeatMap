<template>
    <div id="wrapper">
        <div class="formContainer">

                <h1 class='formTitle'>Sign In</h1>
                <form class="formBase" @submit.prevent='handleSignin()'>
                    <input type="text" v-model="emailAddress" class="formInput" placeholder="Enter your email address" />
                    <input type="password" v-model="password" class="formInput" placeholder="Enter a password" />
                    <button class="formSubmit" type="submit"> Sign In</button>


                </form>
                <div class="textSmall">  Not a member ?<router-link to="/sign-up"> Sign Up now.</router-link> </div> 
                
        </div>
        <div class="textSmall">Alternatively, you can sign in with google</div>
        <button class="googleSignIn" @click.prevent='googleSignin()'>Sign in with Google</button>
    </div>

</template>

<style>

#wrapper {
    background-color: #000;
}
.formTitle{
  color: #fff;
  font-size: 32px;
  font-weight: bold;
  margin-bottom: 28px;
}
.formContainer {
  display: flex;
  flex-direction: column;
  min-height: 660px;
  background-color: rgba(0, 0, 0, 0.75);
  border-radius: 5px;
  width: 100%;
  margin: auto;
  max-width: 450px;
  padding: 60px 68px 40px;
  margin-bottom: 100px;
}
.formBase {
  display: flex;
  flex-direction: column;
  max-width: 450px;
  width: 100%;
}
.formInput {
  background: #333;
  border-radius: 4px;
  border: 0;
  color: #fff;
  height: 50px;
  line-height: 50px;
  padding: 5px 20px;
  margin-bottom: 20px;
}
.formInput:last-of-type{
  margin-bottom: 30px;
}
.formSubmit{
  background: #e50914;
  border-radius: 4px;
  font-size: 16px;
  font-weight: bold;
  margin: 24px 0 12px;
  padding: 16px;
  border: 0;
  color: white;
  cursor: pointer;
}
.textSmall {
  margin-top: 10px;
  font-size: 13px;
  line-height: normal;
  color: #8c8c8c;
}
.googleSignIn{
  background: #ffffff;
  border-radius: 4px;
  font-size: 16px;
  font-weight: bold;
  margin: 24px 0 12px;
  padding: 16px;
  border: 0;
  color: black;
  cursor: pointer;
}
</style>

<script>
import { getAuth,signInWithPopup, GoogleAuthProvider } from '@firebase/auth';
import firebase from '../helpers/firebaseconfig.js'


export default {
  name: 'SignIn',
  methods : {
    handleSignin : function ()  {

    //this checks if the form input elements are valid
    var emailAddress = this.emailAddress;
    var password = this.password;

    firebase.auth().signInWithEmailAndPassword(emailAddress,password).then(()=> {
        this.$router.push({name : "Admin"});
    }).catch((e) => {
        emailAddress = ''
        password = ''
        console.log(e)
    })
  
  }, 
  googleSignin : function () {
    const auth = getAuth()
    const provider = new GoogleAuthProvider()
    signInWithPopup(auth, provider)
    .then((result) => {
      // This gives you a Google Access Token. You can use it to access the Google API.
      // const credential = GoogleAuthProvider.credentialFromResult(result);
      // const token = credential.accessToken;
      // The signed-in user info.
      const user = result.user;
      // ...
      console.log(user)
      localStorage.setItem('authUser', JSON.stringify(user));
    }).catch((error) => {
      // Handle Errors here.

      const errorMessage = error.message;
      console.log(errorMessage)

    });
  }}
}
</script>