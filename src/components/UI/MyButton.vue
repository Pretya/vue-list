<template>
	<button class="btn">
		<slot></slot>
	</button>
</template>

<script>
export default {
	name: 'my-button'
}
</script>

<style>
	.btn {
	padding: 10px;
	margin: 10px 0;
	cursor: pointer;
	width: 100px;
	align-self: flex-end;
	background: none;
	border: 1px solid green;
	color: green;
	}
</style>