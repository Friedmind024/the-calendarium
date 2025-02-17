<script lang="ts">
    import { getContext } from "svelte";
    import MonthInstance from "./MonthInstance.svelte";
    import AddNew from "../../Utilities/AddNew.svelte";
    import NoExistingItems from "../../Utilities/NoExistingItems.svelte";
    import Details from "../../Utilities/Details.svelte";
    import DropZone from "../../Utilities/DropZone.svelte";
    import copy from "fast-copy";
    import type { Month } from "src/schemas";
    import { MonthModal } from "src/settings/modals/month/month";
    import ToggleComponent from "../../Settings/ToggleComponent.svelte";

    const store = getContext("store");
    const { monthStore } = store;
    $: items = copy($monthStore);

    function onDrop(items: Month[]) {
        monthStore.set(items);
    }
    const advanced = (item: Month) => {
        const modal = new MonthModal(item);
        modal.onCancel = () => {}; //no op;
        modal.onClose = () => {
            if (!modal.item.name || !item.id) return;
            monthStore.update(item.id, modal.item);
        };
        modal.open();
    };

    const trash = (item: Month) => {
        monthStore.delete(item.id ?? "");
    };
</script>

<Details
    name={"Months"}
    warn={!$monthStore?.length}
    label={"At least one month is required"}
    desc={`${$monthStore.length} month${$monthStore.length != 1 ? "s" : ""}`}
    open={false}
>
    <ToggleComponent
        name={"Show Intercalary Months Separately"}
        desc={"Intercalary months will appear a distinct months in the calendar."}
        value={$store.showIntercalarySeparately}
        on:click={() => {
            $store.showIntercalarySeparately =
                !$store.showIntercalarySeparately;
        }}
    />

    <AddNew on:click={() => monthStore.add()} />

    {#if !$monthStore.length}
        <NoExistingItems message={"Create a new month to see it here."} />
    {:else}
        <DropZone
            type="month"
            component={MonthInstance}
            {items}
            {onDrop}
            on:advanced={(e) => advanced(e.detail)}
            on:trash={(e) => trash(e.detail)}
        />
    {/if}
</Details>

<style>
</style>
