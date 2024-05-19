<template>
	<div>
		<tawk-header @search-txt="handleSearch" :resetSearch="!!slctdCat" />
		<div class="color-bg">
			<div class="center-container">
				<div class="card-container">
					<tawk-CategoryCard v-if="!slctdCat" v-for="(catg, indx) in filteredData	" :key="catg.id"
						:categoryData="catg" @card-clicked="handleCardDetails" />
					<tawk-CategoryDetails v-if="slctdCat" :categoryData="slctdCat" :detailSearchKey="detailSearchKey"
						@close-details="handleCloseDetails" />
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import Header from './components/Header.vue'
import CategoryCard from './components/CategoryCard.vue'
import CategoryDetails from './components/CategoryDetails.vue'

export default {
	created() {
		fetch("http://localhost:9000/api/categories")
			.then(response => {
				if (!response.ok) {
					throw new Error('Network response was not ok');
				}
				return response.json();
			})
			.then(data => {
				this.categories = data;
				console.log("data==>", data)
			})
			.catch(error => {
				console.error('Error:', error);
			});
	},
	data() {
		return {
			categories: [],
			searchKey: '',
			detailSearchKey: '',
			slctdCat: '',
		}
	},
	methods: {
		handleCardDetails(slctd) {
			this.slctdCat = slctd;
		},
		handleCloseDetails() {
			this.slctdCat = '';
		},
		handleSearch(searchKey) {
			if (this.slctdCat) {
				this.detailSearchKey = searchKey;
			} else this.searchKey = searchKey;
		}
	},
	components: {
		'tawk-header': Header,
		'tawk-CategoryCard': CategoryCard,
		'tawk-CategoryDetails': CategoryDetails
	},
	computed: {
		filteredData() {
			let fData = [];
			let dData = this.categories;
			if (this.searchKey.length > 2) {
				dData = this.categories.filter(obj => obj.title.toLowerCase().includes(this.searchKey));
			} else {
				dData = this.categories;
			}
			dData.forEach((cat) => {
				if (cat.enabled) fData.push(cat);
			})
			fData.sort((a, b) => a.order - b.order);
			return fData;
		}
	},
	watch: {
		slctdCat: function () {
			this.detailSearchKey = '';
			this.searchKey = '';
		}
	},
}
</script>

<style lang="scss" scoped>
@import 'scss/main.scss';
@import 'scss/_variables.scss';

.color-bg {
	background-color: $bg-gray;

	.center-container {
		max-width: 1200px;
		margin: auto;
	}

	.card-container {
		min-height: 70vh;
		padding: 3em 0;
		display: flex;
		flex-wrap: wrap;
		justify-content: space-evenly;
	}
}
</style>