<script setup>
import {ref,computed} from 'vue'

const props = defineProps({
    album:{
        type:Object,
        required:true
    }
})

const isFav = ref(false)

const incStock = ()=>{
//   console.log(`%c ${album.title}`, "border-radius:2px;background-color:lime;display:block;border:2px solid cyan;color:crimson")
  props.album.stock++
  
}

const decStock = ()=>{
  if(props.album.stock>0) props.album.stock--

}


const coverUrl = computed(()=>{
    return  props.album.coverUrl ?? 'src/assets/img/default.jpg'
})

const favHandler = ()=> isFav.value = !isFav.value 


</script>

<template>
    <div class="container px-5 mx-auto "  id="card-album" @click="favHandler">
        <div class="p-5 bg-white flex items-center mx-auto border-b shadow-md mb-10  border-gray-400 rounded-lg sm:flex-row flex-col">
            <div class="sm:w-44 sm:h-44 lg:w-40 lg:h-40 sm:mr-10 inline-flex items-center justify-center flex-shrink-0">
                <img :src="coverUrl">
            </div>
            <div class="flex-grow sm:text-left text-center mt-6 sm:mt-0">
            <h1 class="text-black text-2xl title-font font-bold mb-2">{{album.title}} {{ isFav?'❣':'' }} </h1>
            <h3 class="text-black text-xl title-font mb-2">{{album.artist}} <span class="font-light"> {{album.year}}</span></h3>
            <p class="leading-relaxed text-base">{{album.comment}}</p>
            <div class="py-4">
                <div class="inline-block mr-2" v-if="album.stock>0" > <!-- quand le stock est ok  -->
                    <div class="flex  pr-2 h-full items-center border-l-pink-500">
                        <svg class="text-green-500 w-6 h-6 mr-1"  width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">  
                            <path stroke="none" d="M0 0h24v24H0z"/>  
                            <circle cx="12" cy="12" r="9" />  
                            <path d="M9 12l2 2l4 -4" />
                        </svg>
                        <p class="title-font font-medium">{{ album.stock }} stock</p>
                    </div>
                </div>  
                <div class="inline-block mr-2" v-else><!-- quand le stock est à zéro  -->
                    <div class="flex pr-2 h-full items-center">
                        <svg class="text-gray-500 w-6 h-6 mr-1" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                        <circle cx="12" cy="12" r="10"></circle>
                        <line x1="15" y1="9" x2="9" y2="15"></line>
                        <line x1="9" y1="9" x2="15" y2="15"></line>
                        </svg>
                        <p class="title-font font-medium">out of stock</p>
                    </div>
                </div>


            </div>
            <div class="md:flex font-bold text-gray-800">
                <div class="w-full md:w-1/2 flex space-x-3">
                    <div class="w-1/2">
                    <p>pitchfork pos : #{{ album.pitchforkPos }}</p><!-- pitchfork pos  -->
                    </div>
                </div>
                <div class="w-full">
                    <div class="float-right">
                        <button  @click.stop="incStock()"  type="button" class="border border-teal-500 bg-teal-500 text-white px-4 py-2 m-2 ease select-none hover:bg-teal-400 hover:border-gray-900 hover:rounded-md"> + </button>
                        <button  @click.stop="decStock()" :disabled="!album.stock>0" type="button" class="border border-teal-500 bg-teal-500 text-white rounded-md px-4 py-2 m-2 ease select-none hover:bg-purple-800"> - </button>
                    </div>
                </div>
                </div>
            </div>
        </div>
    </div>

</template>