<template>
	<div
		class="relative bg-white px-0 mb-5 shadow-xl ring-1 ring-gray-900/5 rounded-lg overflow-hidden"
		:class="{ active: isSelected }"
	>
		<div
			class="card-header p-5 cursor-pointer flex items-center justify-between"
			@click="toggle()"
		>
			<div class="">
				<span class="float-right"></span>
				<h2 class="text-gray-900 font-semibold text-2xl">
					{{ office.title }}
				</h2>
				<p class="text-gray-400">{{ office.address }}</p>
			</div>

			<span class="toogle-accordeon-icon w-5">
				<svg
					xmlns="http://www.w3.org/2000/svg"
					fill="none"
					viewBox="0 0 24 24"
					stroke-width="1.5"
					stroke="currentColor"
					class="w-6 h-6"
				>
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						d="M19.5 8.25l-7.5 7.5-7.5-7.5"
					/>
				</svg>
			</span>
		</div>
		<div class="card-body-container">
			<div class="card-body px-8 py-2 pb-5">
				<div v-if="isOnEditMode">
					<div class="flex items-center justify-between">
						<h4 class="font-xl font-bold">New Location</h4>
						<svg
							xmlns="http://www.w3.org/2000/svg"
							class="w-6 h-6 stroke-gray-400 cursor-pointer"
							@click="isOnEditMode = !isOnEditMode"
							fill="none"
							viewBox="0 0 24 24"
							stroke-width="1.5"
							stroke="currentColor"
						>
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								d="M6 18L18 6M6 6l12 12"
							/>
						</svg>
					</div>
					<OfficeForm
						:office="office"
						:index="index"
						:editOfficeFunt="(var1, var2) => editOffice(var1, var2)"
						:createOfficeFunt="(var1) => createOfficeFunt(var1)"
						:toggle="() => closeEditingForm()"
					></OfficeForm>
				</div>

				<div v-if="!isOnEditMode">
					<h3 class="font-bold text-lg">{{ office.contact.name }}</h3>

					<ul>
						<li>{{ office.contact.position }}</li>
						<li>
							<a
								:href="'mailto:' + office.contact.email"
								class="text-cyan-500"
								>{{ office.contact.email }}</a
							>
						</li>
						<li>{{ office.contact.phone }}</li>
					</ul>

					<hr class="my-4" />

					<div class="offices-options-menu flex mt-2">
						<div
							class="items-center flex uppercase w-1/2 cursor-pointer text-xs"
							@click="isOnEditMode = true"
						>
							<svg
								class="h-6 w-6 flex-none stroke-gray-400  mr-1"
								xmlns="http://www.w3.org/2000/svg"
								fill="none"
								viewBox="0 0 24 24"
								stroke-width="1.5"
								stroke="currentColor"

							>
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									d="M16.862 4.487l1.687-1.688a1.875 1.875 0 112.652 2.652L6.832 19.82a4.5 4.5 0 01-1.897 1.13l-2.685.8.8-2.685a4.5 4.5 0 011.13-1.897L16.863 4.487zm0 0L19.5 7.125"
								/>
							</svg>

							Edit
						</div>

						<div
							class="items-center text-xs flex uppercase w-1/2 text-red-400 justify-end cursor-pointer"
							@click="deleteOffice(index)"
						>
							<svg
								class="h-6 w-6 flex-none stroke-red-400  fill-transparent mr-1"
								stroke-linecap="round"
								stroke-linejoin="round"
							>
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									d="M14.74 9l-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 01-2.244 2.077H8.084a2.25 2.25 0 01-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 00-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 013.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 00-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 00-7.5 0"
								/>
							</svg>
							Delete
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import OfficeForm from "../components/officeForm.vue";
export default {
	name: "OfficeCard",
	components: {
		OfficeForm,
	},
	data() {
		return {
			isOnEditMode: false,
		};
	},
	props: {
		office: {
			type: Object,
			required: true,
		},
		isSelected: {
			type: Boolean,
			required: true,
		},
		index: {
			type: Number,
			required: true,
		},
		editOffice: Function,
		deleteOffice: Function,
		createOfficeFunt: Function,
	},
	methods: {


		closeEditingForm() {
			this.isOnEditMode = false;
			this.toggle();
		},

		toggle() {
			this.$emit("toggle", this.index);
		},
	},
};
</script>

<style>
.card-header {
	color: black;
	background: white;
	-webkit-transition: all 0.5s ease-in-out;
	-moz-transition: all 0.5s ease-in-out;
	-o-transition: all 0.5s ease-in-out;
	transition: all 0.5s ease-in-out;
}

.card-body-container {
	max-height: 0px;
	overflow: hidden;

	-webkit-transition: max-height 0.5s ease-in-out;
	-moz-transition: max-height 0.5s ease-in-out;
	-o-transition: max-height 0.5s ease-in-out;
	transition: max-height 0.5s ease-in-out;
}

.active .card-header {
	background-color: theme("colors.gray.400");
	-webkit-transition: all 0.5s ease-in-out;
	-moz-transition: all 0.5s ease-in-out;
	-o-transition: all 0.5s ease-in-out;
	transition: all 0.5s ease-in-out;
}

.active .card-header h2,
.active .card-header p {
	color: white;
	-webkit-transition: all 0.5s ease-in-out;
	-moz-transition: all 0.5s ease-in-out;
	-o-transition: all 0.5s ease-in-out;
	transition: all 0.5s ease-in-out;
}

.active .card-body-container {
	max-height: 1000px;
	-webkit-transition: max-height 0.5s ease-in-out;
	-moz-transition: max-height 0.5s ease-in-out;
	-o-transition: max-height 0.5s ease-in-out;
	transition: max-height 0.5s ease-in-out;
}

.card-header .toogle-accordeon-icon svg {
	stroke: black;
	-webkit-transform: rotate(0deg);
	-moz-transform: rotate(0deg);
	-o-transform: rotate(0deg);
	transform: rotate(0deg);

	transition: all 0.5s ease-in-out;
}

.active .card-header .toogle-accordeon-icon svg {
	stroke: white;
	-webkit-transform: rotate(180deg);
	-moz-transform: rotate(180deg);
	-o-transform: rotate(180deg);
	transform: rotate(180deg);

	transition: all 0.5s ease-in-out;
}
</style>
