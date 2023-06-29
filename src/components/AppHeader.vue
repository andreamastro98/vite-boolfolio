<script>
export default {
    name: "AppHeader",
    data () {      
        return {

        }
    }
}
</script>

<template>
    <div>
        <nav class="navbar navbar-expand-lg bg-body-tertiary">
            <div class="container">
              <Router-link :to="{ name: 'home' }" class="navbar-brand text-gradient bg-gradient-primary-to-secondary" href="#">Portfolio</Router-link>
              <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
              </button>
              <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav">
                  <li class="nav-item">
                    <Router-link :to="{ name: 'projects' }" class="nav-link" href="#">Projects</Router-link>
                  </li>
                </ul>
              </div>
            </div>
        </nav>
    </div>
</template>


<style lang="scss" scoped>

</style>