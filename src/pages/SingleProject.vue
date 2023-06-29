<script>
import axios from 'axios';
export default {
    name: 'SingleProject',
    data () {
        return {
            project: [],
            baseUrl: 'http://127.0.0.1:8000/api',
        }
    },
    created () {
        this.getSingleProject()
    },
    methods: {
        getSingleProject(){
             axios.get(`${this.baseUrl}/projects/${this.$route.params.slug}`).then((response) => {
            if (response.data.success) {
                this.project = response.data.project;
            } else {
                // redirect alla pagina 404
                this.$router.push({ name: 'not-found' })
            }
        });
    },
}
}
</script>

<template>
    <div class="container">
        <h1 v-if="project" class="text-center my-5"> {{ project.title }} </h1>
        <p v-if="project"> <b>Customer: </b> {{ project.customer }} </p>
        <p v-if="project"> <b>Description: </b> {{ project.description }} </p>
        <h5>Type:</h5>
        <ul>
            <li v-for="(elem,index) in project.type" :key="index">
            {{ elem.name }}
            </li>
        </ul>
    </div>
</template>

<style lang="scss" scoped>

</style>