<script setup>
import { ref } from 'vue';
import BlogPost from './components/BlogPost.vue';
import PaginatePost from './components/PaginatePost.vue';
import LoadingSpinner from './components/LoadingSpinner.vue';

const posts = ref([]);
const postXpage = 10; //este valor va ser fijo
const inicio = ref(0); //este valor va ser dinámico
const fin = ref(postXpage);
const loading = ref(true);

const favorito = ref('');

const cambiarFavorito = (title) => {
	favorito.value = title;
};

const next = () => {
	inicio.value = inicio.value + postXpage;
	fin.value = fin.value + postXpage;
};

const prev = () => {
	inicio.value = inicio.value - postXpage;
	fin.value = fin.value - postXpage;
};

/*
fetch('https://jsonplaceholder.typicode.com/posts')
	.then((res) => res.json())
	.then((data) => (posts.value = data));
*/
const fetchData = async () => {
	try {
		const res = await fetch('https://jsonplaceholder.typicode.com/posts');
		posts.value = await res.json();
		console.log(posts.value);
	} catch (error) {
		console.log(error);
	} finally {
		setTimeout(() => {
			loading.value = false;
		}, 1500);
	}
};
fetchData();
</script>

<template>
	<LoadingSpinner v-if="loading" />
	<div class="container" v-else>
		<h1>App</h1>
		<h2>Mis post Favoritos: {{ favorito }}</h2>

		<PaginatePost
			@next="next"
			@prev="prev"
			:inicio="inicio"
			:fin="fin"
			:maxLength="posts.length"
		/>
		<BlogPost
			v-for="post in posts.slice(inicio, fin)"
			:key="post.id"
			:title="post.title"
			:id="post.id"
			:body="post.body"
			@cambiarFavorito="cambiarFavorito"
		/>
	</div>
</template>
<!--
 dentro del Setup se esta creando todo el Script y luego se monta el Template 
 -->
