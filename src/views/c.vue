<template>
	<div>
		<h3>C</h3>
		<a href="javascript:;" @click="handleJump('d')">D</a>
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