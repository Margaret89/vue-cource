<template>
	<div class="app">
		<h1>Страница с постами</h1>
		<input type="text" v-model.trim="modificatorValue">

		<my-button @click="fetchPosts">Получить посты</my-button>

		<my-button @click="showDialog" style="margin: 20px 0;">Создать пост</my-button>

		<my-dialog v-model:show='dialogVisible'>
			<post-form @create = "creatPost"/>
		</my-dialog>

		<post-list
			 :posts="posts" 
			 @remove="removePost"
		/>
	</div>
</template>

<script>
import PostForm from "@/components/PostForm.vue"
import PostList from "@/components/PostList.vue"
import axios from "axios"

export default {
	components: {
		PostForm, PostList
	},
	data() {
		return{
			posts: [
				{id:1, title:'Javascript', body:'Описание поста'},
				{id:2, title:'Javascript 2', body:'Описание поста 2'},
				{id:3, title:'Javascript 3', body:'Описание поста 3'},
				{id:4, title:'Javascript 4', body:'Описание поста 4'},
			],
			dialogVisible: false,
			modificatorValue: '',
		}
	},
	methods: {
		creatPost(post) {
			this.posts.push(post);
			this.dialogVisible = false
		},
		removePost(post) {
			this.posts = this.posts.filter(p => p.id !== post.id)
		},
		showDialog() {
			this.dialogVisible = true
		},
		async fetchPosts() {
			try {
				const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10');
				console.log(response);
			} catch (e) {
				alert('Ошибка')
			}
		}
	}
}
</script>

<style>
/* <style scoped> scoped говорит, что стили применяются только внутри этого компонента */

*{
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}

.app{padding: 20px;}
</style>
