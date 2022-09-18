<template>
	<div class="post">
			<div class="">
				<div class=""> <strong>Id:</strong> {{post.id}}</div>
				<div class=""> <strong>Назва:</strong> {{post.title}}</div>
				<div class=""> <strong> Опис:</strong> {{post.body}}</div>
			</div>
			<div class="">
				<my-button
				@click="$emit('remove', post)"
				>Вилучити</my-button>
			</div>
			
	</div>
</template>

<script>
export default {
	components: {
		
	},
	props: {
		post: {
			type: Object,
			requred: true,
		}
	}
}
</script>

<style scoped>
.post {
	margin-bottom: 10px;
	text-align: left;
	padding: 10px;
	border: 2px solid rgb(42, 190, 42);
	display: flex;
	align-items: center;
	justify-content: space-between;
}
</style>