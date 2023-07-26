<script setup>
import {ref,computed} from 'vue'
import CardAlbum from './components/CardAlbum.vue';

// ici je charge le tableau records 
import {records} from "./assets/js/allRecords"

console.log(records)

// ici j'indique à vue que je vais me servir de ce tableau en tant que data réactive
const allRecords = ref(records)

// je met de côté ma liste brute

const stockOnly  = ref(false)
const sortBy     = ref("Year")

const filteredRecords = computed(()=>{
  if(stockOnly.value){
     return allRecords.value.filter((album)=>album.stock>0)
  }else{
     return allRecords.value
  }
})

const sortedRecords = computed(()=>{

  const copyRecords = [...filteredRecords.value]

  if(sortBy.value==""){
    return copyRecords
  }

  return copyRecords.sort((albumA,albumB)=>{
    if(sortBy.value=="Year"){
      return albumA.year - albumB.year
    }
    if(sortBy.value=="Pitchfork"){
      return albumA.pitchforkPos - albumB.pitchforkPos
    }
  }) 

})



</script>

<template>
   <div class="min-h-screen flex flex-col">
      <header class="h-32 text-2xl w-full flex-none -ml-full shadow-lg bg-gradient-to-br from-teal-600 to-cyan-400">
        <div class="p-4 h-32 text-2xl w-full flex-none -ml-full rounded-2xl transform shadow-lg bg-gradient-to-br from-cyan-400 to-teal-600 -rotate-1 sm:-rotate-1"> Disco Tailwind 💽 (v3)</div>
      </header>
      
    
      <div class="flex flex-row">
         <div class=" px-8 bg-cyan-100 "> <!-- left filter panel -->
          <div class="mt-2 basis-1/4">

              <div class="overflow-hidden shadow sm:rounded-md">
                  <div class="bg-white p-6 w-64 h-96">
                    
                    <fieldset>
                      <legend class="sr-only">Filtres </legend>
                      <div class="text-base font-medium text-gray-900" aria-hidden="true">Filtres ({{sortedRecords.length}})</div>
                        <div class="mt-4 space-y-4">

                          <div class="flex items-start">
                            <div class="flex h-5 items-center">
                              <input 
                                id="comments" 
                                name="comments" 
                                type="checkbox" 
                                class="h-4 w-4 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500"
                                v-model="stockOnly"
                              >
                              
                            </div>
                            <div class="ml-3 text-sm">
                              <label for="comments" class="font-medium text-cyan-700">In stock only</label>
                            </div>
                            {{ stockOnly }}
                          </div>
     
                        </div>
                        <label for="sortBy" class="block text-sm mt-2 font-medium text-cyan-700">Sort by</label>
                        <select 
                          v-model="sortBy" 
                          id="sortBy" 
                          name="sortBy" 
                          class="mt-1 block w-full rounded-md border border-gray-300 bg-white py-2 px-3 shadow-sm focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm">
                            <option value="" >-- Trie --     </option>
                            <option value="Year" >Par année    </option>
                            <option value="Pitchfork">Position</option>
                        </select>
                        
                    </fieldset>
                    {{ sortBy }}
                  </div>
               
              </div>
          </div>
        </div>

        <main class="bg-white py-5 ml-6 basis-auto">
            <!-- component -->
            <section class="text-gray-600 body-font">
                <!-- one records -->
                <CardAlbum 
                  v-for="album in sortedRecords"
                  :key="album.id"
                  :album="album"
                />
               
            </section><!-- end one records -->
        </main>    
      </div>

    </div>
</template>

<style scoped>
</style>
