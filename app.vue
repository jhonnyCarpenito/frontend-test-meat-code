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
	<Hero />

	<!-- Nuestros articulos -->
	<main class="articles_container">
		<section>
			<!-- <div class="articles_title_container">
				<SectionTitle title="Nuestros artículos" />
			</div> -->

			<div class="articles_content">
				<!-- Articulos -->
				<Articles :articles="articles" />

			</div>
		</section>

		<!-- Contactanos -->
		<section>
			<div class="newsletter_title_container">
				<SectionTitle title="Contáctanos" />
			</div>

			<!-- Formulario-->
			<FormContact />
		</section>
	</main>

	</Html>
</template>

<script>
import Articles from './components/Articles.vue'
import Hero from './components/Hero.vue'
import FormContact from './components/FormContact.vue'
import SectionTitle from './components/SectionTitle.vue'

export default {
	components: {
		Articles,
		Hero,
		FormContact,
		SectionTitle
	},
}
</script>