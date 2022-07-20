<template>
  <div class="card">
    <div class="card-header pb-0">
      <h6>Kategori table</h6>
    </div>
    <div class="card-body px-0 pt-0 pb-2">
      <div class="table-responsive p-0">
        <table class="table align-items-center mb-0">
          <thead>
            <tr>
              <th class="text-uppercase text-secondary text-xxs font-weight-bolder opacity-7">Kategori</th>
              <th
                class="text-uppercase text-secondary text-xxs font-weight-bolder opacity-7 ps-2"
              >Kode</th>
              
              <th
                class="text-center text-uppercase text-secondary text-xxs font-weight-bolder opacity-7"
              >Created At</th>
              <th class="text-secondary opacity-7"></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(kategori, index) in kategoris" :key="index">
              <td>
                <div class="d-flex px-2 py-1">
                  <div class="d-flex flex-column justify-content-center">
                    <h6 class="mb-0 text-sm">{{ kategori.nama_kategori }}</h6>
                  </div>
                </div>
                
              </td>
              <td>
                <span class="text-secondary text-xs font-weight-bold">{{ kategori.kode_kategori }}</span>
              </td>
              <td class="align-middle text-center text-sm">
                <span class="text-secondary text-xs font-weight-bold">{{ kategori.created_at }}</span>
              </td>
              <td class="align-middle">
                <a
                  href="javascript:;"
                  class="text-secondary font-weight-bold text-xs"
                  data-toggle="tooltip"
                  data-original-title="Edit user"
                >Edit</a>
              </td>
                
                <!-- <td class="text-center">
                    <router-link :to="{name: 'EditKategori', params:{id: kategori.id }}" class="btn btn-sm btn-primary mr-1">Edit</router-link>
                    <button @click.prevent="kategoriDelete(kategori.id)" class="btn btn-sm btn-danger ml-1">Delete</button>
                </td> -->
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import { onMounted, ref } from 'vue'
export default {

  setup() {

        //reactive state
        let kategoris = ref([])

        //mounted
        onMounted(() => {

            //get API from Laravel Backend
            axios.get('http://localhost:8000/api/kategori')
            .then(response => {
              
              //assign state posts with response data
              kategoris.value = response.data.data

            }).catch(error => {
                console.log(error.response.data)
            })

        })

        //method delete
        function kategoriDelete(id) {
                    
        //delete data kategori by ID
        axios.delete(`http://localhost:8000/api/kategori/${id}`)
        .then(() => {
                    
            //splice posts 
            kategoris.value.splice(kategoris.value.indexOf(id), 1);

            }).catch(error => {
                console.log(error.response.data)
            })

        }

        //return
        return {
            kategoris,
            kategoriDelete
        }

    }

  // name: "kategori-table",
};
</script>
