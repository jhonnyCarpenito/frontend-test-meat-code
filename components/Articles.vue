<template>
    <!-- Filtro de Articulos -->
    <div class="articles__filter">
        <button v-for="filter in filters" :key="filter" class="articles__filter-option" :class="{
            'articles__filter-option--selected': isActiveFilter === filter,
        }" @click="() => handleFilter(filter)">
            {{ filter }}

            <img src="~/assets/img/Arrow.svg" alt="Arrow Image" v-if="isActiveFilter === filter">
        </button>
    </div>

    <div v-if="isLoading" class="loader__container">
        <span class="loader"></span>
    </div>
    <!-- Articulos -->
    <div v-else class="articles ">

        <!-- Articulo -->
        <Article v-for="article in articles.slice(0, 9)" :article="article" />
    </div>
</template>

<script>
import axios from 'axios'
import Article from './Article.vue'
export default {
    components: {
        Article,
    },
    data() {
        return {
            articles: [],
            isActiveFilter: "todos",
            filters: ["todos", "productos", "recetas", "consejos"],
            isDisabledButton: false,
            isLoading: false,
        }
    },
    mounted() {
        this.fetchingFilter()
    },
    methods: {
        handleFilter(filter) {
            this.isActiveFilter = filter
            this.fetchingFilter()
        },
        fetchingFilter() {
            this.isLoading = true
            axios.get('https://5eed24da4cbc340016330f0d.mockapi.io/api/articles', {
                params: {
                    filter: this.isActiveFilter === "todos" ? "" : this.isActiveFilter,
                },
            })
                .then(response => {
                    console.log("response", response.data);
                    this.articles = response.data;
                    console.log("fetch", this.articles);
                })
                .catch(error => {
                    console.log(error);
                })
                .finally(() => {
                    this.isLoading = false
                })
        },
    }
}
</script>