<template>
	<view class="content">
		<u-swiper :list="list" indicator indicatorMode="line" circular></u-swiper>
		<u-gap height="40"></u-gap>
		<u-cell-group>
			<u-cell icon="chat" title="子在川上曰：逝者如斯夫！不舍昼夜。"></u-cell>
			<u-cell icon="chat" title="士不可以不弘毅，任重而道远。"></u-cell>
			<u-cell icon="chat" title="学而不思则罔，思而不学则殆。"></u-cell>
			<u-cell icon="chat" title="温故而知新，可以为师矣。"></u-cell>
			<u-cell icon="chat" title="知之者不如好之者。"></u-cell>
		</u-cell-group>
		<tabbar :tabValue="0"></tabbar>
	</view>
</template>

<script>
	import tabbar from '../../components/tabbar.vue';

	export default {
		components: {
			tabbar
		},
		data() {
			return {
				list: [
					'https://cdn.uviewui.com/uview/album/1.jpg',
					'https://cdn.uviewui.com/uview/album/2.jpg',
					'https://cdn.uviewui.com/uview/album/3.jpg',
				],
			};
		},
		onShareAppMessage() {
			return {
				title: 'xugw',
				path: '/pages/index/index',
			}
		},
		onShareTimeline() {
			return {
				title: 'xugw',
			}
		}
	};
</script>
