<template>
	<div @click="handleCardClick(categoryData)" id="category-card" :class="{ detailCard: cardType === 'detailCard' }">
		<img class="card-img" :src="require('../assets/img/'+categoryData.icon+'.png')"/>
		<h2 class="article-title">{{categoryData.title}}</h2>
		<p v-if="!cardType" class="article-count">{{categoryData.totalArticle}} articles</p>
		<p class="grey-txt"> Last update: {{ handleCountDate(categoryData.updatedOn) }}</p>
		<div v-if="cardType === 'detailCard'" class="card-detail-txt">
			<img class="img" src="../assets/icon/info.png"/>
			<p>{{categoryData.description}}</p>
		</div>
	</div>
</template>

<script>
import moment from 'moment'
export default {
	created(){
		this.moment = moment;
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
        cardType: {
            type: String,
            default() {
                return '';
            }
        },
    },
	methods:{
		handleCardClick(cardId) {
			this.$emit('card-clicked', cardId);
		},
		handleCountDate(updateDate){
			var formatedUpdate = moment(new Date(updateDate));
			return moment(formatedUpdate).fromNow();
		}
	}
}
</script>

<style lang="scss" scoped>
	@import '../scss/_variables.scss';
	#category-card{
		font-family: $font-family;
		border: solid 1px #EEEEEE;
		background-color: #fff;
		border-radius: 3px;
		min-width: 315px;
		margin-top: 1em;
		padding: 2em;
		text-align: center;
		p{
			margin: 0;
			&.article-count{
				color: $green;
			}
		}
		h2{
			margin-bottom: .5em;
		}
		&.detailCard{
			height: initial;
			margin-top: 0
		}
		.card-img{
			max-width: 60px;
		}
		.grey-txt{
			font-size: 10px;
			color: $text-gray;
		}
		.card-detail-txt{
			border-top: solid 1px #EEEEEE;
			margin-top: 1em;
			padding: 1em;
			p{
				color: $text-gray;
			}
			img{
				max-width: 1em;
			}
		}
	}


</style>