<script lang="ts">
    import { modelMeta, headGap } from '~/store';
    import classNames from 'classnames';
    import { onMount } from 'svelte';

    let headBlockSize = { width: 0, height: 0 };
    let opacityOffset = 0.1;

    onMount(() => {
        const resizeObserver = new ResizeObserver((entries) => {
            for (let entry of entries) {
                const { width, height } = entry.contentRect;
                headBlockSize = {
                    width,
                    height
                };
            }
        });
        const headContent = document.querySelector('.first-head-content');
        if (headContent) {
            resizeObserver.observe(headContent);
        }

        return () => {
            if (headContent) {
                resizeObserver.unobserve(headContent);
            }
        };
    });
</script>

<div class="single-head-content">
    <div class="card" style={`width: ${headBlockSize.width}px; height: ${headBlockSize.height}px;`}>
        <slot></slot>
    </div>
</div>

<style lang="scss">
   .single-head-content {
        width: 100%;
        position: relative;
    }

   .card {
        background-color: white;
        border: 1px solid #f9fafb;
        box-shadow: 0px 4px 6px -1px rgba(3, 7, 18, 0.15), 0px 2px 4px -2px rgba(3, 7, 18, 0.08);
    }
</style>