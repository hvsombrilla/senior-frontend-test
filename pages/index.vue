<template>
	<div
		class="relative min-h-screen flex-col justify-center overflow-hidden bg-slate-100 py-3 sm:py-5"
	>
		<div class="sm:mx-auto sm:max-w-xs p-3">
			<h1 class="text-cyan-600 font-light text-center text-6xl m-6">
				Offices
			</h1>

			<button
				@click="toggleOfficeCreationForm()"
				type="button"
				v-if="!showNewForm"
				class="w-full font-light text-left justify-between text-white bg-cyan-700 hover:bg-cyan-600 shadow-xl focus:ring-4 focus:outline-none focus:ring-cyan/50 rounded-xl px-5 py-2.5 inline-flex items-center mr-2 mb-2"
			>
				Add new locations

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
						d="M12 4.5v15m7.5-7.5h-15"
					/>
				</svg>
			</button>
			<!-- Start creation Form -->
			<div
				class="relative bg-white mb-5 shadow-xl ring-1 p-5 ring-gray-900/5 sm:rounded-lg overflow-hidden"
				v-if="showNewForm"
			>
				<div class="flex items-center justify-between">
					<h4 class="font-xl font-bold">New Location</h4>
					<svg
						xmlns="http://www.w3.org/2000/svg"
						class="w-6 h-6 stroke-gray-400 cursor-pointer"
						@click="toggleOfficeCreationForm()"
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
					:createOfficeFunt="(var1) => createOffice(var1)"
				></OfficeForm>
			</div>

			<!-- End Creating Form -->

			<OfficeCard
				v-for="(office, index) in offices"
				:key="index"
				:office="office"
				:index="index"
				:isSelected="activeTab === index"
				@remove="removeOffice(index)"
				:editOffice="(var1, var2) => updateOffice(var1, var2)"
				:createOfficeFunt="(var1) => {createOffice(var1)}"
				:deleteOffice="(var1) => deleteOffice(var1)"
				@toggle="selectOffice(index)"
			></OfficeCard>

			<h4 v-if="offices.length === 0" class="text-center">
				No offices available.
			</h4>

			<div class="text-center">
				<p class="text-gray-400">
					This project is for test purpose only.
				</p>
				<p>
					<a
						href="https://www.dogandponystudios.com"
						class="text-cyan-500 uppercase text-xs"
						>www.dogandponystudios.com</a
					>
				</p>
			</div>
		</div>
	</div>
</template>

<script>
import OfficeForm from "../components/officeForm.vue";
import OfficeCard from "../components/officeCard.vue";

export default {
	name: "App",
	components: {
		OfficeForm,
		OfficeCard,
	},
	data() {
		let officesDefaultData = [
			{
				title: "Headquarters",
				address: "32nd Street, New York, USA",
				contact: {
					name: "Peter Parker",
					position: "Director",
					phone: "+591 123-456-7899",
					email: "main@test.com",
				},
			},
			{
				title: "Alternative Office",
				address: "2nd Street, New York, USA",
				contact: {
					name: "Julia Roberts",
					position: "Lead Developer",
					phone: "+591 778-1234",
					email: "main@test.com",
				},
			},
			{
				title: "Offshore Office",
				address: "65nd Street, Florida, USA",
				contact: {
					name: "John Doe",
					position: "Second Developer",
					phone: "+591 778-1234",
					email: "main@test.com",
				},
			},

		];

		return {
			offices: officesDefaultData,
			activeTab: null,
			editingTab: null,
			showNewForm: false,
		};
	},
	methods: {
		toggleOfficeCreationForm() {
			this.showNewForm = !this.showNewForm;
		},

		selectOffice(index) {

			if (index == this.activeTab) {
				this.activeTab = null;
			} else {
				this.activeTab = index;
			}
		},

		createOffice(office) {
			this.offices.push(office);
			this.showNewForm = false;
		},

		updateOffice(data, index) {
			this.offices[index] = { ...data };
		},

		deleteOffice(index) {
			this.activeTab = null;
			this.offices.splice(index, 1);
		},
	},
};
</script>
