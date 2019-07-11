<template>
	<div style="height: 100vh; background-color: #ddd; overflow: hidden;">
		<h3>A</h3>
		<a href="javascript:;" @click="handleJump('b')">B</a>
		<a href="javascript:;" @click="handleJump('c')">C</a>
	</div>
</template>

<script>
export default {
	name: '',
	data() {
		return {
			
		}
	},
	mounted() {
		
	},
	methods: {
		handleJump(name) {
			this.$router.push({name: name});
		}
	}
}
</script>