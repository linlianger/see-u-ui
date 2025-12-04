<template>
	<view class="container">
		<text class="title">基本使用</text>
		<view class="content">
			<see-text text="我的愿望是世界上没有加班"></see-text>
		</view>
		<text class="title">设置主题</text>
		<view class="content">
			<see-text text="默认" />
			<see-text text="主色" type="primary" />
			<see-text text="失败" type="error" />
			<see-text text="警告" type="warning" />
			<see-text text="成功" type="success" />
			<see-text text="自定义颜色" color="#52f7bd" />
		</view>
		<text class="title">链接</text>
		<view class="content">
			<see-text text="这是SeeYouUI的官方文档" type="primary" mode="link" href="https://www.baidu.com" />
			<view class="gap-12"></view>
			<see-text text="如需更完整功能,请查看Link链接组件" type="warning" mode="link" href="https://www.baidu.com" />
		</view>
		<text class="title">拨打电话</text>
		<view class="content">
			<see-text text="18888888888" type="primary" mode="phone" phone-number="18888888888" />
			<view class="gap-12"></view>
			<see-text text="自定义文本" type="primary" mode="phone" phone-number="19999999999" />
		</view>
		<text class="title">显示金额</text>
		<view class="content flex-start">
			<see-text text="一千块：" />
			<see-text text="1000" type="primary" mode="price" />
			<view class="gap-12"></view>
			<see-text text="好多钱：" />
			<see-text :text="9999999999999.99" type="primary" mode="price" />
			<view class="gap-12"></view>
		</view>
		<text class="title">日期格式化</text>
		<view class="content flex-start">
			<see-text text="字符串时间戳：" />
			<see-text type="primary" mode="date" date="1672502400000" date-format="YYYY-MM-DD" />
			<view class="gap-12"></view>
			<see-text text="数字时间戳 + 时分秒：" />
			<see-text type="primary" mode="date" :date="1715668235000" date-format="YYYY-MM-DD HH:mm:ss" />
			<view class="gap-12"></view>
			<see-text text="显示星期：" />
			<see-text type="primary" mode="date" :date="1715668235000" date-format="YYYY年MM月DD日 WWW" />
			<view class="gap-12"></view>
			<see-text text="Date形式：" />
			<see-text type="primary" mode="date" :date="new Date()" date-format="YYYY年MM月DD日" />
			<view class="gap-12"></view>
			<see-text text="实时：" />
			<see-text type="primary" mode="date" :date="currentTime" date-format="YYYY年MM月DD日 HH:mm:ss.S" />
		</view>
		<text class="title">时间距今</text>
		<view class="content flex-start">
			<see-text text="当前时间 + 1秒：" />
			<see-text type="primary" mode="timeago" :date="Date.now() + 1000" date-format="YYYY-MM-DD" />
			<view class="gap-12"></view>
			<see-text text="当前时间 - 30秒：" />
			<see-text type="primary" mode="timeago" :date="Date.now() - 30 * 1000" date-format="YYYY-MM-DD" />
			<view class="gap-12"></view>
			<see-text text="当前时间 - 30分钟：" />
			<see-text type="primary" mode="timeago" :date="Date.now() - 30 * 60 * 1000" date-format="YYYY-MM-DD" />
			<view class="gap-12"></view>
			<see-text text="当前时间 - 5小时：" />
			<see-text type="primary" mode="timeago" :date="Date.now() - 5 * 3600 * 1000" date-format="YYYY-MM-DD" />
			<view class="gap-12"></view>
			<see-text text="当前时间 - 3天：" />
			<see-text type="primary" mode="timeago" :date="Date.now() - 3 * 86400 * 1000" date-format="YYYY-MM-DD" />
			<view class="gap-12"></view>
			<see-text text="当前时间 - 15天：" />
			<see-text type="primary" mode="timeago" :date="Date.now() - 15 * 86400 * 1000" date-format="YYYY-MM-DD" />
			<view class="gap-12"></view>
			<see-text text="当前时间 - 150天：" />
			<see-text type="primary" mode="timeago" :date="Date.now() - 150 * 86400 * 1000" date-format="YYYY-MM-DD" />
			<view class="gap-12"></view>
			<see-text text="当前时间 - 800天：" />
			<see-text type="primary" mode="timeago" :date="Date.now() - 800 * 86400 * 1000" date-format="YYYY-MM-DD" />
		</view>
	</view>
</template>

<script lang="ts" setup>
import { ref, onMounted, onUnmounted } from 'vue'
import SeeText from '@/components/SeeText/index.vue';

const currentTime = ref(new Date());
let timer: ReturnType<typeof setInterval> | null = null;

onMounted(() => {
	timer = setInterval(() => currentTime.value = new Date(), 30);
});

onUnmounted(() => {
	if (timer) {
		clearInterval(timer);
		timer = null;
	}
});
</script>

<style lang="scss" scoped>
.gap-12{
	width: 100%;
	height: 12px;
}
.flex-start{
	justify-content: flex-start !important;
}
.container {
	width: 100vw;
	overflow: hidden;
	padding: 12px;
	box-sizing: border-box;

	.title {
		font-size: 18px;
		color: #999;
	}
	.content {
		margin-top: 6px;
		margin-bottom: 24px;
		flex-direction: row;
		flex-wrap: wrap;
		align-items: center;
		display: flex;
		align-items: center;
		justify-content: space-between;
	}
}
</style>
