<template>
<div class="nav1" id="nav" >
  
 <b-navbar toggleable="lg" class="navbar-color">
       <b-container>
    <b-navbar-brand ><router-link :to="{ name: 'Home' }" left v-if="!this.store1.token" >HairstudioFinder</router-link></b-navbar-brand>
    <b-navbar-brand left v-if="this.store1.token" class="naslov2"> HairstudioFinder </b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
     
      <!-- Right aligned nav items -->
     
      <b-navbar-nav class="ml-auto">

        <b-nav-item-dropdown class="dropdown" text="Registracija" right v-if="!this.store.token && !this.store1.token">
          <b-dropdown-item><router-link :to="{ name: 'SignUpKorisnik' }">KORISNIK</router-link></b-dropdown-item>
          <b-dropdown-item><router-link :to="{ name: 'SignUpSalon' }">SALON</router-link></b-dropdown-item>
        </b-nav-item-dropdown>

        <b-nav-item-dropdown class="dropdown" text="Login" right v-if="!this.store.token && !this.store1.token">
          <b-dropdown-item><router-link :to="{ name: 'LoginKorisnik' }">KORISNIK</router-link></b-dropdown-item>
          <b-dropdown-item><router-link :to="{ name: 'LoginSalon' }">SALON</router-link></b-dropdown-item>
        </b-nav-item-dropdown>
        <b-nav-item-dropdown class="dropdown" text="Salon" right v-if="this.store1.token">
          <b-dropdown-item><router-link class="profil" :to="{ name: 'PocetnaSalon', params:{id: store1.salon._id}}">Profil</router-link></b-dropdown-item>
          <b-dropdown-item @click.prevent="obrisiSalon()">Obrisi profil</b-dropdown-item>
        </b-nav-item-dropdown>

        <b-nav-item-dropdown class="dropdown" text="Korisnik" right v-if="this.store.token">
          <b-dropdown-item><router-link class="profil" :to="{ name: 'ProfilKorisnik'}">Profil</router-link></b-dropdown-item>
          <b-dropdown-item @click.prevent="obrisiKorisnika()">Obrisi profil</b-dropdown-item>
        </b-nav-item-dropdown>

        

        <b-nav-item nav-link right @click="logout" v-if="this.store.token || this.store1.token">Odjava</b-nav-item>
      
      </b-navbar-nav>
      
    </b-collapse>
    
  </b-container>
  
  </b-navbar>
  
   </div>




</template>

<script>


import store from '@/store'
import store1 from '@/store1';
import {Auth} from '@/services/index'
import Swal from 'sweetalert2'
export default {
    name: 'Navbar',
    data(){
        return{
         store,
         store1
        }
  
      },
      methods:{
        async logout(){
          if(this.store.token){
            try {
              this.store.token = false
              this.store.korisnik = false
              this.store1.token = false;
              this.store1.salon = false;
              let res = await Auth.logoutKorisnik()
              localStorage.clear()
              console.log(res);
              
              
              
             this.$router.push({ name: 'Home' })
             return
            } catch (error) {
              console.log(error);
            }
            
          }
          if(this.store1.token){
            try {
              this.store.token = false
              this.store.korisnik = false
              this.store1.token = false;
              this.store1.salon = false;
              let res = await Auth.logoutSalon()
              localStorage.clear();
              console.log(res);
              
              this.$router.push({ name: 'Home' })
              return
            } catch (error) {
              console.log(error);
            }
            
          }
        },
         obrisiSalon(){
          Swal.fire({
             title: 'Jeste li sigurni?',
            text: 'Nećeš moći vratiti svoj račun nazad!',
            showCancelButton: true,
            confirmButtonText: 'Da, obriši!',
            cancelButtonText: 'Ipak ne, zadržavam račun'
          }).then((result)=>{
              if(result.value){
               this.$router.push({ name: 'Home'})
              console.log("salon brisi");
              let res =  Auth.ObrisiSalon() 
              console.log(res);
              this.store1.token = false;
              this.store1.salon = false;
              Swal.fire(
                'Obrisano!',
                'Tvoj račun je izbrisan',
                'success'
              )
            } else if (result.dismiss === Swal.DismissReason.cancel){
              Swal.fire(
                'Otkazano!',
                'Tvoj račun je siguran :)',
                'error'
              )
            }
          })
        },
         obrisiKorisnika(){
          Swal.fire({
            title: 'Jeste li sigurni?',
            text: 'Nećeš moći vratiti svoj račun nazad!',
            type: 'warning',
            showCancelButton: true,
            confirmButtonText: 'Da, obriši!',
            cancelButtonText: 'Ipak ne, zadržavam račun'
          }).then((result) => {
              if(result.value){
                console.log("korisnik brisi");
              let res =  Auth.ObrisiKorisnika()
              console.log(res);
              this.store.token = false
              this.store.korisnik = false
              this.$router.push({ name: 'Home'})
              Swal.fire(
                'Obrisano!',
                'Tvoj račun je izbrisan',
                'success'
              )
            } else if (result.dismiss === Swal.DismissReason.cancel){
              Swal.fire(
                'Otkazano!',
                'Tvoj račun je siguran :)',
                'error'
              )
            }
          })
        },
      },
      mounted() {
        console.log(this.store1);
        var nav = document.getElementById('nav');
window.onscroll = function(){
  if(window.pageYOffset > 100){
    nav.style.background = "#8b6d53"
  }
  else{
    nav.style.background = "transparent"
  }
}

	

      }
}

</script>

<style>

.naslov2{
  left: 20px;
}

.nav1{
  z-index: 10000;
  top:0px;
  width:100%;
  position: fixed;
}


navbar-color{
  opacity: 100%;
}

.navbar-light .navbar-brand {
    color: white;
}
.navbar-light .navbar-nav .nav-link {
    color: white;
}
.router-link-active{
    color: white;
    border: 2px solid white;
  padding: 10px;
  border-radius: 50px 20px;
}
.router-link-active:hover{
    text-decoration: none;
    color: black;
}
a{
    color: black;
    
    
}
a:hover{
    text-decoration: none;
    color: #212529;
}

.prijava a{
  color: white;
}

.prijava a:hover{
  color: #212529;
}

.profil{
  color: black;
}
</style>