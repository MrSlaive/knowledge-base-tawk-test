<template>
	<div id="header-wrapper">
		<div class="header-text">
			<h1>Knowledge base doesn't have to be boring</h1>
			<h3>Everything you need to manage your messaging</h3>
		</div>
		<div class="search-box-cont">
			<input id="searchTxt" :value="searchTxt" @input="handleIinputChange" type="text" class="search-inpt" placeholder="Search..."/>
			<button class="search-btn"><img class="search-icon" src="../assets/icon/magnifyGlass.png"></button>
		</div>
	</div>
</template>

<script>
export default {
	data() {
		return {
			searchTxt: ''
		}
	},
	props:{
        resetSearch: {
            type: Boolean,
            default() {
                return false;
            }
        },
	},
	methods: {
		handleIinputChange(e) {
				this.$emit('search-txt', e.target.value);
		},
	},
	watch: { 
		resetSearch: function() {
			this.searchTxt = '';
			document.getElementById('searchTxt').value = ''
		}
	},
}
</script>

<style lang="scss" scoped>
@import '../scss/_variables.scss';

#header-wrapper{
	padding: 2em 8em;
	.header-text {
		text-align: center;
		font-family: $font-family;
		color: $text-black;
		padding: 20px;

		h1 {
			font-style: normal;
			font-weight: bold;
			font-size: 34px;
			line-height: 41px;
		}

		h3 {
			font-style: normal;
			font-weight: normal;
			font-size: 20px;
			line-height: 24px;
		}
	}

	// 
	.search-box-cont{
		$searchHeight: 60px;
		display:flex;
		border: 1px solid #EEEEEE;
		background-color: #fff;
		height: $searchHeight;
		border-radius: 3px;
		.search-inpt {
			flex: 1;
			margin: 0 auto;
			height: $searchHeight;
			padding: 0 20px;
			font-size: 1rem;
			outline: none;
			background: none;
			border: none;
		}
		.search-btn {
			background-color: $green;
			height: $searchHeight;
			width: 80px;
			border: none;
			border-top-right-radius: 3px;
			border-bottom-right-radius: 3px;
			.search-icon {
				max-width: 20px;
			}
		}
	}
}
// </style>