<template>
<div>
	<div v-show="isActive == true" class="modal is-active">
		<div @click="isActive = false" class="modal-background"></div>
		<div class="modal-content">
			<div class="box">
				<slot></slot>
			</div>
		</div>
		<button @click="isActive = false" class="modal-close is-large" aria-label="close"></button>
	</div>
	<button @click="isActive = true" class="button is-primary">{{ button }}</button>
</div>
</template>

<script>
export default{
	props: ["button"],
	data(){
		return {
			isActive: false
		}
	}
}
</script>