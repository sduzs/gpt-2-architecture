<script lang="ts">
	import '~/styles/app.css';
	import '~/styles/global.scss';
	import Topbar from '~/components/Topbar.svelte';
	import { isLoaded, predictedColor, rootRem } from '~/store';
	import Article from '~/components/article/Article.svelte';
	import { onMount } from 'svelte';
	import { Spinner } from 'flowbite-svelte';

	let topBarHeight = 0;
	let scrollLeft = 0;

	let minScreenWidth = 1300;
	let minColumWidth = Math.floor(minScreenWidth / 24) - rootRem * 2;

	let intersectionObserver: IntersectionObserver;
	let tobBarActive = false;
	let target: HTMLElement;

	onMount(() => {
		isLoaded.set(true);

		intersectionObserver = new IntersectionObserver(handleIntersection, {
			root: null,
			rootMargin: '0px',
			threshold: 0.2
		});

		if (target) {
			intersectionObserver.observe(target);
		}
		window.addEventListener('scroll', handleMobileScrollX);

		return () => {
			intersectionObserver.disconnect();
			window.removeEventListener('scroll', handleMobileScrollX);
		};
	});

	function handleIntersection(entries) {
		entries.forEach((entry) => {
			if (entry.isIntersecting) {
				tobBarActive = true;
			} else {
				tobBarActive = false;
			}
		});
	}

	const handleMobileScrollX = () => {
		scrollLeft = window.scrollX || document.documentElement.scrollLeft;
	};
</script>

<div
    id="app"
    style={`--min-screen-width:${minScreenWidth}px;--min-column-width:${minColumWidth}px;--predicted-color:${predictedColor};`}
>
    <div id="landing">
        <main id="main" style={`padding-top:${topBarHeight}px`} bind:this={target}>
            {#if $isLoaded}
                <slot />
            {:else}
                <div class="flex h-full w-full items-center justify-center"><Spinner color="purple" /></div>
            {/if}
        </main>
    </div>
    <div class="article h-auto w-full">
        <Article></Article>
    </div>
    <footer>
        <Topbar isActive={tobBarActive} />
    </footer>
</div>

<style lang="scss">
	#app {
    height: 100vh;
    min-width: 900px;
    display: flex;
    flex-direction: column; // 设置为纵向的flex布局，方便控制顶部、主体、底部的排列顺序
    justify-content: space-between; // 让子元素在垂直方向上均匀分布，可将footer放置在底部
}

#landing {
    height: 100%;
    width: 100%;
    min-width: var(--min-screen-width);
}

main {
    position: relative;
    height: 100%;
    width: 100%;
    display: flex;
    justify-content: start;
    overflow: hidden;
}

footer {
    min-width: var(--min-screen-width);
    width: 100%;
    position: fixed;
    bottom: 0; // 将footer固定在页面底部
    z-index: 999;
    background-color: #fff; // 可以设置背景色等样式，使其视觉效果更符合预期，可根据实际情况调整颜色
}
</style>
