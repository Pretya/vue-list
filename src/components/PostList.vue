<template>
		<div class="">
			<h3>Список</h3>
			<post-item 
				v-for="post in posts"
				:post="post"
				:key="post.id"
				@remove="$emit('remove', post)"
			>
			</post-item>
		</div>
</template>

<script>
	import PostItem from './PostItem.vue';
export default {
	components: {
		PostItem
	},
props: {
	posts: {
		type: Array,
		required: true
	}
}
}
</script>

<style>
	h3 {
		margin: 20px 0;
	}
</style>