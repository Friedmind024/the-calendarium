<script lang="ts">
    import type { MoonState } from "src/@types";
    import { SHADOW_MAP } from "src/utils/constants";

    export let moon: MoonState;
    export let label: boolean = true;
    export let size = 28;

    $: path = SHADOW_MAP[moon.phase ?? "New Moon"];
</script>

<svg
    class="moon"
    id={moon.id}
    preserveAspectRatio="xMidYMid"
    aria-label={label ? `${moon.name}\n${moon.phase}` : null}
    width={size}
    height={size}
    viewBox="0 0 32 32"
>
    <circle cx="16" cy="16" r="10" fill={moon.faceColor} />
    <path class="shadow" fill={moon.shadowColor} d={path} />
    <circle
        cx="16"
        cy="16"
        r="10"
        fill="none"
        stroke="#000"
        stroke-width="2px"
    />
</svg>

<style>
</style>
