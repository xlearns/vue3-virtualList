<script setup>
import { computed, ref, onMounted, watch } from "vue";
let props = defineProps({
	listData: {
		type: Array,
		default: () => [],
	},
	itemSize: {
		type: Number,
		default: 200,
	},
	containerHeight: {
		type: Number,
	},
});

// 列表总宽度
const listHeight = computed(() => {
	return props.listData.length * props.itemSize;
});
//偏移量
const startOffset = ref(0);
//可视区域高度
const screenHeight = ref(0);
//起始索引
const start = ref(0);
//结束索引
const end = ref(null);
const list = ref();
// 可显示的列表数
const visibleCount = computed(() => {
	return Math.ceil(screenHeight.value / props.itemSize);
});

//偏移量对应的style
const getTransform = computed(() => {
	return `translate3d(0,${startOffset.value}px,0)`;
});
//
const visibleData = computed(() => {
	return props.listData.slice(
		start.value,
		Math.min(end.value, props.listData.length)
	);
});

const scrollEvent = function () {
	//当前滚动位置
	let scrollTop = list.value.scrollTop;
	//此时的开始索引
	start.value = Math.floor(scrollTop / props.itemSize);
	//此时的结束索引
	end.value = start.value + visibleCount.value;
	//此时的偏移量
	startOffset.value = scrollTop - (scrollTop % props.itemSize);
};

const init = function () {
	screenHeight.value = list.value.clientHeight;
	start.value = 0;
	end.value = start.value + visibleCount.value;
};
onMounted(() => {
	init();
});

// 窗口改变
window.addEventListener("resize", () => {});
</script>
<template>
	<div ref="list" class="infinite-list-container" @scroll="scrollEvent">
		<!-- 滚动条 -->
		<div
			class="infinite-list-phantom"
			:style="{ height: listHeight + 'px' }"
		></div>
		<!-- 内容 -->
		<div class="infinite-list" :style="{ transform: getTransform }">
			<div
				ref="items"
				class="infinite-list-item"
				v-for="item in visibleData"
				:key="item.id"
				:style="{ height: itemSize + 'px', lineHeight: itemSize + 'px' }"
			>
				{{ item.value }}||{{ visibleData.length }}
			</div>
		</div>
	</div>
</template>
<style scoped>
.infinite-list-container {
	height: 100%;
	overflow: auto;
	position: relative;
	-webkit-overflow-scrolling: touch;
}

.infinite-list-phantom {
	position: absolute;
	left: 0;
	top: 0;
	right: 0;
	z-index: -1;
}

.infinite-list {
	left: 0;
	right: 0;
	top: 0;
	position: absolute;
	text-align: center;
}

.infinite-list-item {
	padding: 10px;
	color: #555;
	box-sizing: border-box;
	border-bottom: 1px solid #999;
}
</style>
