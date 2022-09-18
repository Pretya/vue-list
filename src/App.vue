<template>
<div class="app">

	<my-input
		placeholder="Пошук"
		v-model="searchQuery"
	>
	</my-input>

	<div class="app__btns">
		<my-button 
			@click="fetchPosts"
		>
			Отримати список
		</my-button>

		<my-select
			v-model="selectedSort"
			:options="sortOptions"
		>
			
		</my-select>
	</div>

	<post-list 
		:posts="searchPost"
		@remove="removePost"
	>
	</post-list>

	<div 
		ref="observer" 
		class="observer"
	>
	</div>

</div>

</template>

<script>
	import PostList from '@/components/PostList';
	import axios from 'axios';
	export default {
		components: {
			PostList
		},
		data() {
			return {
				posts: [],
				selectedSort: '',
				searchQuery: '',
				page: 1,
				limit: 10,
				totalPage: 0,
				sortOptions: [
					{value: 'title', name: 'За назвою'},
					{value: 'body', name: 'За описом'},
				]
			}
		},
		methods: {
			removePost(post) {
				this.posts = this.posts.filter(p => p.id !== post.id)
			},
			async fetchPosts() {
				try {
					
					const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
						params: {
							_page: this.page,
							_limit: this.limit,
						}
					});
					this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit);
					this.posts = response.data;
				} catch (e) {
					alert('Error')
				}
			},
			async loadPosts() {
				try {
					this.page += 1;
					const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
						params: {
							_page: this.page,
							_limit: this.limit,
						}
					});
					this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit);
					this.posts = [...this.posts, ...response.data];
				} catch (e) {
					alert('Error')
				}
			}
		},
		mounted() {
			this.fetchPosts();
			let options = {
				rootMargin: '0px',
				threshold: 1.0
			};
			let callback = (entries, observer) => {
				if(entries[0].isIntersecting) {
					this.loadPosts();
				}
			};
			let observer = new IntersectionObserver(callback, options);
			observer.observe(this.$refs.observer);
		},
		computed: {
			sortedPosts() {
				return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]));
			},
			searchPost() {
				return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
			}
		},
	}
</script>

<style>
	* {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
}
.app {
	padding: 20px;
}
.app__btns {
	display: flex;
	justify-content: space-between;
}
.page__wrapper {
	display: flex;
	margin-top: 15px;
}
.page {
	border: 1px solid black;
	padding: 10px;
	margin-right: 5px;
}
.current-page {
	border: 2px solid teal
}

</style>
