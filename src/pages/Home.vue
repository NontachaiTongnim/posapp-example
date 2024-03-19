<template>
  <div> <Navbar/>
    <div class="container py-4 ">
        <div class="row">
          <div class="col-lg-12">
            <table class="table">
              <thead>
                <tr>
                  <th width="10%" class="text-center">ลำดับ</th>
                  <th width="35%" class="text-center">ชื่อสินค้า</th>
                  <th width="35%" class="text-center">ราคา(บาท)</th>
                
                </tr>
              </thead>
              <tbody>
                <tr v-for="(store, index) in stores" :key="index">
                  <td width="10%" class="text-center">{{ index + 1 }}</td>
                  <td width="35%">{{ store.name }}</td>
                  <td width="35%">{{ store.price }}</td>
                </tr>
              </tbody>
            </table>
          </div>
    
    
        </div>
      </div>
    </div>
</template>

<script setup>
  
  import {onMounted, ref} from "vue"
  import axios from "axios"
  import Navbar from "../components/Navbar.vue"

  
  const stores = ref({})

  
  const getStore = async() =>{
    try {
      const res = await axios.get("http://127.0.0.1:8000/api/store")
       stores.value = res.data.data
       console.log(res.data);
    } catch (e) {
      console.log(e);
    }
  }
  

  
  onMounted(()=>{
    getStore()
  })
  </script>

<style scoped></style>
