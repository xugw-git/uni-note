<template>
	<view class="content">
		<u-list>
			<u-list-item v-for="(item, index) in indexList" :key="index">
				<u-cell :title="`列表长度-${index + 1}`"><u-avatar slot="icon" shape="square" size="35" :src="item.url" customStyle="margin: -3px 5px -3px 0"></u-avatar></u-cell>
			</u-list-item>
		</u-list>
		<tabbar :tabValue="1"></tabbar>
	</view>
</template>

<script>
import tabbar from '../../components/tabbar.vue';

export default {
	components: { tabbar },
	data() {
		return {
			indexList: [],
			urls: [
				'https://cdn.uviewui.com/uview/album/1.jpg',
				'https://cdn.uviewui.com/uview/album/2.jpg',
				'https://cdn.uviewui.com/uview/album/3.jpg',
				'https://cdn.uviewui.com/uview/album/4.jpg',
				'https://cdn.uviewui.com/uview/album/5.jpg',
				'https://cdn.uviewui.com/uview/album/6.jpg',
				'https://cdn.uviewui.com/uview/album/7.jpg',
				'https://cdn.uviewui.com/uview/album/8.jpg',
				'https://cdn.uviewui.com/uview/album/9.jpg',
				'https://cdn.uviewui.com/uview/album/10.jpg'
			]
		};
	},
	onLoad() {
		for (let i = 0; i < 10; i++) {
			this.indexList.push({
				url: this.urls[uni.$u.random(0, this.urls.length - 1)]
			});
		}
	}
};
</script>
