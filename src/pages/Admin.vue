<template>
    <div> <Navbar/>
      <div class="container py-4 ">
          <div class="row">
            <div class="col-lg-9">
              <table class="table">
                <thead>
                  <tr>
                    <th width="10%" class="text-center">ลำดับ</th>
                    <th width="35%" class="text-center">ชื่อสินค้า</th>
                    <th width="35%" class="text-center">ราคา(บาท)</th>
                    <th width="20%" class="text-center">จัดการ</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(store, index) in stores" :key="index">
                    <td width="10%" class="text-center">{{ index + 1 }}</td>
                    <td width="35%">{{ store.name }}</td>
                    <td width="35%">{{ store.price }}</td>
                    <td width="20%" class="text-center"> 
                      <button type="button" class="btn btn-warning" @click="edit(store.id)">แก้ไข</button>&nbsp; 
                      <button type="button" class="btn btn-danger"  @click="deleted(store.id)">ลบ</button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
      
            <div class="col-lg-3">
              <div class="card">
                <div class="card-body">
                  <form @submit.prevent="createStore">
                    <div class="mb-3">
                      <label for="name" class="form-label">Name</label>
                      <input type="text" class="form-control"   v-model="formData.name"/>
                    </div>
                    <div class="mb-3">
                      <label for="price" class="form-label">Price</label>
                      <input type="number" class="form-control"  v-model="formData.price" />
                    </div>
                    <button type="submit" class="btn btn-primary" v-if="!editMode">บันทึก</button>
                    <button type="button" class="btn btn-primary" v-else @click="update">อัพเดต</button>
                    &nbsp;
                    <button type="reset" class="btn btn-secondary"  @click="editMode = false">ยกเลิก</button>
                   
                  </form>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
  </template>
  
  <script setup>
  
  import {onMounted, ref} from "vue"
  import axios from "axios"
  import Navbar from "../components/Navbar.vue"
  
  const formData = ref({
    name: '',
    price: ''
  });
  
  const stores = ref({})
  const editMode = ref(false)
  const createStore = async() =>{
    try{
      const res = await axios.post("http://127.0.0.1:8000/api/store", formData.value)
      console.log(res.data.message);
      await getStore()
    }catch(e){
      console.log(e)
    }finally{
  
    }
  }
  
  
  const getStore = async() =>{
    try {
      const res = await axios.get("http://127.0.0.1:8000/api/store")
       stores.value = res.data.data
       console.log(res.data);
    } catch (e) {
      console.log(e);
    }
  }
  
  const edit = async(id) =>{
    try {
       editMode.value = true;
       const res = await axios.get("http://127.0.0.1:8000/api/store/" + id)
       formData.value = res.data.data
    } catch (e) {
      console.log(e);
    }
  }
  const update = async(id) =>{
    try {
       const res = await axios.post("http://127.0.0.1:8000/api/store/" + formData.value.id + "/update", formData.value)
       await getStore()
    } catch (e) {
      console.log(e);
    }
  }
  
  const deleted = async(id) =>{
    try {
       const res = await axios.delete("http://127.0.0.1:8000/api/store/" + id + "/delete", formData.value)
       await getStore()
    } catch (e) {
      console.log(e);
    }
  }
  
  onMounted(()=>{
    getStore()
  })
  </script>
  
  <style scoped></style>
  