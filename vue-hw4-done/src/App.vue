<template>
  <div v-if="!formDone">
	<div class="container mt-2">

		<form @submit.prevent="sendForm">
			<app-progress :current="fieldDone" :max="info.length" />
			<div>
				<app-field
					v-for="field,i in info"
					:key="i"
					:label="field.label"
					:value="field.value"
					:valid="field.valid"
					@updated="onUpdate(i, $event)"
				></app-field>
			</div>
			<button class="btn btn-primary mt-2" :disabled="!formReady">
				Send Data
			</button>
		</form>
	</div>

  <vue-final-modal
      v-model="showModal"
      classes="modal-container1"
      content-class="modal-content1"
  >
    <span># Simple modal</span>
    <table class="table table-bordered container mt-4">
      <tbody>
      <tr v-for="field,i in info" :key="i">
        <td>{{ field.label }}</td>
        <td>{{ field.value }}</td>
      </tr>
      </tbody>
    </table>
    <div align="center">
    <button class="btn btn-success btn-modal" @click="formDone = true">Ok</button>
      <button class="btn btn-info btn-modal"
              @click="showModal = false">Cancel</button>
      </div>
  </vue-final-modal>
  </div>
  <div v-else>All Done</div>
</template>

<style>
.btn-modal{
  display: inline-block;
  width: 10%;
  margin-left: 2px;
  margin-right: 2px;
}

.modal-container1 {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-left: auto;
  margin-right: auto;
}
.modal-content1 {
  position: relative;
  width: 50%;
  max-height: 400px;
  padding: 16px;
  overflow: auto;
  border-radius: 4px;
  background-color: white;
}
.modal-close {
  position: absolute;
  top: 0;
  right: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 32px;
  height: 32px;
  margin: 8px 8px 0 0;
  cursor: pointer;
}
.modal-close::hover {
  color: gray;
}
</style>

<script>
import AppField from './components/Field'
import AppProgress from './components/Progress'
import { VueFinalModal } from 'vue-final-modal'

export default {
	components: { AppField, AppProgress, VueFinalModal},
	data: () => ({
		info: [
			{
				label: 'Name',
				value: '',
				pattern: /^[a-zA-Z ]{2,30}$/
			},
			{
				label: 'Phone',
				value: '',
				pattern: /^[0-9]{7,14}$/
			},
			{
				label: 'Email',
				value: '',
				pattern: /.+/
			},
			{
				label: 'Some Field 1',
				value: '',
				pattern: /.+/
			},
			{
				label: 'Some Field 2',
				value: '',
				pattern: /.+/
			}
		],
		formDone: false,
    showModal: false
	}),
	computed: {
		fieldDone(){
			// return this.info.reduce((total, field) => total + (field.valid ? 1 : 0), 0)
			return this.info.filter(field => field.valid).length;
		},
		formReady(){
			return this.fieldDone >= this.info.length;
		},
		progressStyles(){
			let rel = this.fieldDone / this.info.length * 100;
			return { width: rel + '%' };
		}
	},
	methods: {
		onUpdate(i, val){
			let field = this.info[i];
			field.value = val.trim();
			field.valid = field.pattern.test(field.value);
		},
		sendForm(){
			if(this.formReady){
			        this.showModal = true;
			}
		}
	},
	created(){
		this.info.forEach(field => {
			field.valid = false;
		})
	}
}
</script>