<template>
		<div class="mb-4">
			<label class="block text-gray-600 text-sm mb-2">
				{{ label }}
			</label>
			<input
				class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
				:type="type"
                :class="{ 'border-red-500': (!this.isValid && this.showValidations ) }"
				:value="value"
				:placeholder="placeholder"
                @focusout="() => {
                    this.showValidations = true;
                    validate();
                    }"
				@input="
                    (event) => {                  
                        this.$emit('input', event.target.value);                        
                    }"
                
			/>

            <ul v-if="this.showValidations">
                <li class="text-red-500 text-xs italic"  v-for="(error, index) in errors" :key="index">{{ error }}</li>
            </ul>

		</div>
</template>

<script>
import { ref } from 'vue';

export default {
	name: "FormField",
	data() {
		return {
            isValid: false,
            errors: [],
        };
	},
	props: {
        label : {
            type : String,
            required : true
        },
        required : {
            type : Boolean,
            default : false
        },
        value : {
            type : String,
            required : false
        },
        placeholder : {
            type : String,
            required : false
        },
        type : {
            type : String,
            required : false,
            default : "text"
        },
        showValidations : {
            type : Boolean,
            required : false,
            default : false
        },
    },
	methods: {
        isValidEmail(email) {
            var re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
            let msg = "";
            return re.test(String(email).toLowerCase());

        },

        isValidPhone(phone){
            let msg = "";
            let re = /^[\+]?[(]?[0-9]{3}[)]?[-\s\.]?[0-9]{3}[-\s\.]?[0-9]{4,6}$/im;

            console.log("telefono", phone, re.test(String(phone)));
            return re.test(String(phone));
        },

        validate() {

            let errors = [];

           // if (!this.showValidations) { return }

            let valid = true;

            if (this.type === "email") {

                if (! this.isValidEmail(this.value) ) {
                    valid = false;
                    errors.push("Must be a valid email address");
                } 
            }

            if (this.type === "tel") {
                if (! this.isValidPhone(this.value) ) {
                    valid = false;
                    errors.push("Must be a valid phone number");
                }
            }


            if(this.value === "" && this.required) {
                valid = false;
                errors.push("This field is required");
            }

            this.isValid = valid;
            this.errors = errors;
        }
    },
};
</script>

