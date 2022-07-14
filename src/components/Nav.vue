<template>
<nav class="navbar navbar-expand navbar-expand-lg navbar-dark bg-info">
<div class="container">
          <RouterLink class="navbar-brand" to="/">My App <i class="fa fa-github fa-1x"></i></RouterLink>
               <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
               <span class="navbar-toggler-icon"></span>
               </button>
                         <div class="collapse navbar-collapse" id="navbarNav">
                              <ul class="navbar-nav ms-auto">
                              <li class="nav-item"  v-if="token!='' ">
                                   <RouterLink class="nav-link" to="/dashboard">Home </RouterLink>                                 
                              </li>
                              <li class="nav-item" v-if="token!='' ">
                                   <RouterLink class="nav-link" to="/shoutout">Shoutouts </RouterLink>
                              </li>
                              <li class="nav-item" v-if="token!='' ">
                                   <RouterLink class="nav-link" to="/profile">Profile</RouterLink>
                              </li>
                              <li class="nav-item"  v-if="token=='' ">
                                   <RouterLink class="nav-link" to="/register">Register </RouterLink>
                              </li>
                              <li class="nav-item"  v-if="token=='' ">
                                   <RouterLink class="nav-link" to="/login">Login</RouterLink>
                              </li>
                               <li class="nav-item" v-if="token!='' ">
                                   <a class="nav-link" href="#" @click="logout">Logout </a>
                              </li>
                         </ul>
                 </div>
          </div>
</nav>
</template>

<script>
import { useAuthStore } from '../stores/auth'
import { useRouter } from 'vue-router'
import { storeToRefs } from 'pinia'

export default {
     setup() {
          const authStore = useAuthStore()
          const router = useRouter()

          const { token } = storeToRefs(useAuthStore())

          async function logout(){
               await fetch('http://localhost:8000/api/logout', {
                    method:"post",
                    headers:{
                         "Accept":"application/json",
                         "Authorization":"Bearer " + authStore.token

                    }
          }).then(response=>response.json())
          .then(data=>{
               if(data.status=="success"){
                    authStore.destroy()
                    router.push('/login')
               }
           })
          }
          return {
               logout,
               token
          }
     },
}
</script>
<style>
.navbar {
     background: #000000
 }
 .nav-item::after {
     content: '';
     display: block;
     width: 0px;
     height: 2px;
     background: #C0C0C0;
     transition: 0.3s
 }
 .nav-item:hover::after {
     width: 100%
 }
 .navbar-dark .navbar-nav .active>.nav-link,
 .navbar-dark .navbar-nav .nav-link.active,
 .navbar-dark .navbar-nav .nav-link.show,
 .navbar-dark .navbar-nav .show>.nav-link,
 .navbar-dark .navbar-nav .nav-link:focus,
 .navbar-dark .navbar-nav .nav-link:hover {
     color: #fec400
 }
</style>