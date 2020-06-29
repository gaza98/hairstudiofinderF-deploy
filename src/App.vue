<template>
  <div id="app">
    <!-- <Navbar/> -->
    <router-view/>
    <Footer/>
  </div>
</template>

<script>


import Footer from '@/components/layout/Footer'
// import Navbar from '@/components/layout/Navbar'
import {Auth} from '@/services/index'
import store from "@/store";
import store1 from "@/store1";
export default {
 name: 'App',
 components: {
  // Navbar,
  Footer
 },
 data(){
        return {
          store,
          store1
        }
 },
 async mounted(){
   try {
    let res = await Auth.kojiUser()
    console.log(res);
    if(res.data.korisnik){
      this.store.korisnik = res.data.korisnik
      this.store.token = res.data.token
      await localStorage.setItem('token', res.data.token)
      console.log(this.store);
      return
    }
    if(res.data.salon){
      this.store1.salon = res.data.salon
      this.store1.token = res.data.token
      console.log(this.store1);
      await localStorage.setItem('token', res.data.token)
      return
    }
  } catch (error) {
    console.log(error);
  }
 }
}
</script>



<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

// #nav {
//   padding: 30px;

//   a {
//     font-weight: bold;
//     color: #2c3e50;

//     &.router-link-exact-active {
//       color: #42b983;
//     }
//   }
// }
</style>
