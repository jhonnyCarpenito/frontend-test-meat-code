<template>
  <Html lang="es">

    <Head>
      <Title>El secreto de tu cocina</Title>
      <Meta name="description" content="El secreto de tu cocina" />
      <link rel="preconnect" href="https://fonts.googleapis.com" />
      <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="" />
      <link
        href="https://fonts.googleapis.com/css2?family=Caveat:wght@400;500;600;700&family=Open+Sans:wght@400;500;600;700&display=swap"
        rel="stylesheet" />
    </Head>

    <!-- Hero -->
    <div class="hero_container">
      <header class="hero_header">
        <h2 class="hero_logo">
          Logo
        </h2>

        <div class="social_media">
          <a href="#">
            <img src="~/assets/img/Facebook.svg" alt="Facebook image" />
          </a>

          <a href="#">
            <img src="~/assets/img/Instagram.svg" alt="Instagram image" />
          </a>

          <a href="#">
            <img src="~/assets/img/Youtube.svg" alt="Youtube image" />
          </a>
        </div>
      </header>

      <div class="hero_background">
        <img src="~/assets/img/hero.png" class="hero_img" alt="Hero image" />
      </div>

      <div class="hero_title_container">
        <h1 class="hero_title">
          El secreto <br />
          de tu cocina
        </h1>

        <img src="~/assets/img/brush.png" class="hero_title_brush"
          alt="Brush image" />
      </div>
    </div>

    <!-- Nuestros articulos -->
    <main class="articles_container">
      <section>
        <div class="articles_title_container">
          <h2
            class="section_title">
            Nuestros articulos
          </h2>
        </div>

        <div class="articles_content">
          <!-- Filtro de Articulos -->
          <div class="articles_filter">
            <button v-for="filter in filters" :key="filter" class="article_filter_option"
              :class="{
                'article_filter_text_selected': isActiveFilter === filter,
              }" @click="() => handleFilter(filter)">
              {{ filter }}

              <img src="~/assets/img/Arrow.svg" alt="Arrow Image" v-if="isActiveFilter === filter">
            </button>
          </div>

          <!-- Articulos -->
          <div class="articles ">
            <!-- Articulo -->
            <article v-for="article in articles.slice(0, 9)"
              class="article">
              <img :src="article.image" width="270" height="200" alt="Image Preview" />

              <div class="article_content">
                <p class="article_title">
                  {{ article.title }}
                </p>
                <p class="article_text">
                  {{ article.content }}
                </p>

                <a href="#" class="article_button">ver más</a>
              </div>
            </article>
          </div>
        </div>
      </section>

      <!-- Contactanos -->
      <section>
        <div class="newsletter_title_container">
          <h2
            class="section_title">
            Contáctanos
          </h2>
        </div>

        <!-- Formulario-->
        <form class="newsletter_form" @submit.prevent="handleSubmit()">
          <div class="newsletter_inputs_container">
            <div>
              <label class="newsletter-input-text" for="name">NOMBRE</label>
              <input id="name" type="text" v-model="form.firstname"
                class="newsletter_input"
                required />
            </div>
            <div>
              <label class="newsletter-input-text" for="lastname">APELLIDO</label>
              <input id="lastname" type="text" v-model="form.lastname"
                class="newsletter_input"
                required />
            </div>

            <div>
              <label class="newsletter-input-text" for="mail">MAIL</label>
              <input id="mail" type="email" v-model="form.email"
                class="newsletter_input"
                required />
            </div>

            <div>
              <label class="newsletter-input-phone" for="phone">téléfono</label>
              <input id="phone" type="tel" v-model="form.phone"
                class="newsletter_input"
                required />
            </div>
          </div>

          <button :disabled="isDisabledButton"
            class="newsletter_form_button">
            ENVIAR
          </button>
        </form>
      </section>
    </main>

  </Html>
</template>

<script>
import axios from 'axios'
import Swal from 'sweetalert2'

export default {
  data() {
    return {
      articles: [],
      form: {
        firstname: '',
        lastname: '',
        email: '',
        phone: ''
      },
      isActiveFilter: "todos",
      filters: ["todos", "productos", "recetas", "consejos"],
      isDisabledButton: false
    }
  },
  mounted() {
    this.fetchingFilter()
  },
  methods: {
    handleSubmit() {
      this.isDisabledButton = true
      axios.post('https://5eed24da4cbc340016330f0d.mockapi.io/api/newsletter', this.form)
        .then(response => {
          Swal.fire({
            icon: 'success',
            title: '¡Gracias!',
            text: 'Tu información ha sido enviada',
          })
        }).catch(error => {
          Swal.fire({
            icon: 'error',
            title: 'Oops...',
            text: 'Hubo un error, intenta de nuevo',
          })
        }).finally(() => {
          this.isDisabledButton = false
          this.form = {
            firstname: '',
            lastname: '',
            email: '',
            phone: ''
          }
        })
    },
    handleFilter(filter) {
      this.isActiveFilter = filter
      this.fetchingFilter()
    },
    fetchingFilter() {
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

    }

  }
}

</script>

<style lang="scss" scoped>


</style>