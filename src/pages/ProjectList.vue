<script>
import axios from 'axios';
export default {
    name: 'ProjectList',
    data(){
        return{
          projects:[],
          baseUrl:'http://127.0.0.1:8000/api',
          currentPage:1,
          lastPage:null
        }
    },
  mounted(){
    this.getProjects(1);
  },
  methods:{
    getProjects(projectApiPage){
      axios.get(`${this.baseUrl}/projects`,{
        params:{
          page: projectApiPage
        }
      }).then(res=>{
        console.log(res.data);
        //inserire nella variabile projects i dati ricevuti dall'api
        this.projects = res.data.projects.data;
        this.currentPage = res.data.projects.current_page;
        this.lastPage = res.data.projects.last_page;

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

</style>