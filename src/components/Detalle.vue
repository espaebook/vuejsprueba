<template>
  <div>

    <div class="container px-8 mx-auto xl:px-5 max-w-screen-lg py-5 lg:py-8 !pt-0">

      <div class="flex items-center space-x-2 text-sm mt-5">
        
        <router-link to="/"><button type="submit" class="w-24 py-2 font-semibold text-white transition-colors bg-gray-900 rounded-md hover:bg-gray-800 focus:outline-none focus:ring-offset-2 focus:ring focus:ring-gray-200 px-6 dark:bg-white dark:text-black ">Inicio</button></router-link>
      </div>
      
      <div class="max-w-screen-md px-5 mx-auto mt-5">
      
        <h1 class="mt-2 mb-3 text-3xl font-semibold tracking-tight lg:leading-tight text-brand-primary lg:text-2xl dark:text-white">{{ datos.title }}</h1>
        <div class="flex mt-8 space-x-3 text-gray-500">

          <div class="flex items-center space-x-2 text-sm">
              <p class="text-gray-800 dark:text-gray-400">Autor: {{ datos.author }}</p>
              <span class="text-xs text-gray-300 dark:text-gray-600">•</span>
              <time class="text-gray-500 dark:text-gray-400">Fecha: {{ dateTime(datos.publishedAt) }}</time>
          </div>

        </div>
      </div>
    </div>
    <div class="relative z-0 max-w-screen-lg mx-auto overflow-hidden lg:rounded-lg aspect-video">
      <a v-if="datos.urlToImage" class="relative block aspect-video">

        <img
        alt="Thumbnail"
        sizes="(max-width: 300px) 30vw, 33vw"                        
        :src="datos.urlToImage"
        @error="setAltImg"
        decoding="async"
        data-nimg="fill"            
        class="transition-all object-cover"
        style="position: absolute; height: 100%; width: 100%; left: 0; top: 0; right: 0; bottom: 0; color: transparent;"                                
        />
      </a>

      <a v-else class="relative block aspect-video">

        <img
        alt="Thumbnail"
        sizes="(max-width: 300px) 30vw, 33vw"                        
        src="https://pbs.twimg.com/media/DjzCtwVX0AAVtpp.jpg"
        decoding="async"
        data-nimg="fill"            
        class="transition-all object-cover"
        style="position: absolute; height: 100%; width: 100%; left: 0; top: 0; right: 0; bottom: 0; color: transparent;"
        />

      </a>
    </div>
    <div class="container px-8 mx-auto xl:px-5 max-w-screen-lg py-5 lg:py-8">
      <article class="max-w-screen-md mx-auto">
        <div class="mx-auto my-3 prose dark:prose-invert prose-a:text-blue-600">            
          <p>{{ datos.description }}</p>
        </div>
      </article>

      <div class="flex mt-8 space-x-3 text-gray-500">

        <div class="flex items-center space-x-2 text-sm">
            <p class="text-gray-800 dark:text-gray-400">Fuente: {{ fuente }}</p>
    
        </div>

      </div>

    </div>
</div>


</template>

<script>
import axios from 'axios'
import moment from 'moment'
import {useRoute} from "vue-router";

export default {
  name: 'Detalle-noticia',

  data() {
    return {
        datos: [],
        fuente: [],
    }
  },
  mounted() {

    const route = useRoute();
    let name = route.params.name;
    axios
      .get('https://newsapi.org/v2/top-headlines?q='+name+'&country=ve&language=es&pageSize=10&apiKey=e4a148358abc436087c2df46a367587d')
      .then((response) => {
        this.datos = response.data.articles[0]
        this.fuente = response.data.articles[0].source.name
    })
  },
  methods: {
    setAltImg(event) { 
        event.target.src = "https://pbs.twimg.com/media/DjzCtwVX0AAVtpp.jpg" 
    },
    dateTime(value) {
      return moment(value).locale('es').format('d-m-y');
    },
  }

}
</script>
