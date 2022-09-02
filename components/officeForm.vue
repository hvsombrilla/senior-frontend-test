<template>
	<form class="bg-white mt-5" @submit.prevent="sendData()">
		<FormField
			ref="title"
			label="Title"
			placeholder="Ej: Office 1"
			:value="editingData.title"
			@input="
				(event) => {
					updateEditingData('title', event);
				}
			"
			:show-validations="this.showValidations"
			:required="true"
		></FormField>

		<FormField
			ref="address"
			label="Enter the address"
			placeholder="Ej: XYZ Street, New York, USA"
			:value="editingData.address"
			@input="
				(event) => {
					updateEditingData('address', event);
				}
			"
			:show-validations="this.showValidations"
			:required="true"
		></FormField>

		<h4 class="text-xs uppercase text-cyan-500">Contact Information</h4>
		<hr class="my-4" />

		<FormField
			ref="name"
			label="Full Name"
			placeholder="Ej: John Doe"
			:value="editingData.contact.name"
			@input="
				(event) => {
					updateEditingData('name', event);
				}
			"
			:show-validations="this.showValidations"
			:required="true"
		></FormField>

		<FormField
			ref="position"
			label="Job Position"
			placeholder="Job Position"
			:value="editingData.contact.position"
			@input="
				(event) => {
					updateEditingData('position', event);
				}
			"
			:show-validations="this.showValidations"
			:required="true"
		></FormField>

		<FormField
			ref="email"
			label="Email address"
			placeholder="Ej: jhon.doe@xyz.com"
			type="email"
			:value="editingData.contact.email"
			@input="
				(event) => {
					updateEditingData('email', event);
				}
			"
			:show-validations="this.showValidations"
			:required="true"
		></FormField>

		<FormField
			ref="phone"
			label="Phone Number"
			placeholder="Phone Number"
			type="tel"
			:value="editingData.contact.phone"
			@input="
				(event) => {
					updateEditingData('phone', event);
				}
			"
			:show-validations="this.showValidations"
			:required="true"
		></FormField>

		<div class="flex items-center justify-between">
			<button
				type="submit"
				:disabled="!formIsValid"
				class="bg-cyan-600 hover:bg-cyan-500 disabled:bg-gray-400 text-white py-2 px-4 rounded focus:outline-none focus:shadow-outline"
			>
				Save
			</button>
		</div>
	</form>
</template>

<script>
import { ref } from "vue";
import FormField from "./elements/formField.vue";

export default {
	name: "OfficeForm",
	data() {
		let emptyData = {
			title: "",
			address: "",
			contact: { name: "", position: "", phone: "", email: "" },
		};
		let edata;
		let isEditing = true;
		//check if office is undefined
		if (this.office === undefined) {
			edata = emptyData;
			isEditing = false;
		} else {
			edata = JSON.parse(JSON.stringify(this.office));
		}

		return {
			showValidations: false,
			isOpen: false,
			isRemoved: false,
			editingData: edata,
			isEditing: isEditing,
			formIsValid: false
		};
	},
	props: {
		office: {
			type: Object,
			required: false,
		},
		index: {
			type: Number,
			required: false,
		},
		editOfficeFunt: Function,
		createOfficeFunt: Function,
		toggle: Function,
	},
	methods: {
		updateEditingData(key, value) {
			let officeProps = ["title", "address"];

			if (officeProps.includes(key)) {
				this.editingData[key] = value;
			} else {
				this.editingData.contact[key] = value;
			}

			this.validate();
		},
		validate(){
			this.$refs.title.validate();
			this.$refs.address.validate();
			this.$refs.name.validate();
			this.$refs.position.validate();
			this.$refs.email.validate();
			this.$refs.phone.validate();

			this.formIsValid = (this.$refs.title.isValid && this.$refs.address.isValid && this.$refs.name.isValid && this.$refs.position.isValid && this.$refs.email.isValid && this.$refs.phone.isValid);
		
		},
		sendData() {

			console.log("send data");
			this.showValidations = true;

			this.validate();

			//wait for validation to finish
			setTimeout(() => {
				if (this.formIsValid) {
					if (this.isEditing) {
						this.editOfficeFunt(this.editingData, this.index);
						this.toggle();
					} else {
						this.createOfficeFunt(this.editingData);
					}
					
				}
			}, 500);


		},
	},
	components: { FormField },
};
</script>
