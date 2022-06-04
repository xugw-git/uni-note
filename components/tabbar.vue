<template>
	<view>
		<u-tabbar :value="tabValue?tabValue:0" :fixed="true" :placeholder="true" @change="changeTo">
			<u-tabbar-item text="首页" icon="home"></u-tabbar-item>
			<u-tabbar-item text="笔记" icon="list"></u-tabbar-item>
			<u-tabbar-item text="书签" icon="bookmark"></u-tabbar-item>
			<u-tabbar-item text="作品" icon="account"></u-tabbar-item>
		</u-tabbar>
	</view>
</template>

<script>
export default {
	props: {
		tabValue: Number
	},
	data() {
		return {
			list: [
				{
					pagePath: '/pages/index/index'
				},
				{
					pagePath: '/pages/note/note'
				},
				{
					pagePath: '/pages/bookmark/bookmark'
				},
				{
					pagePath: '/pages/works/works'
				}
			]
		};
	},
	methods: {
		changeTo(value) {
			uni.switchTab({
			url: this.list[value].pagePath
			})
		}
	}
};
</script>

<style></style>
