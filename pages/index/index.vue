<template>
	<view class="content">
		<u--text type="primary" text="主色"></u--text>
		<tabbar :tabValue="0"></tabbar>
	</view>
</template>

<script>
import tabbar from '../../components/tabbar.vue';

export default {
	components: { tabbar },
	data() {
		return {};
	},
	onLoad() {},
	methods: {}
};
</script>
