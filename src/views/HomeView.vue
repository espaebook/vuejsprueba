<template>

    <div class="antialiased text-gray-800 dark:bg-black dark:text-gray-400">
        <div class="container px-8 mx-auto xl:px-5 max-w-screen-lg py-5 lg:py-8">
                <div class="absolute top-0 right-0 mt-5 mr-16">

                    <div class="flex align-middle">
                        <div class="w-2/5">
                            <div class="my-3">
                                <h5 class="text-base  font-semibold tracking-tight text-center lg:leading-snug text-brand-primary lg:text-base  dark:text-white">Pais</h5>
                                <!-- <label for="countries" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Pais</label> -->
                            </div>

                        </div>
                        <div class="w-3/5" id='pais'>
                            <select  @change="cambiarPais($event)" class="w-full px-4 py-3 border-2 placeholder:text-gray-800 dark:text-white rounded-md outline-none dark:placeholder:text-gray-200 dark:bg-gray-900 focus:ring-4 border-gray-300 focus:border-gray-600 ring-gray-100 dark:border-gray-600 dark:focus:border-white dark:ring-0">
                                <option :selected="true" v-for="pais in paises" :key="pais" :value="pais.id">{{ pais.name }}</option>                                
                            </select>                            
                        </div>
                    </div>
                    

                </div>            
            
            <div class="max-w-screen-md mx-auto">
                <h1 class="my-16 mb-3 text-3xl font-semibold tracking-tight text-center lg:leading-snug text-brand-primary lg:text-4xl dark:text-white">Ultimas noticias</h1>
            </div>

        </div>

        <div>
            <div class="container px-8 mx-auto xl:px-5 max-w-screen-lg">

                <Carousel :autoplay="2000" :wrap-around="true">
                    <Slide v-for="dato in datos.articles" :key="dato.status" :autoplay="2000">
                        <div class="carousel__item">
                            <div class="cursor-pointer group">
                                <div class="overflow-hidden transition-all bg-gray-100 rounded-md dark:bg-gray-800 hover:scale-105">

                                    <a v-if="dato.urlToImage" class="relative block aspect-video" :href="'/detalles/'+dato.title+'/'+pais">

                                        <img
                                        alt="Thumbnail"
                                        sizes="(max-width: 300px) 30vw, 33vw"                        
                                        :src="dato.urlToImage"
                                        @error="setAltImg"
                                        decoding="async"
                                        data-nimg="fill"            
                                        class="transition-all object-cover"
                                        style="position: absolute; height: 100%; width: 100%; left: 0; top: 0; right: 0; bottom: 0; color: transparent;"                                
                                        />
                                    </a>

                                    <a v-else class="relative block aspect-video" :href="'/detalles/'+dato.title+'/'+pais">

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
                        
                                <div>

                                    <h2 class="text-lg font-semibold leading-snug tracking-tight mt-2 dark:text-white">
                                        <a :href="'/detalles/'+dato.title+'/'+pais">
                                            <span class="bg-gradient-to-r from-green-200 to-green-100 dark:from-purple-800 dark:to-purple-900 bg-[length:0px_10px] bg-left-bottom bg-no-repeat transition-[background-size] duration-500 hover:bg-[length:100%_3px] group-hover:bg-[length:100%_10px]">
                                                {{ dato.title }}
                                            </span>
                                        </a>
                                    </h2>
                                    <div class="flex items-center mt-3 space-x-3 text-gray-500 dark:text-gray-400">
                                        <div class="flex items-center gap-3">
                                            
                                            <span v-if="dato.author" class="text-sm">Autor: {{ dato.author }}</span>
                                            <span v-else class="text-sm">Autor: Anónimo</span>                                            

                                        </div>
                                        <span class="text-xs text-gray-300 dark:text-gray-600">•</span><time class="text-sm">Fuente: {{ dato.source.name }}</time>                                
                                    </div>
                                </div>

                            </div>
                        </div>
                    </Slide>

                    <template #addons>
                        <Navigation />
                        <Pagination />
                    </template>
                </Carousel>

            </div>

            <div class="container mx-auto xl:px-5 max-w-screen-lg mt-16">
                <div class="max-w-screen-md mx-auto">
                    <h1 class="my-2 mb-3 text-3xl font-semibold tracking-tight text-center lg:leading-snug text-brand-primary lg:text-4xl dark:text-white">Buscar noticias</h1>
                </div>
            </div>



            <div class="container px-8 mx-auto xl:px-5 max-w-screen-lg">
                <div class="container w-1/2 mx-auto max-w-screen-lg">

                    <Form class="my-10" @submit="onSubmit">
                        <div class="mb-5">
                            
                            <Field
                            :rules="valida"
                                type="text"
                                placeholder="Buscar contenido"
                                autocomplete="false"
                                class="w-full px-4 py-3 border-2 placeholder:text-gray-800 dark:text-white rounded-md outline-none dark:placeholder:text-gray-200 dark:bg-gray-900 focus:ring-4 border-gray-300 focus:border-gray-600 ring-gray-100 dark:border-gray-600 dark:focus:border-white dark:ring-0"
                                name="contenido"
                            />
                            <div class="mt-1 text-red-600"><small><ErrorMessage name="contenido" /></small></div>
                        </div>
                        <button type="submit" class="w-full py-4 font-semibold text-white transition-colors bg-gray-900 rounded-md hover:bg-gray-800 focus:outline-none focus:ring-offset-2 focus:ring focus:ring-gray-200 px-7 dark:bg-white dark:text-black">
                            Buscar
                        </button>
                    </Form>
                </div>

                <div v-if="resultados.length != 0">         
                    
                    

                    <div v-if="resultados.totalResults > 0">

                        <aside class="sticky top-0 self-start w-full md:w-96">

                            <div class="my-5">

                                <div class="grid">

                                    <div class="flex items-center justify-start py-2">
                                        <h4 class="text-gray-800 dark:text-gray-400 pr-2">Mostrando</h4>
                                        <div class="inline-flex items-center justify-center font-bold px-2 h-6 text-sm bg-blue-50 text-blue-500 rounded-full shrink-0 dark:bg-gray-800 dark:text-gray-300">{{ resultados.totalResults }}</div>
                                        <h4 class="text-gray-800 dark:text-gray-400 pl-2">resultados</h4>
                                        
                                    </div>

                                </div>
                            </div>

                        </aside>

                        <div class="grid gap-10 lg:gap-10 md:grid-cols-2 mb-16">

                            <div v-for="resultado in resultados.articles" v-bind:key="resultado.status" class="cursor-pointer group">
                                <div class="overflow-hidden transition-all bg-gray-100 rounded-md dark:bg-gray-800 hover:scale-105">

                                    <a v-if="resultado.urlToImage" class="relative block aspect-video" :href="'/detalles/'+resultado.title+'/'+pais">

                                        <img
                                        alt="Thumbnail"
                                        sizes="(max-width: 300px) 30vw, 33vw"                        
                                        :src="resultado.urlToImage"
                                        @error="setAltImg"
                                        decoding="async"
                                        data-nimg="fill"            
                                        class="transition-all object-cover"
                                        style="position: absolute; height: 100%; width: 100%; left: 0; top: 0; right: 0; bottom: 0; color: transparent;"                                
                                        />
                                    </a>

                                    <a v-else class="relative block aspect-video" :href="'/detalles/'+resultado.title+'/'+pais">

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
                        
                                <div>
                                    <h2 class="text-lg font-semibold leading-snug tracking-tight mt-2 dark:text-white">
                                        <a :href="'/detalles/'+resultado.title+'/'+pais">
                                            <span class="bg-gradient-to-r from-green-200 to-green-100 dark:from-purple-800 dark:to-purple-900 bg-[length:0px_10px] bg-left-bottom bg-no-repeat transition-[background-size] duration-500 hover:bg-[length:100%_3px] group-hover:bg-[length:100%_10px]">
                                                {{ resultado.title }}
                                            </span>
                                        </a>
                                    </h2>
                                    <div class="flex items-center mt-3 space-x-3 text-gray-500 dark:text-gray-400">
                                        <div class="flex items-center gap-3">
                                            
                                            <span v-if="resultado.author" class="text-sm">Autor: {{ resultado.author }}</span>
                                            <span v-else class="text-sm">Autor: Anónimo</span>
                                            

                                        </div>
                                        <span class="text-xs text-gray-300 dark:text-gray-600">•</span><time class="text-sm">Fuente: {{ resultado.source.name }}</time>                    
                                    </div>
                                </div>

                            </div>

                        </div>


                    </div>

                    <div v-else>
                        <h2 class="text-lg font-semibold leading-snug tracking-tight my-16 dark:text-white">                        
                            <span class="bg-gradient-to-r from-green-200 to-green-100 dark:from-purple-800 dark:to-purple-900 bg-[length:0px_10px] bg-left-bottom bg-no-repeat transition-[background-size] duration-500 hover:bg-[length:100%_3px] group-hover:bg-[length:100%_10px]">
                                No se encontraron resultados
                            </span>
                        </h2>
                    </div>



                </div>

            </div>
        </div>

    </div>



</template>

<script>

import axios from 'axios'
import { Carousel, Navigation, Pagination, Slide } from 'vue3-carousel'
import 'vue3-carousel/dist/carousel.css'
import { Form, Field, ErrorMessage } from 'vee-validate';

export default {

  data() {
    return {     
    
        datos: [],
        resultados: [],
        paises: [{"id":"ar", "name":"Argentina"}, {"id":"au", "name":"Australia"}, {"id":"at", "name":"Austria"}, {"id":"be", "name":"Bélgica"}, {"id":"br", "name":"Brasil"}, {"id":"bg", "name":"Bulgaria"}, {"id":"ca", "name":"Canadá"}, {"id":"cn", "name":"Porcelana"}, {"id":"co", "name":"Colombia"}, {"id":"cu", "name":"Cuba"}, {"id":"cz", "name":"Republica checa"}, {"id":"eg", "name":"Egipto"}, {"id":"fr", "name":"Francia"}, {"id":"de", "name":"Alemania"}, {"id":"gr", "name":"Grecia"}, {"id":"hk", "name":"Hong Kong"}, {"id":"hu", "name":"Hungría"}, {"id":"in", "name":"India"}, {"id":"id", "name":"Indonesia"}, {"id":"ie", "name":"Irlanda"}, {"id":"il", "name":"Israel"}, {"id":"it", "name":"Italia"}, {"id":"jp", "name":"Japón"}, {"id":"lv", "name":"letonia"}, {"id":"lt", "name":"Lituania"}, {"id":"my", "name":"Malasia"}, {"id":"mx", "name":"México"}, {"id":"ma", "name":"Marruecos"}, {"id":"nl", "name":"Países Bajos"}, {"id":"nz", "name":"Nueva Zelanda"}, {"id":"ng", "name":"Nigeria"}, {"id":"no", "name":"Noruega"}, {"id":"ph", "name":"Filipinas"}, {"id":"pl", "name":"Polonia"}, {"id":"pt", "name":"Portugal"}, {"id":"ro", "name":"Rumania"}, {"id":"ru", "name":"Rusia"}, {"id":"sa", "name":"Arabia Saudita"}, {"id":"rs", "name":"Serbia"}, {"id":"sg", "name":"Singapur"}, {"id":"sk", "name":"Eslovaquia"}, {"id":"si", "name":"Eslovenia"}, {"id":"za", "name":"Sudáfrica"}, {"id":"kr", "name":"Corea del Sur"}, {"id":"se", "name":"Suecia"}, {"id":"ch", "name":"Suiza"}, {"id":"tw", "name":"Taiwán"}, {"id":"th", "name":"Tailandia"}, {"id":"tr", "name":"Pavo"}, {"id":"ae", "name":"Emiratos Árabes Unidos"}, {"id":"ua", "name":"Ucrania"}, {"id":"gb", "name":"Reino Unido"}, {"id":"us", "name":"Estados Unidos"}, {"id":"ve", "name":"Venezuela"}],
        selected: "Venezuela",
        pais: "", 
                
    }
  },
  mounted() {  

    if (!this.pais) {
        this.pais = 've';
    }

    axios
      .get('https://newsapi.org/v2/top-headlines?country=ve&language=es&pageSize=10&apiKey=b58ea758718441f6bed89f6b379daec8')
      .then((response) => {
        this.datos = response.data
    })
  },
  methods: {
    setAltImg(event) { 
        event.target.src = "https://pbs.twimg.com/media/DjzCtwVX0AAVtpp.jpg" 
    },
    onSubmit(values) {
      this.busca(values.contenido);
    },
    valida(value) {

      if (!value) {
        return 'Este campo es requerido';
      }

      if (value.length < 3) {
        return 'El mínimo de caracteres debe ser de 3';
      }

      return true;
    },
    busca(values) {    

        axios
        .get('https://newsapi.org/v2/top-headlines?q='+values+'&country='+this.pais+'&pageSize=10&apiKey=b58ea758718441f6bed89f6b379daec8')
        .then((response) => {    
            this.resultados = response.data
        })
    },
    cambiarPais(event) {
        this.pais = event.target.value
        axios
        .get('https://newsapi.org/v2/top-headlines?country='+event.target.value+'&apiKey=b58ea758718441f6bed89f6b379daec8')
        .then((response) => {
            this.datos = response.data
        })
    }

    // sendToView(view) {

    //     this.$router.push({path: '/newpath', query : { foo: "bar"}});
    // },
  },
  components: {
    Carousel,
    Slide,
    Pagination,
    Navigation,
    Form,
    Field,
    ErrorMessage,
  },
}
</script>

<style>
.carousel__item {
  min-height: 200px;
  width: 75%;
  font-size: 20px;
  border-radius: 8px;
  justify-content: center;
  align-items: center;
}

.carousel__slide {
  padding: 10px;
}

.carousel__prev,
.carousel__next {
  box-sizing: content-box;
}
</style>
