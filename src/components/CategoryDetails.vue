<template>
	<div id="category-details">
		<p class="breadcrumbs">
			<a @click="$emit('close-details');" class="actv-link">All categories</a> > {{ categoryData.title }}
		</p>
		<div class="detail-cont">
			<div class="category-card-cont">
				<tawk-CategoryCard :categoryData="categoryData" cardType="detailCard"/>
			</div>
			<div class="articles-cont">
				<div class="article" v-for="(catg, indx) in filteredData" :key="catg.id">
					<div class="article-img">
						<img class="card-img" src="../assets/icon/article.png"/>
					</div>
					<div class="article-txt">
						<h3>{{ catg.title }}</h3>
						<p>Updated {{ handleCountDate(catg.updatedOn) }}</p>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import CategoryCard from './CategoryCard.vue'
import moment from 'moment'
export default {
	created() {
		this.moment = moment;
		fetch("http://localhost:9000/api/category/" + this.categoryData.id)
			.then(response => {
				if (!response.ok) {
					throw new Error('Network response was not ok');
				}
				return response.json();
			})
			.then(data => {
				this.categoriesDetails = data;
				console.log("data==>", data)
			})
			.catch(error => {
				console.error('Error:', error);
			});
			this.detailSearchKey = '';
	},
	props: {
		categoryData: {
			type: Object,
			default() {
				return {
					activityId: '',
					activityPosition: null
				};
			}
		},
		detailSearchKey: {
			type: String,
			default() {
				return ""
			}
		},
	},
	computed: {
		filteredData() {
			let fData = [];
			let dData = this.categoriesDetails;
			if(this.detailSearchKey.length > 2) {
				dData = this.categoriesDetails.filter(obj =>obj.title.toLowerCase().includes(this.detailSearchKey));
			} else {
				dData = this.categoriesDetails;
			}
			dData.forEach((cat) => {
				if (cat.status === "published") fData.push(cat);
			})
			return fData;
		},
	},
	data() {
		return {
			categoriesDetails: [],
		}
	},
	methods: {
		handleCardClick(cardId) {
			this.$emit('card-clicked', cardId);
		},
		handleCountDate(updateDate){
			var formatedUpdate = moment(new Date(updateDate));
			return moment(formatedUpdate).fromNow();
		}
	},
	components: {
		'tawk-CategoryCard': CategoryCard
	}
}
</script>

<style lang="scss" scoped>
@import '../scss/_variables.scss';

#category-details {
	font-family: $font-family;
	margin-top: 1em;
	width: 100%;
	.breadcrumbs{
		padding: 1em 0;
		color: $text-gray;
		.actv-link{
			color: $green;
			cursor: pointer;
		}
	}
	.detail-cont {
		display: flex;
		gap: 2em;
		.category-card-cont {
			flex: 1;
		}
		.articles-cont {
			flex: 3;
			display: flex;
			flex-direction: column;
			gap: 1em;
			.article {
				border: solid 1px #EEEEEE;
				border-radius: 3px;
				background: #fff;
				padding: 2em;
				display: flex;
				gap: 1em;
				&-img{
					display: flex;
					align-items: center;
					padding: 1em;
					img{
						width: 17px;
					}
				}
				&-txt{
					flex: 1;
					h3{
						margin-bottom: .5em;
					}
					p{
						margin-top: 0;
						font-size: 10px;
						color: $text-gray;
					}
				}
			}
		}
	}

	.grey-txt {
		color: #9C9AA6;
	}
}
</style>