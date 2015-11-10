<style>
  .editing{
    background: #888;
  }
  .browser__item{
    color: white;
	position: relative;
	cursor: pointer;
  }
  
  .browser__item__delete{
	  display: none;
  }
  .browser__item:hover .browser__item__delete{
	  display: block;
	  position: absolute;
	  width: 1rem;
	  right: 0;
	  top: 0;
	  color: #AAA;
  }
  .browser__item__delete:hover{
	  color: #FFF!important;
  }
</style>

<template>
	<div class="browser__item" :class="{'editing': post.id === editingId}">
		{{post.title}}
		<div class="browser__item__delete" @click="remove">
			x
		</div>
	</div>
</template>

<script>
	module.exports = {
		props: ["post", "editingId"],
		data: function(){
			return {
			}
		},
		methods: {
			remove: function(){
				this.$dispatch("remove-post", this.post);
			}
		}
	}
	
</script>