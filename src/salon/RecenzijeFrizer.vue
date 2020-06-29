<template>
    <div>
      <Navbar/>

        <div class="container">
        
        <div class="card-deck justify-content-center group">
                <div  class="card mb-sm-4" style="width: 18rem;">
                      <div class="card-body">
                          <h5 class="card-title text-center"><b>Zaposlenik</b><hr>{{ime}}</h5>
                      </div>
                </div>           
        </div>
        <div>
          <div class="rating">
                          <star-rating :read-only="true" :increment="0.01" v-model="prosjekOcjena"></star-rating>
                      </div>
        </div>
        <br>
        </div>

        <div class="container">
        
          <div v-for="komentar in komentari" :key="komentar._id" class="alert alert-secondary" role="alert">

          <b>Komentira:</b> {{komentar.korisnik}} <br> {{komentar.komentar}} <br>
           Ocjena: <b>{{komentar.ocjena}}</b> 
          </div>
        </div>
    </div>
</template>


<script>
import {StarRating} from 'vue-rate-it';
import Navbar from '@/components/layout/Navbar'
import Komentar from '@/services/komentar'
export default {
  components:{
    Navbar,
    StarRating
  },
    data(){
        return{
            ime: '',
            username: '',
            body: {
              komentar: '',
              ocjena: '',
              salon: this.$route.params.id_salona,
              frizer: this.$route.params.id
            },
            komentari: [],
            prosjekOcjena: [],
            feedback: '',
            
        }
    },
    
    methods:{
      async Komentari(){
        try {
          let res = await Komentar.DohvatiSve(this.$route.params.id)
          console.log(res);
          this.komentari = res.data
          const ocjena_red = res.data.reduce((acc,curr) =>{
            return acc + curr.ocjena
            
            }, 0);
          
         this.prosjekOcjena = ocjena_red / res.data.length
          console.log(this.prosjekOcjena)
        } catch (error) {
          console.log(error);
        }
      }
    },
    async created(){
      this.ime = this.$route.params.ime
      await this.Komentari()
    },
    
}
</script>

<style>

.rating{
  display:flex;
  width: 100%;
  align-items: center;
  flex-direction: column; 
  justify-content: center;
}

.kom{
  width: 80%;
}
.ocjena{
  width: 20%;
}
.card{
  background-color: rgb(209, 205, 201);
}

body{
    background-color: #a69180;
}

form{
    margin: 30px 30px;
}
form h3{
    text-align: center;
}
.btn-primary{
    background-color: #463D36;
    border-color: #463D36;
}
.btn-primary:hover{
    background-color: rgb(115, 190, 100);
    border-color: rgb(115, 190, 100);
}
.wrapper {
  display: flex;
  align-items: center;
  flex-direction: column; 
  justify-content: center;
  width: 100%;
  min-height: 100%;
  padding: 20px;
}

#formContent {
  -webkit-border-radius: 10px 10px 10px 10px;
  border-radius: 10px 10px 10px 10px;
  background: rgb(209, 205, 201);
  padding: 30px;
  width: 90%;
  max-width: 450px;
  position: relative;
  padding: 0px;
  -webkit-box-shadow: 0 30px 60px 0 rgba(0,0,0,0.3);
  box-shadow: 0 30px 60px 0 rgba(0,0,0,0.3);
  text-align: center;
}

/* Simple CSS3 Fade-in-down Animation */
.fadeInDown {
  -webkit-animation-name: fadeInDown;
  animation-name: fadeInDown;
  -webkit-animation-duration: 1s;
  animation-duration: 1s;
  -webkit-animation-fill-mode: both;
  animation-fill-mode: both;
}

@-webkit-keyframes fadeInDown {
  0% {
    opacity: 0;
    -webkit-transform: translate3d(0, -100%, 0);
    transform: translate3d(0, -100%, 0);
  }
  100% {
    opacity: 1;
    -webkit-transform: none;
    transform: none;
  }
}

@keyframes fadeInDown {
  0% {
    opacity: 0;
    -webkit-transform: translate3d(0, -100%, 0);
    transform: translate3d(0, -100%, 0);
  }
  100% {
    opacity: 1;
    -webkit-transform: none;
    transform: none;
  }
}
</style>