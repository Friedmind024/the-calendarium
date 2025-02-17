<script lang="ts">
    import { Platform, Setting, setIcon } from "obsidian";
    import { warning } from "./utils";

    export let open = !Platform.isMobile;
    export let name: string;
    export let desc: string = "";
    export let warn: boolean = false;
    export let label: string | null = null;
    const details = (node: HTMLElement) => {
        if (open) node.setAttr("open", "open");
    };
    const summary = (node: HTMLDivElement) => {
        new Setting(node).setHeading().setName(name);
    };
    const handle = (node: HTMLElement) => {
        setIcon(node, "chevron-right");
    };
</script>

<details class="calendarium-nested-settings" use:details>
    <summary class="calendarium-nested-summary">
        <div class="setting-item setting-item-heading">
            <div class="setting-item-info">
                <div class="setting-item-name">{name}</div>
                <div class="setting-item-description">{desc}</div>
            </div>
        </div>
        <div class="collapser">
            <div class="warning-container">
                {#if warn}
                    <div use:warning />
                {/if}
                <div class="handle" use:handle />
            </div>
        </div>
    </summary>
    {#if warn && label}
        <div class="warning-label-container">
            <div class="setting-item-description warning-label">
                {label}
            </div>
        </div>
    {/if}
    <slot />
</details>

<style>
    .calendarium-nested-settings {
        position: relative;
    }
    .warning-label-container {
        display: flex;
        justify-content: flex-end;
        position: absolute;
        right: 0;
    }
    /* .calendarium-nested-summary,
    .calendarium-nested-settings {
        position: relative;
    } */
    .warning-label {
        color: var(--text-error);
    }
    .calendarium-nested-summary {
        outline: none;
        display: block !important;
        list-style: none !important;
        list-style-type: none !important;
        min-height: 1rem;
        border-top-left-radius: 0.1rem;
        border-top-right-radius: 0.1rem;
        cursor: pointer;
        position: sticky;
        top: 0;
        z-index: 2;
        background-color: var(--creator-background-color);
    }

    summary::-webkit-details-marker,
    summary::marker {
        display: none !important;
    }
    .collapser {
        position: absolute;
        top: 50%;
        right: 8px;
        transform: translateY(-50%);
        content: "";
    }

    .handle {
        transform: rotate(0deg);
        transition: transform 0.25s;
        display: flex;
    }

    details[open] .handle {
        transform: rotate(90deg);
    }
</style>
