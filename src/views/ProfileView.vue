<template>
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.4.0/css/font-awesome.min.css">
 <div class="card-header text-danger bg-success text-light text-center">Users Information
     </div>
          <div class="card-body bg-secondary">
              <div class="card-title">
                   <table class="table bg-light table-striped table-bordered border-dark">
                         <thead>
                                   <th> Last Name </th>
                                   <th> First Name </th>                        
                                   <th> Contact </th>               
                                   <th> Email </th>
                         </thead>
                  <tbody>
                     <tr>
                              <td>{{user.lname}}</td>
                              <td>{{user.fname}}</td>
                              <td>{{user.mobile}}</td>
                              <td>{{user.email}}</td>
                     </tr>
                  </tbody>
              </table>
           </div>
     </div>
</template>
<script>
import { useAuthStore } from '../stores/auth'
import { useRouter } from 'vue-router'
export default {
     data:()=>{
          return {
               user:[]
          }
     },

    async  mounted(){
     await fetch('http://localhost:8000/api/user',{
          method:'get',
          headers:{
               "Accept":"application/json",
               "Authorization":"Bearer " + this.token
          }
        }).then(response=>response.json())
        .then(data=>{
          this.user = data
        })
     },

     setup() {
          
          const { token } =  useAuthStore()    
          const router = useRouter()    

          if(token==""){
               router.replace('/login')
          }
          
               return{
                    token
         }
     },
}
</script>