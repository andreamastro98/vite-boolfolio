<script>
import axios from 'axios';
export default {
    name: 'ProjectList',
    data(){
        return{
          projects:[],
          baseUrl:'http://127.0.0.1:8000/api',
          currentPage:1,
          lastPage:null,
          types:null,
          selectedType:'all',
          technologies:null,
          selectedTechnologies:[],
        }
    },
  mounted(){
    this.getProjects(1);
    this.getTypes();
    this.getTechnologies();
  },
  watch: {
    selectedTechnologies: {
      handler: 'getProjects',
      deep: true
    }
  },
  methods:{
    getProjects(projectApiPage){

      const params = {
        page: projectApiPage
      }

      if(this.selectedType !=='all'){
        params.type_id = this.selectedType
      }

      if (this.selectedTechnologies.length > 0) {
        params.technologies_ids = this.selectedTechnologies.join(',')
      }

      //console.log('params info', params)

      axios.get(`${this.baseUrl}/projects`,{ params }).then(res=>{
        console.log(res.data);
        //inserire nella variabile projects i dati ricevuti dall'api
        this.projects = res.data.projects.data;
        this.currentPage = res.data.projects.current_page;
        this.lastPage = res.data.projects.last_page;

      })
    },

    getTypes() {
      axios.get(`${this.baseUrl}/types`).then(res => {
        this.types = res.data.types
        //console.log(this.types)
      })
    },

    getTechnologies() {
      axios.get(`${this.baseUrl}/technologies`).then(res => {
        this.technologies = res.data.technologies
        console.log(this.technologies)
      })
    }

  },
}
</script>

<template>
    <section class="py-5">
          <div class="container px-5 mb-5">
              <div class="text-center mb-5">
                  <h1 class="display-5 fw-bolder mb-0"><span class="text-gradient d-inline">Projects</span></h1>
              </div>
              <div class="d-flex justify-content-around">

                <div class="d-flex">
                  <!-- checks -->
                  <div class="form-check me-3" v-for="(elem, index) in technologies">
                    <input class="form-check-input" type="checkbox" :value="elem.id" id="defaultCheck1" v-model="selectedTechnologies">
                    <label class="form-check-label" for="defaultCheck1">
                      {{elem.name}}
                    </label>
                  </div>
                </div>

                <div>
                  <!-- select -->
                  <div class="container mb-5">
                    <select @change="getProjects" class="form-select" v-model="selectedType" aria-label="Default select example">
                      <option value="all">-- All --</option>
                      <option v-for="(elem,index) in types" :value="elem.id" :key="index">{{ elem.name }}</option>
                    </select>
                  </div>
                </div>               

              </div>

              <div class="row gx-5 justify-content-center">
                  <div class="col-lg-11 col-xl-9 col-xxl-8">
                      <!-- Project Card 1-->
                        <div class="card overflow-hidden shadow rounded-4 border-0 mb-5" v-for="(elem, index) in projects" :key="index">
                            <div class="card-body p-0">
                                <div class="d-flex align-items-center">
                                    <div class="p-5 desc">
                                        <h2 class="fw-bolder">{{ elem.title }}</h2>
                                        <h6 v-if="elem.type" class="fw-bolder">{{ elem.type.name }}</h6>                                   
                                        <p v-if="elem.description">{{ elem.description }}</p>
                                        <button class="btn btn-primary">
                                            <Router-link :to="{ name: 'SingleProject', params: { slug: elem.slug } }" class="nav-link" href="#">Show</Router-link>
                                        </button>                                        
                                    </div>
                                    <img class="img-fluid" :src="`http://127.0.0.1:8000/storage/${elem.cover_image}`" alt="Title"/>
                                </div>
                            </div>
                        </div>                                
                  </div>
              </div>
              <nav aria-label="Page navigation example">
                <ul class="pagination">
                  <li class="page-item"><a class="page-link" @click.prevent="getProjects(currentPage - 1)" href="#">Previous</a></li>

                  <li class="page-item" :class="(currentPage === elem) ? 'active' : ''" v-for="(elem, index) in lastPage" :key="index">
                    <a class="page-link" href="#" @click.prevent="getProjects(elem)">{{ elem }}</a>
                  </li>
              
                  <li class="page-item"><a class="page-link" @click.prevent="getProjects(currentPage + 1)" href="">Next</a></li>
                </ul>
              </nav>
          </div>
      </section>
</template>


<style lang="scss" scoped>
.img-fluid{
    width: 40%;
}

.desc{
    width: 60%;
}

.form-select{
  width: auto;
}

</style>