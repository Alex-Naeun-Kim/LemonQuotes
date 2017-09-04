<template>
<div class="row">
	<h1>Lemon Quotes</h1>
    <div class="search">
      <!-- <input type="text" v-model="searchQuery" /> -->
      <!-- <input type="text" v-model="inSearch" /> -->
      <input type="text" @change="innerSearch" /><!-- prop를 input에 바인드할 때는 이렇게-->
      <span class="icon-search"></span>
    </div>
</div>
</template>

<script>
	export default {
		props: ['inSearch'],
		methods: {
			innerSearch(){
				this.$emit('update', this.inSearch);
			}
		}

	}
</script>