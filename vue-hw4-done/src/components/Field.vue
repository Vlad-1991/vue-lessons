<template>
	<div class="form-group">
		<label>
			{{ label }}

      <transition name="fade" mode="out-in" v-if="activated" appear
                  appear-active-class="create">
			<fa-icon v-if="valid"
				class="text-success"
				icon="check-circle"
			/>
        <fa-icon v-else
                 class="text-danger"
                 icon="circle-exclamation"
        />
      </transition>
		</label>
		<input type="text" class="form-control" :value="value" @input="onInput">
	</div>
</template>

<style>

.create{
  animation: createAppear 0.5s;
}

.fade-enter-active{
  animation: fadeIn 0.5s;
}

.fade-leave-active{
  animation: fadeOut 0.5s;
}

@keyframes fadeIn {
  from{ opacity: 0; transform: rotateY(-90deg); }
  to{ opacity: 1; transform: rotateY(0); }
}

@keyframes fadeOut {
  from{ opacity: 1; transform: rotateY(0); }
  to{ opacity: 0; transform: rotateY(90deg); }
}

@keyframes createAppear {
  from{ opacity: 0}
  to{ opacity: 1}
}

{

}
</style>

<script>
export default {
	props: {
		label: { type: String, required: true },
		value: { type: String, required: true },
		valid: { type: Boolean, required: true }
	},
	data(){
		return {
			activated: this.value != ''
		}
	},
	methods: {
		onInput(e){
			this.activated = true;
			this.$emit('updated', e.target.value)
		}
	}
}
</script>


