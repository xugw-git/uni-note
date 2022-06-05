<template>
	<view class="content">
		<u-list>
			<u-list-item v-for="(item, index) in indexList" :key="index">
				<u-cell>
					<u-avatar slot="icon" shape="square" size="35" :src="item.url" customStyle="margin: -3px 5px -3px 0"></u-avatar>
					<u-link slot="title" :href="item.linkUrl" :text="item.title" color="#0b87da"></u-link>
				</u-cell>
			</u-list-item>
		</u-list>
		<tabbar :tabValue="3"></tabbar>
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
			],
			titleList: ['vue-admin', '博客案例', '记事本', '实时天气', '番茄时钟', '计算器', '鼓式机器', 'Markdown编辑器', '随机引语'],
			linkUrls: [
				'https://vue-admin-mu.vercel.app/#/',
				'https://blog-case.vercel.app/#/',
				'https://codepen.io/xugw/full/poppMxP',
				'https://codepen.io/xugw/full/NWXXZoV',
				'https://codepen.io/xugw/full/VwyKyJo',
				'https://codepen.io/xugw/full/VwyKpEO',
				'https://codepen.io/xugw/full/JjMKMzV',
				'https://codepen.io/xugw/full/BaJKXNo',
				'https://codepen.io/xugw/full/YzYLMOe'
			]
		};
	},
	onLoad() {
		for (let i = 0; i < 9; i++) {
			this.indexList.push({
				url: this.urls[uni.$u.random(0, this.urls.length - 1)],
				title: this.titleList[i],
				linkUrl: this.linkUrls[i]
			});
		}
	}
};
</script>
