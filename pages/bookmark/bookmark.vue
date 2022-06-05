<template>
	<view>
		<u-index-list :index-list="indexList.index">
			<template v-for="(item, index) in itemArr">
				<u-index-item>
					<u-index-anchor :text="indexList.text[index]"></u-index-anchor>
					<u-cell v-for="(cell, cindex) in item" :key="cindex">
						<u-avatar slot="icon" shape="square" size="35" :src="avatarList[index][cindex]" customStyle="margin: -3px 5px -3px 0"></u-avatar>
						<u-link slot="title" :href="cell.url" :text="cell.title" color="#0b87da"></u-link>
					</u-cell>
				</u-index-item>
			</template>
		</u-index-list>
		<tabbar :tabValue="2"></tabbar>
	</view>
</template>

<script>
import tabbar from '../../components/tabbar.vue';

export default {
	components: { tabbar },
	data() {
		return {
			indexList: { index: ['A', 'B', 'C', 'D'], text: ['A - 前端库', 'B - vue 相关', 'C - 其他', 'D - 文档'] },
			itemArr: [
				[
					{ title: 'bootstrap', url: 'https://github.com/twbs/bootstrap' },
					{ title: 'echarts', url: 'https://github.com/apache/echarts' },
					{ title: 'uni-app', url: 'https://github.com/dcloudio/uni-app' },
					{ title: 'uView', url: 'https://github.com/umicro/uView2.0' }
				],
				[
					{ title: 'vue', url: 'https://github.com/vuejs/vue' },
					{ title: 'vue-cli', url: 'https://github.com/vuejs/vue-cli' },
					{ title: 'vuex', url: 'https://github.com/vuejs/vuex' },
					{ title: 'vue router', url: 'https://github.com/vuejs/router' },
					{ title: 'element-ui', url: 'https://github.com/ElemeFE/element' }
				],
				[
					{ title: 'axios', url: 'https://github.com/axios/axios' },
					{ title: 'mockjs', url: 'https://github.com/nuysoft/Mock' },
					{ title: 'express', url: 'https://github.com/expressjs/express' },
					{ title: 'django', url: 'https://github.com/django/django' }
				],
				[
					{ title: 'docsify', url: 'https://github.com/docsifyjs/docsify' },
					{ title: 'vuepress', url: 'https://github.com/vuejs/vuepress' },
					{ title: 'hexo', url: 'https://github.com/hexojs/hexo' }
				]
			],
			avatarList: [[], [], [], []],
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
		for (let i = 0; i < 4; i++) {
			for (let j = 0; j < this.itemArr[i].length; j++) {
				this.avatarList[i].push(this.urls[uni.$u.random(0, this.urls.length - 1)]);
			}
		}
	}
};
</script>
